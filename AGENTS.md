# AGENTS.md

## Tooling
- `opencode.json` configures two MCP servers:
  - `github` — local MCP (`@modelcontextprotocol/server-github`), needs `GITHUB_TOKEN` env var (a repo-scoped PAT). Used for GH CLI/REST lookups during work.
  - `context7` — remote docs search. Use `"use context7"` or reference `context7` in prompts to pull current library docs (e.g. for MCP/GitHub setup).
- No build, test, or lint steps yet (docs-only repo). When infra scripts land, document the commands here.

## Commits
- **Conventional commits**: `feat:`, `fix:`, `docs:`, `chore:`, etc.
- **Logical batching**: group related changes per commit. No single bloated commits that mix ADRs, infra changes, and docs.
- One ADR = one commit (when first added).
- **PR template**: All PRs must follow `.github/PULL_REQUEST_TEMPLATE.md`. Use the template checkboxes and fill out the required sections.
