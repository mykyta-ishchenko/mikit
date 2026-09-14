---
name: handoff
description: Capture the current task's state into a handoff document so a fresh session can resume it without losing context
disable-model-invocation: true
argument-hint: "[optional focus note]"
---

Write a self-contained handoff so the next session can continue this task with zero memory of the current conversation.

## Steps

1. Gather the real state — read it, don't guess:
   - `git branch --show-current`, `git status --short`, `git log --oneline <default>..HEAD` where `<default>` is the default branch of `origin` (`git symbolic-ref --short refs/remotes/origin/HEAD`; if unset, `HEAD branch` from `git remote show origin`)
   - Open PR, if any: `gh pr view --json number,url,state` (ignore the error when there's none)
   - From this conversation: the goal, what's done, what's left, and every non-obvious decision **and the reason behind it**

2. Write the handoff to `<scratch>/handoff-<branch-slug>.md` (branch slug = branch name with `/` → `-`), following the template below. `<scratch>` is the project's scratch directory if it has a convention for one, otherwise `.temp/`; create it if missing, and if it is not gitignored say so in the report instead of committing it.
   - Be concrete and terse — the reader has no context. Prefer file paths, commands, and exact next steps over prose.
   - Capture the *why* behind decisions so they aren't relitigated, and call out blockers, gotchas, and open questions explicitly.
   - If a focus note was passed with the invocation (`$ARGUMENTS`), give that aspect extra weight.
   - Don't dump the whole diff — point to it. The handoff is a map, not a copy of the work.

3. Report to the user: the file path and the exact prompt to paste in the next session:
   > Read `<path>` and continue.

## Template

```markdown
# Handoff: <task title>

**Branch:** <branch> · **PR:** <url or —> · **Date:** <today, absolute>

## Goal
<1–2 sentences: what we're doing and why>

## Status
<what's committed / pushed, working-tree state, what's been verified (e.g. the project's verify command green)>

## Done
- <completed, verified step>

## Next
1. <the very next concrete action>
2. <then…>

## Decisions
- <choice> — <why> (locked in; don't reopen without reason)

## Watch out
- <blockers, gotchas, failing tests, open questions, things that surprised us>

## Resume
- Invoke the project's `learn-project` skill if it has one, otherwise read `CLAUDE.md`; then read this file and pick up from **Next**.
- <any task-specific setup or command needed to get going>
```
