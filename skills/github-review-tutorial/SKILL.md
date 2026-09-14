---
name: github-review-tutorial
description: Use when about to post a review or comments on a GitHub pull request: "review this PR", "post the findings".
---

# GitHub Review

Mechanics for posting a pull request review on GitHub. The review itself is delegated to whatever skill the prompt names. This skill decides what may be posted and how.

## Rules

- **Target.** Every write goes to the PR you were asked to review and nothing else. No comments, reviews, reactions, branch changes, or pushes on any other PR. Unsure which PR? Stop and ask.
- **Review type.** Submit as "Comment" only. Never "Approve", never "Request changes".
- **One review, not scattered comments.** A single review with a short summary body and the findings as inline comments grouped under it. Nothing to flag? Post the summary alone. Don't add inline comments for the sake of having them.
- **Anchoring.** Inline comments only on lines that are part of the diff, with path, line, and side. A finding with no diff line to sit on goes into the summary body. One bad anchor rejects the whole review.
- **No bot voice.** No "Generated with…" signatures, no reaction prompts, no thank-yous. Be specific and short: file and line, what is wrong, what to do. Use a suggestion block for a small, exact fix.

## Steps

1. **Verify the target.** Fetch the PR's number, state, draft flag, head branch, and head commit. If it is closed or merged, say so and stop. Use these values for every write that follows.

2. **Understand the PR.** Read the title, description, linked issues, and the full diff.

3. **Check for earlier reviews from this account on the PR.** If the head commit is the same one you last reviewed, don't post: say so and stop. Otherwise this is a re-review: compare the diff since the last reviewed commit against what was already posted. Post inline comments only for findings that are new or changed. In the summary, say how many earlier findings were addressed and how many still stand.

4. **Run the review** with the skill named in the prompt.

5. **Post one review.** Event Comment, the head commit as the reviewed commit, a 2-3 sentence summary body (what was checked, overall impression, which commit was reviewed), and the findings as inline comments. If the API rejects the review, move the offending comments into the body and post once more.

6. **Return the review URL.**
