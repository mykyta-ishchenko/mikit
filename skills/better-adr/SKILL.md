---
name: better-adr
description: Use when recording, drafting, reworking, or explaining an architecture or design decision: "write an ADR", "record this decision", "compare the options and pick one", "make a readable page for this ADR".
---

# Better ADR

An ADR captures one decision: the context that forced it, the options weighed, the choice, and what
it costs. Each decision gets two outputs:

1. **The record**: the ADR text, where the project keeps its decision records. This is the source
   of truth, and never an artifact.
2. **The readable page**: a one-page HTML artifact that lets someone who will never read the record
   understand the decision in under a minute. It links back to the record.

`references/example-adr.md` is a worked record, and the `readable-page` skill bundles the matching
page. Read both before writing; they show the target shape better than any rule here.

Use it even when the user only describes a decision ("we'll send pushes ourselves") without saying
"ADR".

## Where it goes

Follow the project's conventions: where records live (a repo folder, a docs tool), how files are
named, which tracker the ticket ids come from, and the brand the page uses. Look in its docs,
`CLAUDE.md`, and existing records. If it has no place for ADRs, propose one and confirm first.

## Workflow

1. **Collect the decision.** What was decided, which options were weighed and why each lost, what
   it costs, what the reader must do next. Pull what you can from the conversation, linked docs,
   tickets and code; ask the user only for what is missing. Verify every fact you state (file
   paths, product features, limits) against its source and note the date you checked it. Never
   invent a reason: if nobody recorded one, ask.
2. **Draft the record** in the format below and show it to the user. Publish it only after they say
   so. Publishing is outward-facing, and the team reads it.
3. **Build the readable page** (below). Publish it as an HTML artifact and link the record in its
   footer.
4. **Iterate.** The user usually changes the decision several times while reviewing. Apply each
   change to both outputs in the same turn so they never disagree. Before you overwrite a published
   record, fetch the live one; someone may have edited it.
5. **After acceptance the record is immutable.** A later change of mind is a new ADR that supersedes
   the old one, never an edit. Only the old record's status changes, to point at the new one.

## The record

- **Title:** `YYYY-MM-DD Short title that states the decision`. Use the date the decision is
  recorded. Example: `2026-10-18 Push goes straight to Apple: direct APNs, no OneSignal`. A file
  name, where there is one, is the same in kebab case.
- **Subtitle:** one sentence summarising the decision.
- **Line breaks:** in a docs tool that turns every newline into a hard break, write one paragraph
  per line. In a repository, follow its wrapping rule.

Sections, in this order:

1. **TL;DR**: the decision in one paragraph, key nouns in bold.
2. **Status**: `Proposed (date)`, `Accepted (date)` or `Superseded by <link> (date)`. Name the ADRs
   it builds on or supersedes, and the ticket ids.
3. **Context**: the concrete problem and the forces, as facts. Include the pieces that already
   exist in the code, with paths and "checked <date>".
4. **🪛 Decision drivers**: hard filters (F1, F2, …) that eliminate options, then tie-breakers (T1,
   T2, …) that rank the survivors. Name the drivers before comparing options, or you end up
   rationalising a favourite.
5. **Options**: a driver × option matrix. Score every option on every driver with 🟢 / 🟡 / 🔴 plus
   a few words of reason, and mark the chosen column ⭐. Below it, one line per option that lost:
   `X, rejected: fails F1. <why>`. An option that is right later gets "deferred, not rejected".
6. **Decision**: "Adopt X:" then one bold-led bullet per topic (Data, Process, Deploy, Not now…).
   Then a Pros / Cons table, then one sentence on why it beat the runner-up.
7. **Consequences**: **Harder / risks**, each as `**Risk:** what happens. → mitigation.`; then
   **Mitigations & future**, which includes the one-time manual setup.
8. **References**: primary sources, each dated ("checked 2026-11-16"). End with a link to the
   readable page.

## The readable page

Build it with the `readable-page` skill; its bundled example is this kind of page. For an ADR it
usually holds, in this order: the badge `ADR | YYYY-MM-DD`, the decision as the `h1`, the main
wins, how it works with one diagram, now and later, the options with one line each (no matrix; that
stays in the record), what it costs, the one-time setup, and a footer link to the record. Drop what
this decision doesn't need.

Never put the status on the page: it belongs in the record. No em dashes in the record either.

## Before you publish

- Is the decision clear from the first screen alone?
- Do the record and the page say the same thing?
- Is every fact verified and dated in the record, and is every reason one somebody actually gave?
- Is the copy free of `·`, em dashes, names, statuses and time estimates?
- Does the diagram text fit its boxes? Look at the page once before publishing if you can render
  it.

## Reviewing an ADR

Check it against the sections and the "Before you publish" list, then report each finding as the
section, what is wrong, and the rewrite.
