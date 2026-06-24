# absolute-spec

Lightweight standalone design spec. Scan the codebase, ask only what code can't answer, write a reviewed spec, stop. No task board, no build. Chains into `/absolute-work` when you're ready to implement.

## Quick Facts

| Field | Value |
|-------|-------|
| Category | workflow |
| Version | 0.5.0 |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |

## How to Install

```bash
npx skills add maddhruv/absolute-spec
```

## Usage

```bash
/absolute-spec "Design a commenting system for blog posts"
/absolute-spec "Draft a design doc for our rate limiter, don't build it yet"
```

## Flow

```
SCAN → CLARIFY (3–5 questions) → WRITE → REVIEW (scored) → HANDOFF
```

Output: a `docs/plans/YYYY-MM-DD-<topic>-design.md` with a weighted reviewer score. Chains into `/absolute-work` at decompose when ready.

## Maintainers

- [@maddhruv](https://github.com/maddhruv)
