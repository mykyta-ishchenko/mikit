---
name: reflect
description: Review the current session, extract learnings, and suggest updates to the project's instructions
disable-model-invocation: true
---

Reflect on the current session and extract actionable improvements for project documentation.

## Steps

1. Review the full conversation history — what was asked, what went wrong, what was corrected, what patterns emerged.

2. Identify learnings in two categories:

   **Mistakes & corrections** — things the user had to correct or reject:
   - Wrong assumptions about code structure or conventions
   - Incorrect approaches that were rejected
   - Patterns that were followed incorrectly

   **Discoveries** — things learned that aren't documented:
   - Project conventions that aren't in CLAUDE.md or its guides
   - Patterns that should be followed but aren't written down
   - Common decisions that could be codified as rules

3. For each learning, decide where it belongs:
   - `CLAUDE.md` — project-wide rules and conventions
   - The project's guides — whichever files `CLAUDE.md` references for architecture, conventions, testing, and the like, if the project has them
   - Memory — user-specific preferences that don't belong in the repo

4. Present the list to the user via `AskUserQuestion`:
   - What to add and where
   - Draft text for each addition
   - Let the user approve, edit, or reject each item

5. Apply the approved changes to the relevant files.
