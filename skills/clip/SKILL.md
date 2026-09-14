---
name: clip
description: Use when the user wants something from the chat in their clipboard: "copy that", "clip the sql", "give me the curl".
argument-hint: "[filter hint, e.g. sql, curl, bash]"
---

Copy a single useful artifact from recent conversation to the system clipboard.

**Do not** copy the entire last message. Pick one concrete artifact the user most likely wants to paste into a terminal, DB client, or editor.

## Selection

1. Scan recent assistant messages (latest first) for copy-worthy artifacts:
   - Shell / CLI commands (bash, `make`, `docker`, `uv`, `gh`, `psql`, `curl`, ...)
   - SQL queries
   - Code snippets (function, class, config block)
   - URLs, IDs, file paths, tokens, one-line outputs the user just asked for
2. If `$ARGUMENTS` is provided, treat it as a filter (e.g. `sql` → only SQL, `curl` → only curl commands, `last bash` → latest shell command). Match case-insensitively on artifact type or contents.
3. Prefer the **most recent** matching artifact.
4. If multiple plausible candidates tie and no filter is given, list them numbered with one-line previews and ask the user which to copy — do not guess.
5. If nothing copy-worthy is found, say so and stop — do not copy.

## Copying

- Strip surrounding markdown fences, language tags, and leading prompts (`$ `, `> `, `psql=# `, etc.) — copy the raw content the user would paste.
- Preserve internal formatting (newlines, indentation) as-is.
- Send the content to the system clipboard using whatever clipboard utility fits the current OS. If nothing suitable is available, say so and stop — do not fall back to printing the content.
- After copying, report in one short line: what was copied and its source (e.g. "Copied SQL query from the previous message (142 chars).").
- Never echo secrets (tokens, passwords) back into chat — just confirm the copy happened.
