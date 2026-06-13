# absolute

absolute is a production-ready development workflow engine for AI coding agents (claude-code, gemini-cli, openai-codex, and mcp). One skill, four commands, dispatched as `/absolute <command> [target]` — covering the loop you run every day: **think → plan → build → polish → document**.

## Quick Facts

| Field | Value |
|-------|-------|
| Category | workflow |
| Version | 0.2.0 |
| Commands | `work`, `ui`, `simplify`, `docs` |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |
| References | 4 command flows + 41 deep-dive guides |

## How to Install

1. Make sure you have Node.js installed on your machine.
2. Run the following command in your terminal:

```bash
npx skills add maddhruv/absolute
```

3. The `absolute` skill is now available in your AI coding agent. Invoke it as `/absolute <command>`.

## Commands

| Command | Phase | What it does |
|---|---|---|
| `/absolute work [target]` | Think · Plan · Build | End-to-end, phase-gated SDLC: relentless design interview → reviewed spec → dependency-graphed task board → safe-wave TDD execution → verification → converge. Handles features, bugs, refactors, greenfield, planning breakdowns, and migrations. |
| `/absolute ui [target]` | Design | Build polished, intentional UIs with concrete CSS/Tailwind values — typography, color, layout, spacing, dark mode, accessibility, animations, components. The most comprehensive UI knowledge base, not vague advice. |
| `/absolute simplify [target]` | Polish | Autonomously simplify your staged/unstaged git changes or a target path — reduce complexity, flatten nesting, remove redundancy and dead code — then run tests to prove nothing broke. |
| `/absolute docs [target]` | Document | Diátaxis-driven documentation: write, improve, or audit tutorials, how-tos, reference, explanation, and developer docs (README, CONTRIBUTING, ADRs). Detects the docs stack and verifies every claim against the code. |

### Usage

```bash
/absolute work "Add OAuth2 login with Google and GitHub providers"
/absolute work "Grill me on my plan for a real-time chat feature, then build it"
/absolute ui "Design a pricing page for my SaaS"
/absolute ui "Make this dashboard feel less like AI slop"
/absolute simplify
/absolute simplify "clean up my changes in src/api/"
/absolute docs "Write a getting-started tutorial for the CLI"
/absolute docs "Audit our docs site and propose a restructure"
```

You can also describe the task and let the router pick the command:

```bash
/absolute "clean up my working changes"      # → simplify
/absolute "write a README for this package"   # → docs
/absolute                                     # → shows the menu, recommends a command
```

## Overview

`absolute` replaces four separate skills with one coherent workflow engine. A thin
router (`SKILL.md`) resolves which command you want — by explicit name, by intent, or
by presenting a context-aware menu when called with no argument — then loads that
command's full playbook from `references/<command>.md` and follows it.

Each command keeps its own opinionated flow: `work` gates between every phase, `docs`
gates on the outline before writing prose, and `simplify` stays bounded to scoped git
changes. None of them auto-commit — they write files and report; you commit.

### What makes this skill different

- **One install, one update.** `npx skills add maddhruv/absolute` brings all four commands; updates pull everything at once.
- **Shared vocabulary.** `/absolute work | ui | simplify | docs` is a single command surface your agent and team learn once.
- **Codebase before questions.** Every command reads what exists before asking, and verifies behavior instead of assuming it.
- **Gate before expensive work.** Hard gates and bounded scope keep you in control; no silent scope creep.

### Reference Guides

Command flows live in `references/work.md`, `references/ui.md`, `references/simplify.md`,
and `references/docs.md`. Each command's deep-dive guides are namespaced under
`references/<command>/` (e.g. `references/ui/typography.md`,
`references/work/execution-model.md`, `references/simplify/python.md`,
`references/docs/docs-stacks.md`) and load only when the task needs them.

## Tags

`workflow` `sdlc` `planning` `tdd` `ui` `design` `simplification` `refactoring` `documentation` `diataxis`

## Platforms

- claude-code
- gemini-cli
- openai-codex
- mcp

## Maintainers

- [@maddhruv](https://github.com/maddhruv)

---

*Generated from [Absolute](https://github.com/maddhruv/absolute/tree/main/skills/absolute)*
