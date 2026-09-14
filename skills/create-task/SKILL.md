---
name: create-task
description: Use when the user wants to file an issue, ticket, task, or bug in the project's tracker: "create a task", "file a ticket", "log this as a bug".
argument-hint: "[description]"
---

Create a well-formed issue in the project's tracker: the one its conventions name (Linear, Jira, GitHub Issues, whatever the repo uses), through its MCP plugin or CLI.

## Steps

1. **Understand what to create.** Use `$ARGUMENTS` as the seed if provided; otherwise ask the user what the issue is about. Clarify the problem or goal and why it matters. If a detail is unknown, ask the user or leave it out. Never fill it in with an assumption. If several distinct issues are implied, confirm whether to split them into separate tickets.

2. **Fetch the tracker's context before choosing fields.** Query it for what actually exists: teams, projects, milestones, labels, workflow statuses, whichever of these the tracker has. Never guess these.

3. **Draft the issue** in the language the project uses for tickets (English unless its conventions say otherwise):
   - **Capture only what was discussed.** Write the problem and intent as the user stated them. Do not invent scope, acceptance criteria, an implementation approach, or any decision that wasn't discussed. Leave undecided things open for whoever picks the task up. A thin but accurate ticket beats a fleshed-out but fabricated one.
   - **Title**: short and imperative, describing the outcome (e.g. "Stop exposing external_id in API user responses").
   - **Description**: enough to act on cold, with no padding. Lead with *why* and what's wanted. Add scope or structured sections only when the user actually gave that detail; a short ticket is fine.

4. **Choose fields to best fit** (see below).

5. **Show the full draft to the user**, title, description, and every chosen field, and confirm before creating. Adjust if needed.

6. **Create the issue** and return its identifier and URL.

## Field selection

Use whichever of these the tracker has:

- **Team / project / milestone**: place it where the work belongs; match the project and milestone to the issue's scope.
- **Labels**: assign the relevant existing labels (type, area). Don't invent new labels unless the user asks.
- **Priority**: set from urgency and impact; when unclear, default to the tracker's neutral value and say so.
- **Status**: the "up next" status if it should be worked on soon, the backlog status otherwise.
- **Assignee / parent**: set only when the user specifies them or it is obvious from context.

## Common mistakes

- Inventing a solution, scope, or acceptance criteria the user never discussed. Capture intent and leave open decisions for execution time.
- Padding the ticket to look complete. Less but accurate beats more but wrong.
- Guessing project, label, or status names instead of fetching what exists (step 2).
- Writing the ticket in the conversation's language instead of the project's.
- A description that only makes sense with this chat's context. Write it to be actionable cold.
- Creating the issue before the user confirms the draft.

This skill only creates issues. Picking one up and implementing it is a separate job.
