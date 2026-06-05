# absolute-work

absolute-work is a production-ready AI agent skill for claude-code, gemini-cli, openai-codex, and mcp. An end-to-end, phase-gated software development lifecycle that turns a ticket, task, plan, or migration into a validated design, a dependency-graphed task board, and verified code.

## Quick Facts

| Field | Value |
|-------|-------|
| Category | workflow |
| Version | 0.1.0 |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |
| References | 6 deep-dive guides |
| Evals | 14 test cases |

## How to Install

1. Make sure you have Node.js installed on your machine.
2. Run the following command in your terminal:

```bash
npx skills add AbsolutelySkilled/AbsolutelySkilled --skill absolute-work
```

3. The absolute-work skill is now available in your AI coding agent (Claude Code, Gemini CLI, OpenAI Codex, etc.).

## Overview

absolute-work is one continuous skill that takes any unit of work — a ticket, a task, a plan, a migration — from fuzzy intent to verified code. It replaces the traditional brainstorm-then-build handoff with a single lifecycle that stops at hard gates so you stay in control the whole way.

It relentlessly interviews you to a shared design (one question at a time, codebase-first), writes and self-reviews a spec, decomposes the work into atomic tasks on a persistent local markdown board, then peels those tasks off **one safe wave at a time** with test-first verification. No external trackers, no silent scope creep, and no code until the design is approved.

### The 6 phases

1. **Intake & Brainstorm** — deep context scan, codebase-first questioning, adaptive question banks per work type, relentless design interview until mutual 100% confidence
2. **Spec** — writes the approved design to `docs/plans/`, then a separate reviewer subagent grades it on a scored rubric
3. **Decompose & Plan** — atomic tasks, a dependency graph, and safe-wave assignment on `.absolute-work/board.md`
4. **Execute** — onion-peel one wave at a time, TDD per task (red → green → refactor)
5. **Verify** — binary signals (test/lint/typecheck/build) then an independent scored evaluator
6. **Converge** — full suite, summary, manual test steps to exercise the new functionality, close the board, suggest a commit (never auto-commit)

### What makes this skill different

- **Phase-gated** — stops and waits for your explicit "go" between every phase. Control over speed.
- **Safety-first execution** — blockers and dependents run sequentially; only provably-independent tasks parallelize. When in doubt, it serializes.
- **Adaptive intake** — detects the work type (feature, bug, refactor, greenfield, planning, migration) and swaps in a tailored question bank. Migrations get first-class handling (call-site inventory, codemods, incremental rollout, rollback).
- **Spec-driven + test-driven** — a reviewed spec before code, tests before implementation, generator-evaluator separation for grading.
- **Fully local** — all state lives in `.absolute-work/board.md`. No JIRA, no GitHub API, fully portable and resumable across sessions.

### Reference Guides

| Guide | Coverage |
|-------|----------|
| Intake Playbook | Adaptive question banks per work type, codebase-first intelligence, design-tree traversal, calibration |
| Migration Playbook | Call-site inventory, codemods, coexistence seams, incremental rollout, backwards-compat, rollback |
| Spec Writing | Spec template, section scaling, writing style, decision log, scored review protocol |
| Board Format | Full `.absolute-work/board.md` spec, status transitions, sequence/wave model, example board |
| Execution Model | DAG patterns, safe-wave (sequential-blocker / parallel-independent) algorithm, agent prompts, conflict handling, failure recovery |
| Verification Framework | TDD per task, verification signals, generator-evaluator protocol, scored rubric, mandatory tail tasks |

### Key Principles

1. Phase gates always — explicit approval between every phase
2. Codebase before questions — only ask what code can't answer
3. Relentless until aligned — interview to mutual 100% confidence
4. Spec before code, tests before implementation
5. Dependency-first decomposition — a DAG, never a flat list
6. Safety-first execution — serialize when in doubt
7. Generator ≠ evaluator — the builder never grades its own work
8. No silent scope creep, and never auto-commit

## Tags

`sdlc` `planning` `task-management` `tdd` `migration` `workflow` `spec-driven-development` `brainstorming` `verification`

## Platforms

- claude-code
- gemini-cli
- openai-codex
- mcp

## Related Skills

Pair absolute-work with these complementary skills:

- [absolute-simplify](https://www.absolutelyskilled.pro/skill/absolute-simplify)
- [absolute-ui](https://www.absolutelyskilled.pro/skill/absolute-ui)

## Frequently Asked Questions

### What is absolute-work?

An end-to-end, phase-gated software development lifecycle for AI agents. It turns a ticket, task, plan, or migration into a validated design, a dependency-graphed task board, and verified code — relentlessly interviewing you to a shared design, writing a reviewed spec, decomposing into atomic tasks on a persistent local markdown board, then peeling tasks off one safe wave at a time with test-first verification.

### How is this different from just asking the agent to build something?

absolute-work imposes structure and control. It refuses to write code until the design is approved, it stops at a hard gate between every phase, it tracks everything on a persistent board that survives across sessions, and it verifies each task with an independent evaluator instead of self-grading. The result is fewer wrong turns, no silent scope creep, and a complete audit trail.

### Does it integrate with JIRA or GitHub Issues?

No — absolute-work is fully local by design. "Ticket", "task", "planning", and "migration" are intake *types* that produce tasks in a local `.absolute-work/board.md` file. There are no external tracker dependencies, so it is portable and works in any repo.

### How do I install absolute-work?

Run `npx skills add AbsolutelySkilled/AbsolutelySkilled --skill absolute-work` in your terminal. The skill will be immediately available in your AI coding agent.

### What AI agents support absolute-work?

This skill works with claude-code, gemini-cli, openai-codex, mcp. Install it once and use it across any supported AI coding agent.

## Maintainers

- [@maddhruv](https://github.com/maddhruv)

---

*Generated from [AbsolutelySkilled](https://www.absolutelyskilled.pro/skill/absolute-work)*
