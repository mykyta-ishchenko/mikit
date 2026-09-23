---
name: create-pr
description: Use when the branch is ready and the user says "create a PR", "open a pull request", "ship it", "make the PR".
---

Create a pull request for the current branch:

1. Resolve the default branch of `origin` (`git symbolic-ref --short refs/remotes/origin/HEAD`; if unset, `HEAD branch` from `git remote show origin`). If the current branch is the default branch, tell the user and stop.

2. Check for uncommitted changes. If any, show the diff and ask the user whether to commit them now (you draft the message and commit) or stop so they can handle it themselves. Don't proceed with the PR until the working tree is clean.

3. Check if a PR already exists:
   ```
   gh pr view --json number,url,state,headRefName
   ```
   If it exists and is open, tell the user and stop (`/mikit:respond-review` handles review comments).

4. Gather branch context:
   - `git log --oneline <default>..HEAD` — commits on this branch
   - `git diff <default>...HEAD --stat` — files changed
   - `.github/pull_request_template.md` if the repo has one — use it verbatim as the body skeleton; otherwise use `## Summary` and `## Test plan`

5. Follow the project's git and PR conventions: `CONTRIBUTING.md`, a git or PR guide in the docs, whatever the repo has.

6. Draft the PR:
   - **Title**: short (under 70 chars), conventional-commit style, matches the nature of the work on the branch (not just the latest commit)
   - **Body**: use the template's sections if there is one, otherwise `## Summary` and `## Test plan`. Whatever the sections, cover:
     - the task link or number, if one is in context (conversation, branch name, commits); if none, leave the template's placeholder intact
     - what the task was and what we wanted to achieve, in a sentence or two
     - what came out of it, briefly
     - how to test it, if there is a way (the project's verify command, manual steps)
   - Write like a developer, not a bot: plain words, short sentences, no filler, no restating the diff.
   - Write for a reviewer who has only the PR: the diff, the repo, and links they can open. Name every change by what it does in the code. When the work came from something only you can see (a local plan, a spec file that isn't committed, this conversation), say what that part does in plain words: "adds retry to the upload client", not "Part 2 of the plan" or "option B we agreed on".

7. Push the branch if not already pushed.

8. Create the PR.

9. Return the PR URL.
