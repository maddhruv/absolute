---
name: absolute
version: 0.2.0
description: >
  A focused development workflow engine for AI coding agents, invoked as
  `/absolute <command> [target]`. One skill, four commands covering the loop you
  run every day — think → plan → build → polish → document:
  `work` (end-to-end, phase-gated SDLC: relentless design interview → reviewed
  spec → dependency-graphed task board → safe-wave TDD execution → verification),
  `ui` (polished, intentional interface design with concrete CSS/Tailwind values —
  typography, color, layout, dark mode, accessibility, animations),
  `simplify` (autonomously simplify your working git changes — reduce complexity,
  remove redundancy, keep tests green), and
  `docs` (Diátaxis-driven documentation — tutorials, how-tos, reference,
  explanation, READMEs, ADRs, with stack detection and code-verified accuracy).
  Triggers on "absolute work|ui|simplify|docs", "build this end-to-end",
  "plan and build", "grill me on this", "design this UI", "make this less like AI
  slop", "simplify this", "clean up my changes", "write docs", "audit our docs",
  or any multi-step development, design, cleanup, or documentation task.
category: workflow
tags:
  - workflow
  - sdlc
  - planning
  - tdd
  - ui
  - design
  - simplification
  - refactoring
  - documentation
  - diataxis
platforms:
  - claude-code
  - gemini-cli
  - openai-codex
  - mcp
user-invocable: true
argument-hint: "[work|ui|simplify|docs] [target]"
license: MIT
maintainers:
  - github: maddhruv
---

# Absolute — Development Workflow Engine

One skill, four commands, dispatched as `/absolute <command> [target]`. Each
command owns a phase of the everyday development loop:

**think → plan → build** (`work`) → **design** (`ui`) → **polish** (`simplify`) → **document** (`docs`)

The body of each command lives in `references/<command>.md`. This file is the
router: it decides which command the user wants, loads that reference, and follows
it. The command references are long and opinionated — load only the one in play.

---

## Commands

| Command | Phase | What it does | Reference |
|---|---|---|---|
| `work [target]` | Think · Plan · Build | End-to-end, phase-gated SDLC: relentless design interview → reviewed spec → dependency-graphed task board → safe-wave TDD execution → verification → converge. Handles features, bugs, refactors, greenfield, planning breakdowns, migrations. | [references/work.md](references/work.md) |
| `ui [target]` | Design | Build polished, intentional UIs with concrete CSS/Tailwind values: typography, color, layout, spacing, dark mode, accessibility, animations, components. The most comprehensive UI knowledge base, not vague advice. | [references/ui.md](references/ui.md) |
| `simplify [target]` | Polish | Autonomously simplify your staged/unstaged git changes or a target path — reduce complexity, flatten nesting, remove redundancy and dead code — then run tests to prove nothing broke. | [references/simplify.md](references/simplify.md) |
| `docs [target]` | Document | Diátaxis-driven documentation: write, improve, or audit tutorials, how-tos, reference, explanation, and developer docs (README, CONTRIBUTING, ADRs). Detects the docs stack; verifies every claim against the code. | [references/docs.md](references/docs.md) |

---

## Routing rules

Resolve the command on every invocation, then hand off to its reference.

1. **First word matches a command** (`work`, `ui`, `simplify`, `docs`) → read
   `references/<command>.md` and follow it exactly. Everything after the command
   name is the target/argument.

2. **First word doesn't match, but the intent clearly maps to one command** → load
   that command's reference and proceed as if it was invoked explicitly:
   - "build X end-to-end", "plan and build", "break this into tasks", "grill me on
     this plan", "pick up this ticket", "run this migration" → **work**
   - "design a pricing page", "style this component", "make this less like AI
     slop", "fix the spacing/typography/color", "dark mode" → **ui**
   - "simplify this", "clean up my changes", "refactor this", "reduce complexity",
     "remove dead code", "tidy this up" → **simplify**
   - "write docs/a tutorial/a README", "document this", "improve this doc",
     "audit our docs", "restructure the documentation" → **docs**

   If two commands genuinely fit, ask once which the user wants. Otherwise pick the
   clear match and state which command you loaded.

3. **No argument** (`/absolute` alone) → do not auto-run anything. Present the
   command table above, then recommend the 1–2 highest-value next commands based
   on context (e.g. uncommitted changes → `simplify`; a fresh ticket or vague
   goal → `work`; UI files in the diff → `ui`; missing or stale docs → `docs`).
   The recommendation is a suggestion the user confirms.

4. **No clear command match at all** → treat the full input as a development task
   and load **work** (the default lifecycle), using the input as the brief.

Once a command reference is loaded, follow its own activation banner, gates, and
flow. Sub-command references do not re-invoke this router.

---

## Principles shared across commands

- **Codebase before questions.** Read what exists before asking; ask only what the
  code cannot answer.
- **Gate before expensive work.** `work` gates between phases; `docs` gates on the
  outline; `simplify` is bounded to scoped changes. Never silently expand scope.
- **Verify, don't assume.** Prove behavior with tests, real output, and code-checked
  facts — never aspirational claims.
- **Never auto-commit.** Every command writes files and reports; the user commits.
