# absolute

absolute is a production-ready development workflow engine for AI coding agents (claude-code, gemini-cli, openai-codex, and mcp). One skill, ten commands, dispatched as `/absolute <command> [target]` — a **build loop** you run every day (think → spec → plan → build → polish → document) plus an **engineering-health** family that keeps the codebase maintained (deps, security, dead code, lint/type debt, flaky tests).

## Quick Facts

| Field | Value |
|-------|-------|
| Category | workflow |
| Version | 0.4.0 |
| Commands | `work`, `spec`, `ui`, `simplify`, `docs`, `upgrade`, `audit`, `prune`, `debt`, `deflake` |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |
| References | 10 command flows + 1 shared engine + 41 deep-dive guides |

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
| `/absolute spec [target]` | Plan | Lightweight standalone design spec — for when you want a doc to discuss or hand off, not build now. Codebase scan → bounded clarify pass (a few targeted questions, not a grill) → reviewed spec written to `docs/plans/` → independent scored review → stop. Chains into `work` when you're ready to build. |
| `/absolute ui [target]` | Design | Build polished, intentional UIs with concrete CSS/Tailwind values — typography, color, layout, spacing, dark mode, accessibility, animations, components. The most comprehensive UI knowledge base, not vague advice. |
| `/absolute simplify [target]` | Polish | Autonomously simplify your staged/unstaged git changes or a target path — reduce complexity, flatten nesting, remove redundancy and dead code — then run tests to prove nothing broke. |
| `/absolute docs [target]` | Document | Diátaxis-driven documentation: write, improve, or audit tutorials, how-tos, reference, explanation, and developer docs (README, CONTRIBUTING, ADRs). Detects the docs stack and verifies every claim against the code. |
| `/absolute upgrade [target]` | Health | Dependency upgrades: outdated/vulnerable deps planned into semver waves (patch/minor batched, majors gated and changelog-read), applied incrementally with lockfiles regenerated and tests green after each. |
| `/absolute audit [target]` | Health | Vulnerability & security scan (defensive, your own repo): dependency CVEs plus risky code patterns (secrets, injection, weak authz), severity × reachability triaged and remediated without suppressing. Complements built-in `/security-review`. |
| `/absolute prune [target]` | Health | Dead code & dependency cleanup, repo-wide: unused deps, unreferenced exports, unreachable code, orphaned files — removed only with tool evidence, in reversible waves. (Diff-scoped cleanup → `simplify`.) |
| `/absolute debt [target]` | Health | Lint & typecheck debt paydown: clear pre-existing repo-wide lint/type violations and suppressions (`@ts-ignore`, `# type: ignore`) one rule per wave, fixing causes not symptoms. (Diff-scoped quality → `simplify`.) |
| `/absolute deflake [target]` | Health | Flaky test fixes: detect nondeterministic tests empirically (repeat/shuffle/parallel runs), diagnose the root cause, fix it — never retry/skip/sleep — and verify across many randomized runs. |

### Usage

```bash
/absolute work "Add OAuth2 login with Google and GitHub providers"
/absolute work "Grill me on my plan for a real-time chat feature, then build it"
/absolute spec "Design a commenting system for blog posts"
/absolute spec "Draft a design doc for our rate limiter, don't build it yet"
/absolute ui "Design a pricing page for my SaaS"
/absolute ui "Make this dashboard feel less like AI slop"
/absolute simplify
/absolute simplify "clean up my changes in src/api/"
/absolute docs "Write a getting-started tutorial for the CLI"
/absolute docs "Audit our docs site and propose a restructure"
/absolute upgrade "Bring our dependencies up to date"
/absolute audit "Scan the repo for vulnerable deps and security issues"
/absolute prune "Remove dead code and unused dependencies"
/absolute debt "Clear our lint warnings and type errors"
/absolute deflake "Fix the flaky tests in our CI"
```

You can also describe the task and let the router pick the command:

```bash
/absolute "clean up my working changes"        # → simplify
/absolute "write a design spec for X"          # → spec
/absolute "write a README for this package"     # → docs
/absolute "are we vulnerable to any CVEs"       # → audit
/absolute "upgrade our dependencies"            # → upgrade
/absolute "our CI keeps flaking"                # → deflake
/absolute                                       # → shows the menu, recommends a command
```

## Overview

`absolute` is one coherent workflow engine spanning both building software and keeping it
healthy. A thin router (`SKILL.md`) resolves which command you want — by explicit name, by
intent, or by presenting a context-aware menu when called with no argument — then loads
that command's full playbook from `references/<command>.md` and follows it.

Each command keeps its own opinionated flow: `work` gates between every phase, `spec`
writes a reviewed design doc and stops, `docs` gates on the outline before writing prose,
and `simplify` stays bounded to scoped git changes. The five engineering-health commands
(`upgrade`, `audit`, `prune`, `debt`, `deflake`) share one loop — scan → risk-ranked gate
→ safe-wave fix → verify → report — and run on green `main`, distinct from `simplify`
(your working diff). None of them auto-commit — they write files and report; you commit.

### What makes this skill different

- **One install, one update.** `npx skills add maddhruv/absolute` brings all ten commands; updates pull everything at once.
- **Shared vocabulary.** `/absolute work | spec | ui | simplify | docs | upgrade | audit | prune | debt | deflake` is a single command surface your agent and team learn once.
- **Codebase before questions.** Every command reads what exists before asking, and verifies behavior instead of assuming it.
- **Gate before expensive work.** Hard gates and bounded scope keep you in control; no silent scope creep.

### Reference Guides

Command flows live in `references/<command>.md` (`work`, `spec`, `ui`, `simplify`, `docs`,
`upgrade`, `audit`, `prune`, `debt`, `deflake`). Each command's deep-dive guides are
namespaced under `references/<command>/` (e.g. `references/ui/typography.md`,
`references/work/execution-model.md`, `references/simplify/python.md`,
`references/docs/docs-stacks.md`) and load only when the task needs them. Two shared
references avoid duplication: `spec` reuses `references/work/spec-writing.md`, and the five
engineering-health commands share `references/health-engine.md` for their common loop.

## Tags

`workflow` `sdlc` `planning` `spec` `specification` `tdd` `ui` `design` `simplification` `refactoring` `documentation` `diataxis` `maintenance` `dependencies` `security` `dead-code` `linting` `flaky-tests`

## Platforms

- claude-code
- gemini-cli
- openai-codex
- mcp

## Maintainers

- [@maddhruv](https://github.com/maddhruv)

---

*Generated from [Absolute](https://github.com/maddhruv/absolute/tree/main/skills/absolute)*
