# absolute-debt

Lint and typecheck debt paydown. Clears pre-existing repo-wide lint/type violations and suppressions (@ts-ignore, # type: ignore) one rule per wave, fixing causes not symptoms — never quieting the checker.

## Quick Facts

| Field | Value |
|-------|-------|
| Category | workflow |
| Version | 0.5.0 |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |

## How to Install

```bash
npx skills add maddhruv/absolute-debt
```

## Usage

```bash
/absolute-debt "Clear our lint warnings and type errors"
/absolute-debt "Get to strict TypeScript mode"
```

Runs on green `main`. For diff-scoped quality use `/absolute-simplify`.

## Maintainers

- [@maddhruv](https://github.com/maddhruv)
