# MiKit

**How I work, wherever I work.**

A personal [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin: the skills I want in every project, independent of any one repo. Project rules live in each repo's `CLAUDE.md`. This kit holds the habits that don't change between them.

## Install

Register the marketplace once, then install the plugin:

```bash
claude plugin marketplace add mykyta-ishchenko/mikit
claude plugin install mikit@mikit
```

To pin it to a project so every clone and CI run gets it, declare it in the project's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "mikit": {
      "source": { "source": "github", "repo": "mykyta-ishchenko/mikit" },
      "autoUpdate": true
    }
  },
  "enabledPlugins": { "mikit@mikit": true }
}
```

## Skills

Skills trigger on their own when a request matches, or explicitly as `/mikit:<name>`.

| Skill | What it does |
|---|---|
| `ai-security` | Reviews code that touches an LLM for prompt injection, data exfiltration, unsafe output handling, and auth bypass |
| `better-code-review` | Runs a full PR review: project context, previous rounds, code, tests, security, with severity-marked comments |
| `changelog` | Adds Keep a Changelog entries under Unreleased, folding fixes to unreleased work into the original entry |
| `clip` | Copies one command, query, or snippet from the conversation to the system clipboard |
| `create-pr` | Drafts a pull request from the branch, using the repo's template if it has one, and opens it after you confirm |
| `create-task` | Files a well-formed issue in the project's tracker from what was actually discussed, after you confirm the draft |
| `docs` | How to write, update, and review documentation that survives refactoring |
| `github-review-tutorial` | Rules for posting a PR review on GitHub: verify the target PR, comment-only, one review with inline findings |
| `python-tests` | How to write and review Python tests: what to test, test types, mocking, structure, organization |
| `rebase` | Rebases the current branch onto the latest default branch, carrying uncommitted changes along |
| `reflect` | Reviews the session for mistakes and undocumented conventions, then proposes edits to the project's instructions |
| `handoff` | Writes a handoff file so a fresh session can resume the task with zero context |
| `learn-project` | Prints the layout and reads the project's instructions, docs, and task runner so a session starts with full context |
| `respond-review` | Triages unresolved PR review comments, fixes what is valid, declines what is not, and replies to each |

## Conventions

- Every skill is project-agnostic. When one needs project context, it defers to the project's own conventions: `CONTRIBUTING.md`, docs, `CLAUDE.md`, whatever the repo has.
- A skill's description says *when* to use it, not what it does. That line is what Claude matches your request against.
- Every change bumps `version` in `.claude-plugin/plugin.json`. Installed copies only refresh when the version changes.

## License

[MIT](LICENSE)
