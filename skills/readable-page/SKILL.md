---
name: readable-page
description: Use when information is meant to be looked at rather than read through: a plan, report, audit, comparison, explainer, or decision as a one-page HTML: "make a page for this", "show it visually", "I won't read a long doc".
---

# Readable Page

A one-page HTML artifact that someone who will never read the long version understands in under a
minute. The first screen carries the point; everything below supports it.

`assets/example-page.html` is a worked example: a decision with a diagram. Treat it as the level of
simplicity to aim for, not a template to fill. Reuse its CSS and components; pick only the sections
this content needs.

## The page

- **First screen:** a short badge naming what the page is (`ADR`, `Plan`, `Report`) with its date,
  an `h1` that states the point as a plain sentence with the key phrase in the accent colour, and
  two or three sentences under it. Three short cards with the main points, if there are three.
- **Then only what the reader needs**, each section with a small mono tag over its `h2`. Use what
  fits:
  - One picture of how something works: an inline SVG diagram.
  - Now and later: two cards, the later one dashed.
  - Options with one line each, the chosen one first and highlighted.
  - What it costs or what can go wrong: up to four cards.
  - What to do: a short numbered list.
  - A table when the content is a comparison.
- **Footer:** links to the full source (a record, a doc, an issue, a PR).

Five sections is plenty. If a section is there because pages usually have it, cut it.

## Look

Light only.

- The project's brand goes into the example's `:root` tokens. Without a brand, keep the example's.
- A near-white background with a faint accent glow at the top and a dot grid that fades out.
- White cards with hairline borders and a 10px radius, and no shadows.
- The accent darkened enough to read as text on white. A bright brand colour is only a tint.

## Diagram

- Draw what really exists. A dashed frame around the system the page is about; each node gets a
  tinted icon tile, a title and one muted line; chips carry key values.
- Edges have arrowheads. An accent pulse runs along them three times and replays on hover.
- A dashed "Kept out" row shows what is out of bounds, when that matters.
- Reuse the example's SVG classes and keep its fit script: some apps render fonts wider than a
  preview, and the script keeps every label inside its box.

## Copy

- Plain words, short sentences, at most five items per list. Name things the way the reader knows
  them.
- No middle dots (`·`) as separators and no em dashes anywhere. Use separate elements, commas or
  "and".
- No time estimates, no people's names in steps, no status pills, counters, label clouds,
  timelines or avatars. Anything that competes with the point for attention goes.
- No dark theme and no long caption beside the badge.

## Before you publish

- Does the first screen alone tell the reader the point?
- Is every section one the reader needs, and is anything repeated?
- Does every fact come from a source you checked?
- Look at the page once, rendered, if you can.
