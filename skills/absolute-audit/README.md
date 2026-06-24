# absolute-audit

Vulnerability and security scan for your own repo. Finds dependency CVEs plus risky code patterns (secrets, injection, weak authz), triages by severity × reachability, and remediates without suppressing.

## Quick Facts

| Field | Value |
|-------|-------|
| Category | workflow |
| Version | 0.5.0 |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |

## How to Install

```bash
npx skills add maddhruv/absolute-audit
```

## Usage

```bash
/absolute-audit "Scan the repo for vulnerable deps and security issues"
/absolute-audit "Are we vulnerable to any CVEs?"
```

Authorized defensive use — audits your own repository. Complements the built-in `/security-review` (which reviews the pending diff).

## Maintainers

- [@maddhruv](https://github.com/maddhruv)
