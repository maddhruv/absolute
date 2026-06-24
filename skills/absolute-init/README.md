# absolute-init

One-time setup for the absolute workflow engine. Detects your stack + asks a few questions about how you want absolute to behave, then writes JSON config that every other `absolute-*` skill reads on every run.

## Quick Facts

| Field | Value |
|-------|-------|
| Category | workflow |
| Version | 0.5.0 |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |

## How to Install

```bash
npx skills add maddhruv/absolute-init
```

## Usage

```bash
/absolute-init
```

## What it does

1. **Detect** — reads your package manager, test runner, lint/format tools, build scripts, and CI config.
2. **Interview** — asks ~4–6 questions: output style, autonomy level, TDD strictness, spec output dir, relevant command families.
3. **Write** — produces `.absolute.config.json` (project-level, commit this) and optionally `~/.absolute/config.json` (user-level, machine-local).

Every other `absolute-*` skill reads this config instead of re-detecting. Commands work without it (soft recommend only).

## Maintainers

- [@maddhruv](https://github.com/maddhruv)
