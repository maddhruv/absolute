# absolute-upgrade

Dependency upgrades done safely. Groups deps into semver waves (patch/minor batched, majors gated and changelog-read), applies incrementally, regenerates lockfiles, and runs tests green after each wave.

## Quick Facts

| Field | Value |
|-------|-------|
| Category | workflow |
| Version | 0.5.0 |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |

## How to Install

```bash
npx skills add maddhruv/absolute-upgrade
```

## Usage

```bash
/absolute-upgrade "Bring our dependencies up to date"
/absolute-upgrade "Upgrade React to 19"
```

Runs on green `main`. For vulnerability-specific work use `/absolute-audit`.

## Maintainers

- [@maddhruv](https://github.com/maddhruv)
