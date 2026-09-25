---
name: better-code-review
description: Use when asked to review a pull request or a branch: "review this PR", "do a code review", "check my changes".
---

# Better Code Review

A review is a judgment about whether this is the right change, not a pass over the diff. The `code-review` skill hunts bugs; this skill adds everything it does not see.

If the `code-review` skill's rules conflict with this one, this one wins.

## Mindset

- **Start from the task, not the diff.** Find out what the change was meant to solve: the linked task, the description, the conversation. Judge the change against that. A clean diff that solves the wrong problem, or half of the right one, is the first thing to say.
- **Challenge the approach, not only the code.** Is this the simplest way to get there? Would a change elsewhere make this one unnecessary? Would a different boundary, data shape, or order of operations be cheaper to own? Say so even when the code as written is fine.
- **Think of the next reader.** Not the author, not you: the person who opens this file in a year. Will they understand why, or only what?
- **Every finding carries a reason and a way out.** "This is wrong" without why and without an alternative is noise.
- **Separate defect from taste.** A defect is flagged as a defect. Taste is offered once, as an option, and let go.

## What to check

Go through each. Skip only what the diff genuinely does not touch, and say what you skipped.

- **Intent.** Does it do what the task asked, all of it, and nothing the task did not ask for?
- **Approach.** Is the design right for this codebase? Would a simpler structure, an existing abstraction, or a different split of responsibilities do the job?
- **Architecture.** Are the pieces in the right layers and modules? Any new coupling, cycles, or leaked boundaries?
- **Correctness.** Edge cases, error paths, concurrency, resources. The `code-review` skill does most of this; you look for what it missed.
- **Performance.** Anything that grows with data or traffic: queries in loops and N+1, lazy-loaded relations inside loops, sequential awaits that could run concurrently, blocking calls in async code, a new HTTP client per request, fetching every column when a few are needed, the same query repeated within one request, a large result set loaded into memory instead of streamed, missing indexes.
- **Tests.** Is every behavior the change introduces covered, with the right kind of test: unit for logic, integration for boundaries, end-to-end only where nothing else proves it? Do the tests check behavior or the implementation? Look for the untested: a new module with no test file, public functions no test references, critical paths and error branches visible in the code but absent from the tests. For Python, `mikit:python-tests` is the checklist.
- **Security.** Hardcoded secrets, including as field defaults. SQL built from strings. Shell commands with user input. Server-side fetches of user-controlled URLs. File paths built from user input. Untyped or unvalidated bodies at API boundaries. Routes without auth, or identity taken from the request instead of the token. Secrets or personal data in logs and error responses. Unsafe deserialization, `eval` or `exec` on dynamic input. Run `/security-review` when the change touches auth, endpoints, user input, or infrastructure, and `mikit:ai-security` when it touches LLM integration.
- **Conventions.** Does it follow the project's guides on architecture, naming, structure, testing, docs? Point at the guide, not at your preference.
- **Clarity.** Could it be written so the next reader needs no explanation? Fewer moving parts, better names, one obvious path. Dead code, god classes, duplication across files, long parameter lists where one object would do, mutable default arguments, deep nesting, magic numbers without a name.
- **Comments.** Is each one strictly necessary? A comment that repeats the code, or says what a better name would, goes. A comment that holds a non-obvious why stays.
- **Documentation.** Does anything the change affects need an update: guides, READMEs, changelog? And the reverse: is there documentation here nobody will read, or that duplicates the code? `mikit:better-docs` is the checklist.
- **Best practices.** Of the language, framework, and libraries in play. Current ones, not folklore.
- **CI and infrastructure.** Only when the change touches workflows, Dockerfiles, deploy configs, or scripts. Third-party actions pinned to a commit SHA, not a tag. An explicit least-privilege `permissions` block. Secrets passed through `env`, never interpolated into `run`. No untrusted event input (`github.event.*`, `github.head_ref`) interpolated into shell. Concurrency groups on push and PR workflows. Named steps. Repeated step sequences extracted into a composite action. Inline scripts short; long or branching logic belongs in a script file. Containers not running as root. Environment-specific config holding only what differs, and never a secret.

## Steps

1. Invoke `mikit:learn-project`.

2. Understand the intent: task, description, linked issues, the conversation if there is one.

3. If this is a follow-up review: note which earlier issues are resolved and which are open, verify fixes are actually in the diff and not just acknowledged, look only at what changed since the last round, and never repeat a point already made. If nothing new, say so in a short summary and post no inline comments.

4. On a follow-up, resolve your own earlier comments whose fix is in the diff, with a short reply first. Never resolve other reviewers' comments.

5. Run the `code-review` skill for the code pass: the plugin's if installed, otherwise the built-in one.

6. Go through "What to check" for everything the code pass did not cover. Don't economize: on anything beyond a small diff, dispatch read-only subagents in parallel, one per dimension, each with the diff and the project's guides, and merge what they return. Time spent here is cheaper than a bug in production.

7. Write it up in two layers. One overall block, in this order: the verdict in one line (merge, merge after fixes, or rework, and what blocks it); the judgment on intent, approach, and architecture; anything with no single line to sit on; the dimensions you skipped and why. Then inline comments on the exact lines or hunks for every finding that has one. Group related nits into one comment so the author is not buried. Every finding: severity, reason, suggested change. Plain words, short sentences, no filler, like a developer would write it.

## Severity Markers

Start each finding with a colored circle and severity:

- "🔴 [critical]" — must fix before merge
- "🟡 [warning]" — should fix, potential issue
- "🔵 [suggestion]" — optional improvement
