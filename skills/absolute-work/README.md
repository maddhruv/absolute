# absolute-work

End-to-end, phase-gated SDLC for AI coding agents. Takes any unit of work — a ticket, a task, a plan, a migration — from fuzzy intent to verified code through six hard-gated phases.

## Quick Facts

| Field | Value |
|-------|-------|
| Category | workflow |
| Version | 0.5.0 |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |
| Deep-dive guides | `references/` (intake-playbook, migration-playbook, spec-writing, board-format, execution-model, verification-framework) |

## How to Install

```bash
npx skills add maddhruv/absolute-work
```

## Usage

```bash
/absolute-work "Add OAuth2 login with Google and GitHub providers"
/absolute-work "Grill me on my plan for a real-time chat feature, then build it"
/absolute-work "Refactor the auth middleware to use the new session API"
```

## Phases

```
INTAKE & BRAINSTORM ─┃ gate ┃─ SPEC ─┃ gate ┃─ DECOMPOSE & PLAN ─┃ gate ┃─ EXECUTE ─┃ gate per wave ┃─ VERIFY ─┃ gate ┃─ CONVERGE
```

Nothing is written until the design is approved. Every phase ends with a hard gate.

## Maintainers

- [@maddhruv](https://github.com/maddhruv)
