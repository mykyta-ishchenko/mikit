---
name: rebase
description: Rebase the current branch onto the latest default branch
disable-model-invocation: true
---

Rebase the current branch onto the latest default branch of `origin`:

1. Resolve the default branch: `git symbolic-ref --short refs/remotes/origin/HEAD` (strip the `origin/` prefix). If it is unset, read `HEAD branch` from `git remote show origin`. If neither works, ask the user.
2. `git fetch origin <default> && git rebase --autostash origin/<default>`. Autostash carries uncommitted changes across the rebase, so there is no manual stash/pop.
3. If there are conflicts, show them, attempt to resolve, and ask the user to confirm before `git rebase --continue`.
4. Report the result: how many commits were rebased, whether the autostash was reapplied, any issues.
