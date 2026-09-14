# CLAUDE.md

This repo is `mikit`, a personal Claude Code plugin published as its own single-plugin marketplace. It holds skills that must work in any project. Everything here is public and in English.

## Layout

- `.claude-plugin/plugin.json`: plugin manifest, owns `version`
- `.claude-plugin/marketplace.json`: marketplace manifest; its plugin entry mirrors the plugin description
- `skills/<name>/SKILL.md`: one skill per directory
- `README.md`: install instructions and the skills table

## Skill rules

- **Project-agnostic.** Never reference a specific project's files, commands, or tools. Defer to the project's conventions wherever it keeps them: `CONTRIBUTING.md`, docs, `CLAUDE.md`, a `learn-project` skill if it has one. Say "the project's verify command", not a literal command, and "the project's git conventions", not a file path.
- **Description says when, not what.** For model-invocable skills, start with `Use when` and include two or three phrases a user would actually say. Keep it short: the skill listing is shared with every other plugin and gets trimmed. For skills with `disable-model-invocation: true`, the description is for the `/` menu and should say what the skill does.
- **Lean bodies.** Steps and rules only. No narrative, no restating what the tools already explain.
- **Keep skills on the current standard.** Best practices move. Before creating or editing a skill, re-read `superpowers:writing-skills` and the Anthropic guidance it references; do not work from memory. Periodically, and whenever Claude Code or the superpowers plugin updates, audit every skill against the current standard and fix drift in the same change.

## Releasing a change

Installed copies refresh only when `version` changes, so every change ships with a bump:

1. Edit the skill or manifest.
2. Bump `version` in `plugin.json`: minor when a skill is added or removed, patch otherwise.
3. If the plugin description changed, mirror it in `marketplace.json`.
4. If a skill was added, removed, or renamed, update the table in `README.md`.
5. `claude plugin validate .claude-plugin/plugin.json` and `claude plugin validate .claude-plugin/marketplace.json` must pass.
6. To see it live: `claude plugin update mikit@mikit`, then `/reload-plugins` in the session. `claude plugin details mikit@mikit` shows the loaded inventory and token cost.

## Commits

- Only commit when asked
- One-line conventional commit messages (`feat:`, `fix:`, `docs:`, `chore:`)
- Never mention Claude, AI, or tooling in commits, code, or docs. No `Co-Authored-By`
