---
name: docs
description: Use when writing, updating, or reviewing project documentation: "update the docs", "document this", "review the docs", "does this need a doc?".
---

# Docs

A good doc survives routine code changes. Renaming a class, moving a method, refactoring a function should not require touching it. If it does, the doc is bound too tightly to the implementation.

## Core principle

Document the **invariant**, not the **instance**:

- "Subscribers are dispatched after the transaction commits; failures are isolated." True regardless of how the code is wired.
- "`InMemoryEventBus.publish` calls `handler(event)` inside `try/except` on line 47 of `events.py`." Breaks the moment anyone reorders, renames, or refactors.

**The staleness test:** if I rename this class or move this file tomorrow, will the doc need updating? If yes, pull up a level of abstraction.

## The abstraction ladder

| Level | What it captures | Lifespan |
| --- | --- | --- |
| **Principles** | Why we work this way; what problem the pattern solves | Lifetime of the architecture |
| **Patterns** | Shape of a solution ("ports and adapters", "transactional outbox") | Until the architecture changes |
| **Roles** | What a layer, module, or concept owns and doesn't | Until we restructure |
| **Specifics** (paths, names, signatures) | Implementation detail | Stale next refactor. Don't document |

Aim for the top three. The bottom row is what code, types, and tests already say; repeating it in prose creates a second source of truth that eventually lies.

## When to write

- **Architecture decisions**: why this approach, what was considered, what was rejected.
- **Cross-cutting flows**: patterns that span modules, where no single file tells the whole story.
- **A module or library's introduction**: what it is, what it's for.

If the code or tests already say it, don't.

## Do

- Describe principles, patterns, roles. They survive refactoring.
- Capture trade-offs: alternatives considered and why each was rejected. That is the next reader's rediscovery saved.
- Use tables for comparisons. Readers scan tables; they don't read paragraphs.
- Cross-link to other docs instead of duplicating content.

## Don't

- Quote file paths, class names, function names, variable names. They rename and the doc rots silently.
- Paste large code blocks. Code lives in code; a doc shows the shape of a pattern, not the implementation.
- Repeat what types or tests enforce. Those are the canonical source of truth.
- Write "future plans" sections. Roadmaps live in the tracker, not in guides.
- Write step-by-step tutorials. They decay fastest. Explain at the principle level.

## Anti-patterns

- **Stale prose** that contradicts the code. Worse than no doc: it lies confidently.
- **A wall of code blocks.** The reader is reading code with prose comments; that belongs in the code.
- **Two docs on the same topic.** They drift apart. Pick one canonical source and link from the other.

## Maintenance

- Update the doc in the same PR as the change it describes.
- Delete what's no longer true. Git history keeps what was once written.
- After a major refactor, re-read related docs for stale paths and renamed concepts. Nothing automated validates prose.

## Updating after a change

1. List what changed on the branch (diff against the default branch) and which modules, features, or packages it affects.
2. Find where the project keeps its docs: module docs, guides, READMEs, wherever its conventions say.
3. Decide per affected area: a new module gets an introduction; a changed public API or behavior gets its doc updated; a new cross-cutting concept gets a guide entry; a changed layout gets the README updated. Nothing else gets a doc.
4. Show the user what you plan to write or change and confirm before writing.
5. Write it by the rules above.

## Reviewing

1. Find the documentation files changed on the branch: guides, module docs, READMEs.
2. Check each against every section above, then against the project's own documentation conventions where it has them.
3. Also check the reverse: does the code change on this branch leave any existing doc stale? A doc that nobody touched is the one most likely to lie now.
4. Report each finding as file and line, severity, and what to change.
