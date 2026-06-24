# absolute-prune

Dead code and dependency cleanup, repo-wide. Removes unused deps, unreferenced exports, unreachable code, and orphaned files — only with tool evidence, in reversible waves, tests green after each.

## Quick Facts

| Field | Value |
|-------|-------|
| Category | workflow |
| Version | 0.5.0 |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |

## How to Install

```bash
npx skills add maddhruv/absolute-prune
```

## Usage

```bash
/absolute-prune "Remove dead code and unused dependencies"
/absolute-prune "What can we safely delete?"
```

Runs on green `main`. For diff-scoped cleanup use `/absolute-simplify`.

## Maintainers

- [@maddhruv](https://github.com/maddhruv)
