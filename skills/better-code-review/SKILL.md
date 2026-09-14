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
- **Performance.** Anything that grows with data or traffic: queries in loops, N+1, unbounded memory, missing indexes, work on the hot path that belongs off it.
- **Tests.** Is every behavior the change introduces covered, with the right kind of test: unit for logic, integration for boundaries, end-to-end only where nothing else proves it? Do the tests check behavior or the implementation? Anything untested that should not be? For Python, `mikit:python-tests` is the checklist.
- **Security.** Input handling, auth, secrets, injection, what the change exposes. Run `/security-review` when it touches auth, endpoints, user input, or infrastructure, and `mikit:ai-security` when it touches LLM integration.
- **Conventions.** Does it follow the project's guides on architecture, naming, structure, testing, docs? Point at the guide, not at your preference.
- **Clarity.** Could it be written so the next reader needs no explanation? Fewer moving parts, better names, one obvious path.
- **Comments.** Is each one strictly necessary? A comment that repeats the code, or says what a better name would, goes. A comment that holds a non-obvious why stays.
- **Documentation.** Does anything the change affects need an update: guides, READMEs, changelog? And the reverse: is there documentation here nobody will read, or that duplicates the code? `mikit:docs` is the checklist.
- **Best practices.** Of the language, framework, and libraries in play. Current ones, not folklore.

## Steps

1. Invoke `mikit:learn-project`.

2. Understand the intent: task, description, linked issues, the conversation if there is one.

3. If this is a follow-up review: note which earlier issues are resolved and which are open, verify fixes are actually in the diff and not just acknowledged, look only at what changed since the last round, and never repeat a point already made. If nothing new, say so in a short summary and post no inline comments.

4. On a follow-up, resolve your own earlier comments whose fix is in the diff, with a short reply first. Never resolve other reviewers' comments.

5. Run the `code-review` skill for the code pass: the plugin's if installed, otherwise the built-in one.

6. Go through "What to check" for everything the code pass did not cover. Don't economize: on anything beyond a small diff, dispatch read-only subagents in parallel, one per dimension, each with the diff and the project's guides, and merge what they return. Time spent here is cheaper than a bug in production.

7. Write it up in two layers. One overall block: the verdict on intent, approach, and architecture, plus anything with no single line to sit on. Then inline comments on the exact lines or hunks for everything that does. Group related nits into one comment so the author is not buried. Every finding: severity, reason, suggested change. Plain words, short sentences, no filler, like a developer would write it. End with the dimensions you skipped and why.

## Severity Markers

Start each finding with a colored circle and severity:

- "🔴 [critical]" — must fix before merge
- "🟡 [warning]" — should fix, potential issue
- "🔵 [suggestion]" — optional improvement
