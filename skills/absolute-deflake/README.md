# absolute-deflake

Flaky test fixes. Detects nondeterministic tests empirically (repeat/shuffle/parallel runs), diagnoses the root cause (shared state, clock, network, concurrency), fixes it — never retries, skips, or sleeps.

## Quick Facts

| Field | Value |
|-------|-------|
| Category | workflow |
| Version | 0.5.0 |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |

## How to Install

```bash
npx skills add maddhruv/absolute-deflake
```

## Usage

```bash
/absolute-deflake "Fix the flaky tests in our CI"
/absolute-deflake "This test fails randomly"
```

## Maintainers

- [@maddhruv](https://github.com/maddhruv)
