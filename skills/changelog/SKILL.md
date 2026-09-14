---
name: changelog
description: Use after completing a feature, fix, or any notable change, before it is pushed: "update the changelog", "add a changelog entry".
---

Update the `CHANGELOG.md` file with entries describing what changed.

## Format

Follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) categories:

- **Added** — new features
- **Changed** — changes to existing functionality
- **Fixed** — bug fixes
- **Removed** — removed features or deprecated code
- **Deprecated** — soon-to-be removed features
- **Security** — vulnerability fixes

## Steps

1. Read `CHANGELOG.md`. If the project has none, ask before creating one.
2. Determine what changed — use the diff against the default branch, recent commits, or ask the user.
3. Add entries under `## [Unreleased]`, grouped by category.
4. Each entry is a single line starting with `- ` — concise, user-facing language.
5. Only add categories that have entries — don't create empty sections.
6. If a category already exists under `[Unreleased]`, append to it rather than duplicating.

## Rules

- Write from the user's perspective, not the developer's (e.g., "Add push notifications" not "Add PushNotificationService class")
- One line per change, no sub-bullets
- Use imperative mood ("Add", "Fix", "Remove", not "Added", "Fixed", "Removed")
- Don't include internal refactors or code cleanup unless they affect behavior
- Don't duplicate entries that are already in the changelog

## Unreleased vs released changes

The `[Unreleased]` section describes the **difference between the last released version and the current state** — not a commit log.

Before adding an entry, check what's already in `[Unreleased]` and what's in previous versioned sections:

- **Fix for an unreleased feature** — don't add a "Fixed" entry. The feature in `[Unreleased]` was never released broken, so just keep the original "Added" entry. If needed, update its wording.
- **Fix for a released feature** — add a "Fixed" entry, because users of the released version experienced the bug.
- **Improvement to an unreleased feature** — update the existing "Added" entry instead of adding a separate "Changed".
- **Removal of an unreleased feature** — remove the "Added" entry entirely. No "Removed" needed since it was never released.

The same logic applies to all categories: ask "would this matter to someone upgrading from the last release?" If not, fold it into the existing unreleased entry or skip it.
