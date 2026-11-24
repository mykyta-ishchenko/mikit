---
name: better-adr
description: Use when writing, updating, or reviewing a decision record (ADR): "write an ADR", "record this decision", "document why we chose X", "review this ADR".
---

# Better ADR

A decision record answers three questions in under a minute: what did we decide, why, and what did
we turn down. Everything else is noise that hides those answers.

## Where it goes

1. Follow the project's convention for decision records: their folder, file names, numbering, and
   how a changed decision is handled. Look in its docs, `CLAUDE.md`, and existing records.
2. If it has none, propose `docs/decisions/NNNN-<decision-in-kebab-case>.md` and confirm first.
3. One decision per file. A file that holds several decisions gets split, not extended.

## Shape

```markdown
# <The decision as one sentence>

Status: Accepted · <who decided> · <YYYY-MM-DD>

## Decision

One or two sentences: what we do now. A table when the decision is a set of values.

## Why

- One reason per bullet. Two to four bullets.

## Rejected

| Option | Why not |
|--------|---------|
| ...    | ...     |

## Consequences

- What each affected area has to do, and what gets harder.

## Revisit when

- The condition that should reopen this decision. Optional.

## Links

- The issue, the PR, the state or doc a reader needs next. Optional.
```

Status is one of `Proposed`, `Accepted`, `Replaced by <link>`, `Dropped`. Leave out an optional
section rather than writing "none".

## Rules

- **The title is the decision.** "Offline entries keep the phone's time", not "Offline entries".
  A reader who stops at the title knows what was decided.
- **Who and when, once.** They live in the status line. No "(Name, date)" after every bullet.
- **Current truth only.** The body says what holds now. Earlier values and reversals go in one
  `History:` line at the end, or stay in git.
- **One fact per bullet.** A bullet that needs a second sentence is two bullets. About 20 words is
  the ceiling.
- **Tables for comparisons.** Options against reasons, values per market, before against after.
- **Why is required.** A decision with no reason can't be challenged or revisited. If nobody
  recorded the reason, ask; don't invent one. If it stays unknown, write "not recorded".
- **Rejected is required** when there were alternatives. That is the next reader's rediscovery
  saved.
- **Decision, not state.** Versions deployed, environments, task status, and "today it is…" go in
  the project's state docs or tracker and are linked. They go stale within days; the decision does
  not.
- **No implementation detail.** Endpoints, file paths, class names, colors in hex, PR numbers in the
  body rot first. Name the rule; link to where the detail lives. See `better-docs` if available.
- **Plain words.** No hedging, no narrative of how the discussion went.

## When a decision changes

Follow the project's rule. Without one:

- **Replace:** write a new record, set the old one's status to `Replaced by <link>`, and leave its
  body alone. Readers can still see why the old choice was made.
- **Adjust** a detail that keeps the decision intact (a price, a limit): rewrite the record in place
  and add the old value to its `History:` line.

## Reviewing

Check each record against:

1. Can the title alone tell a reader what was decided?
2. Is there exactly one decision in the file?
3. Is the status line present, with who and when, and nowhere else repeated?
4. Does Why give at least one real reason? Does Rejected list the alternatives?
5. Does any bullet hold more than one fact, or exceed about 20 words?
6. Is any line state or implementation detail that will be wrong next month?
7. Is history mixed into the body instead of the `History:` line?

Report each finding as file, line, and the rewrite, not just the rule it breaks.
