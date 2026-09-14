---
name: respond-review
description: Fix unresolved PR review comments and reply to each one
disable-model-invocation: true
---

Respond to unresolved review comments on the current PR.

## Mindset

**Don't apply reviewer suggestions blindly.** Every comment is a hypothesis, not a directive — your job is to evaluate, not to obey. For each finding:

- **Read the actual code and context** before forming an opinion. The reviewer may be operating on a stale file view, missing a constraint, or reading the wrong file.
- **Reason about validity** — does the suggestion improve correctness, safety, readability, or performance in a meaningful way for *this* codebase? Or is it a generic best-practice that doesn't apply here?
- **Push back when you disagree.** Declining a request is a normal outcome — state your reasoning briefly and move on. Don't argue, don't apologize, don't capitulate to be polite.
- **Watch for false signals** — reviewers sometimes flag intentional decisions as bugs (e.g. "you removed X" when X is still there; "this could fail" when the failure path is correct). Verify the claim against current state before fixing.
- **Distinguish "wrong" from "different style"** — if the reviewer prefers a different shape of the same correct code, ask whether the change is worth the diff. Usually no.

Categories of valid responses: **fixed** (you agree, applied), **declined** (you disagree with reasoning), **explained** (the comment was a question/misunderstanding), **deferred** (valid but out of scope for this PR).

## Steps

1. Get the current branch and find the associated PR:
   If no PR exists or it's closed — tell the user and stop.

2. Fetch all review threads and find unresolved ones.

3. Filter threads:
   - Skip resolved threads
   - Skip threads where the **last comment** is already from us (already replied)
   - Keep unresolved threads with comments we haven't responded to

4. If no actionable threads — tell the user "nothing to respond to" and stop.

5. For each unresolved comment, understand what's being asked:
   - Read the referenced file and the surrounding code
   - Determine if the comment requests a code change or is just a question/suggestion

6. **Triage each comment** before touching code:
   - **Valid + actionable** → apply the change, run the project's verify command after all fixes.
   - **Valid but out of scope** → prepare a "deferred" reply, optionally suggest a follow-up issue.
   - **Invalid / based on stale view / generic noise** → don't change code, prepare a declined reply with brief reasoning.
   - **Misunderstanding / question** → prepare an explanation, no code change.
   - When in doubt, lean toward declining over agreeing — a wrongly-applied "fix" creates churn and noise; a wrongly-declined comment can be revisited cheaply.

7. **Ask the user for confirmation before committing and replying:**
   - Show a summary table: for each comment — what you did (fixed / declined / explained) and the planned reply text
   - If the user wants changes — adjust fixes or replies and ask again
   - Only proceed to step 8 after the user confirms

8. **Commit and push fixes first** (before replying):
   - Stage and commit all code changes with a descriptive message
   - Push to the PR branch
   - The reviewer should see the fix in the diff when they read the reply

9. **Reply to each comment**.
   Reply style rules:
   - If fixed with a code change: reply "fixed" or "done"
   - If it was already correct / not applicable: briefly explain why (1-2 sentences max)
   - If it's a question: answer concisely
   - If you disagree: state your reasoning briefly, don't write essays
   - **Never** write long responses.
   - Sound like a human developer, not a bot
   - Easy to read: plain words, short sentences, one point per reply. The reviewer should get it on the first pass

10. After all replies, give the user a short summary of what was fixed and what was declined.
