---
name: work
description: Take a task, an issue or a description, to a plan, local changes, or an open PR, in the working mode you pick at the start.
disable-model-invocation: true
argument-hint: "[issue id or URL | description]"
---

Take a piece of work from intent to the finish line the user picks.

## Mode

Ask first, before reading any code: both questions in one prompt. Skip whichever `$ARGUMENTS` or the
conversation already answers. If there is no task yet, ask for it in the same prompt.

**Finish line:**

|           | Code changes | Commits                      | Ends with                                  |
|-----------|--------------|------------------------------|--------------------------------------------|
| **Plan**  | none         | none                         | a design and a plan                        |
| **Local** | yes          | none until the user approves | a working tree ready for the user's review |
| **PR**    | yes          | as you go                    | an open PR                                 |

**Decisions:**

- **Check in**: confirm the approach and the plan with the user, then implement without further
  stops.
- **Autonomous**: decide everything yourself and run to the finish line. See "Autonomous" below.

The mode overrides every skill invoked along the way. In Plan and Local nothing is committed or
pushed, including the design docs and plans those skills write and would commit. Picking PR is the
go-ahead to commit, push, and open the PR.

## Steps

1. Invoke `mikit:learn-project`.

2. Get the intent. If the task is an issue in the project's tracker (an ID or URL), fetch it through
   the tracker's MCP plugin or CLI: title, description, labels, linked issues. Otherwise the
   description is the intent.

3. Explore the affected code: current structure, patterns, dependencies, tests. Know what the change
   touches before deciding how.

4. Design.
    - Check in: if the task is ambiguous, several approaches compete, or it touches unfamiliar or
      shared code, run a design session with the user (`superpowers:brainstorming` if installed).
      Otherwise state the approach in a few sentences and confirm it.
    - Autonomous: weigh two or three approaches yourself, pick one, log it.

5. Local and PR: if the current branch doesn't match the task, create one from the latest default
   branch following the project's branch naming.

6. Plan: concrete steps, what changes and why, which tests are affected. For anything beyond a
   handful of steps, use `superpowers:writing-plans` if installed. Check in: show it and confirm.
    - Plan mode ends here. Present the design and the plan, and offer to continue in Local or PR.

7. Implement the plan end to end. Don't stop after each step to ask whether to continue.
    - Check in: stop only for a decision the plan didn't cover.
    - PR: commit each logical step, following the project's commit conventions.

8. Verify per the project's rules: verify command green, docs updated where the change warrants it
   (`mikit:docs`), changelog entry if the project keeps one (`mikit:changelog`).

9. Autonomous: review your own diff with `mikit:better-code-review` and fix what it finds. Nobody
   has checked your decisions yet.

10. Hand over.
    - Local: what changed, how it was verified, the decision log, and a proposed commit message.
      Wait for the user's review. Commit after their go; push or open a PR only if they ask.
    - PR: open it with `mikit:create-pr`, decision log in the body, and return the URL.

## Autonomous

- When an invoked skill would ask the user something, answer it yourself.
- Choose what the codebase already does; failing that, the smallest option that is easiest to
  reverse.
- Log every non-obvious choice: what you picked, over what, and why. The log goes into the
  hand-over.
- Stop and ask only when going on would need something outside the task: an irreversible or
  destructive action, shared environments, credentials, someone else's branch or PR, or a task that
  turns out contradictory or much bigger than described.
