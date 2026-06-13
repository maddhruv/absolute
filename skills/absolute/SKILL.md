---
name: absolute
version: 0.4.0
description: >
  A focused development workflow engine for AI coding agents, invoked as
  `/absolute <command> [target]`. One skill, ten commands. Five cover the build loop
  you run every day — think → spec → plan → build → polish → document:
  `work` (end-to-end, phase-gated SDLC: relentless design interview → reviewed
  spec → dependency-graphed task board → safe-wave TDD execution → verification),
  `spec` (lightweight standalone design spec: codebase scan → bounded clarify pass
  → write a reviewed design doc to docs/plans → scored reviewer subagent → stop,
  no build),
  `ui` (polished, intentional interface design with concrete CSS/Tailwind values —
  typography, color, layout, dark mode, accessibility, animations),
  `simplify` (autonomously simplify your working git changes — reduce complexity,
  remove redundancy, keep tests green), and
  `docs` (Diátaxis-driven documentation — tutorials, how-tos, reference,
  explanation, READMEs, ADRs, with stack detection and code-verified accuracy).
  Five more keep the codebase healthy — standing maintenance on green main, each a
  risk-ranked, gated, safe-wave scan-and-fix:
  `upgrade` (dependency upgrades — outdated/vulnerable deps bumped in semver waves,
  majors gated and changelog-read, tests green after each),
  `audit` (vulnerability & security scan — dependency CVEs plus risky code patterns,
  severity-triaged and remediated; defensive, on your own repo),
  `prune` (dead code & dependency cleanup — unused deps, exports, files removed with
  evidence, repo-wide),
  `debt` (lint & typecheck debt paydown — clear pre-existing warnings/errors and
  suppressions by rule, no cheating the checker), and
  `deflake` (flaky test fixes — diagnose and fix the root nondeterminism, never
  retry/skip/sleep).
  Triggers on "absolute work|spec|ui|simplify|docs|upgrade|audit|prune|debt|deflake",
  "build this end-to-end", "plan and build", "grill me on this", "write a spec",
  "draft a design doc", "design this UI", "make this less like AI slop",
  "simplify this", "clean up my changes", "write docs", "audit our docs",
  "upgrade our dependencies", "are we vulnerable", "security audit", "remove dead
  code", "fix our lint/type errors", "fix flaky tests", or any multi-step
  development, design, cleanup, documentation, or maintenance task.
category: workflow
tags:
  - workflow
  - sdlc
  - planning
  - spec
  - specification
  - tdd
  - ui
  - design
  - simplification
  - refactoring
  - documentation
  - diataxis
  - maintenance
  - dependencies
  - security
  - dead-code
  - linting
  - flaky-tests
platforms:
  - claude-code
  - gemini-cli
  - openai-codex
  - mcp
user-invocable: true
argument-hint: "[work|spec|ui|simplify|docs|upgrade|audit|prune|debt|deflake] [target]"
license: MIT
maintainers:
  - github: maddhruv
---

# Absolute — Development Workflow Engine

One skill, ten commands, dispatched as `/absolute <command> [target]`. Two families:

**Build loop** (the everyday flow):
**think → plan → build** (`work`) → **spec only** (`spec`) → **design** (`ui`) → **polish** (`simplify`) → **document** (`docs`)

**Engineering health** (standing maintenance on green `main`):
`upgrade` (deps) · `audit` (security) · `prune` (dead code) · `debt` (lint/type) · `deflake` (flaky tests)

The body of each command lives in `references/<command>.md`. This file is the
router: it decides which command the user wants, loads that reference, and follows
it. The command references are long and opinionated — load only the one in play. The
five health commands share one loop in `references/health-engine.md` (loaded by each).

---

## Commands

| Command | Phase | What it does | Reference |
|---|---|---|---|
| `work [target]` | Think · Plan · Build | End-to-end, phase-gated SDLC: relentless design interview → reviewed spec → dependency-graphed task board → safe-wave TDD execution → verification → converge. Handles features, bugs, refactors, greenfield, planning breakdowns, migrations. | [references/work.md](references/work.md) |
| `spec [target]` | Plan | Lightweight standalone design spec — for when you want a doc to discuss or hand off, not build now. Codebase scan → bounded clarify pass (a few questions, not a grill) → write a reviewed spec to `docs/plans/` → independent scored review → stop. Reuses `work`'s spec template + rubric. | [references/spec.md](references/spec.md) |
| `ui [target]` | Design | Build polished, intentional UIs with concrete CSS/Tailwind values: typography, color, layout, spacing, dark mode, accessibility, animations, components. The most comprehensive UI knowledge base, not vague advice. | [references/ui.md](references/ui.md) |
| `simplify [target]` | Polish | Autonomously simplify your staged/unstaged git changes or a target path — reduce complexity, flatten nesting, remove redundancy and dead code — then run tests to prove nothing broke. | [references/simplify.md](references/simplify.md) |
| `docs [target]` | Document | Diátaxis-driven documentation: write, improve, or audit tutorials, how-tos, reference, explanation, and developer docs (README, CONTRIBUTING, ADRs). Detects the docs stack; verifies every claim against the code. | [references/docs.md](references/docs.md) |
| `upgrade [target]` | Health | Dependency upgrades: list outdated/vulnerable deps, plan them into semver waves (patch/minor batched, majors gated + changelog-read), apply incrementally, regenerate lockfiles, tests green after each. | [references/upgrade.md](references/upgrade.md) |
| `audit [target]` | Health | Vulnerability & security scan (defensive, your own repo): dependency CVEs + risky code patterns (secrets, injection, weak authz), severity × reachability triaged, remediated without suppressing. Complements built-in `/security-review`. | [references/audit.md](references/audit.md) |
| `prune [target]` | Health | Dead code & dependency cleanup, repo-wide: unused deps, unreferenced exports, unreachable code, orphaned files — removed only with tool evidence, in reversible waves. (Diff-scoped cleanup is `simplify`.) | [references/prune.md](references/prune.md) |
| `debt [target]` | Health | Lint & typecheck debt paydown: clear pre-existing repo-wide lint/type violations and suppressions (`@ts-ignore`, `# type: ignore`) one rule per wave, fixing causes not symptoms. (Diff-scoped quality is `simplify`.) | [references/debt.md](references/debt.md) |
| `deflake [target]` | Health | Flaky test fixes: detect nondeterministic tests empirically (repeat/shuffle/parallel runs), diagnose the root cause, fix it — never retry/skip/sleep — and verify across many randomized runs. | [references/deflake.md](references/deflake.md) |

---

## Routing rules

Resolve the command on every invocation, then hand off to its reference.

1. **First word matches a command** (`work`, `spec`, `ui`, `simplify`, `docs`,
   `upgrade`, `audit`, `prune`, `debt`, `deflake`) → read `references/<command>.md`
   and follow it exactly. Everything after the command name is the target/argument.

2. **First word doesn't match, but the intent clearly maps to one command** → load
   that command's reference and proceed as if it was invoked explicitly:
   - "build X end-to-end", "plan and build", "break this into tasks", "grill me on
     this plan", "pick up this ticket", "run this migration" → **work**
   - "write a spec", "spec out this feature", "draft a design doc for X", "I want a
     spec to hand off / review, don't build it yet" → **spec**
     (The fork vs `work`: `spec` writes a reviewed design doc and stops; `work` writes
     the doc *and* builds it. When unsure if they'll build now, prefer `spec` — it can
     chain into `work` afterward.)
   - "design a pricing page", "style this component", "make this less like AI
     slop", "fix the spacing/typography/color", "dark mode" → **ui**
   - "simplify this", "clean up my changes", "refactor this", "reduce complexity",
     "remove dead code", "tidy this up" → **simplify**
   - "write docs/a tutorial/a README", "document this", "improve this doc",
     "audit our docs", "restructure the documentation" → **docs**
   - "upgrade our dependencies", "bump deps", "update packages", "move off the
     deprecated X", "clear the Dependabot backlog" → **upgrade**
   - "security audit", "are we vulnerable", "scan for CVEs", "check for secrets/
     injection", "harden this" → **audit** (defensive, whole repo; pair with the
     built-in `/security-review` for the pending diff)
   - "remove dead code", "find unused deps/exports", "what can we delete", "clean up
     orphaned files" (repo-wide) → **prune**
   - "fix our lint warnings", "clear the type errors", "get to strict mode", "burn
     down `@ts-ignore`/suppressions" → **debt**
   - "fix flaky tests", "CI is flaky", "this test fails randomly/intermittently" → **deflake**

   Disambiguation: **`prune`/`debt`** act repo-wide on green `main`; **`simplify`**
   acts on *your working diff* — route by scope. **`audit`** (the command) scans the
   whole repo for security; "audit our docs" still → **docs**.

   If two commands genuinely fit, ask once which the user wants. Otherwise pick the
   clear match and state which command you loaded.

3. **No argument** (`/absolute` alone) → do not auto-run anything. Present the
   command table above, then recommend the 1–2 highest-value next commands based
   on context (e.g. uncommitted changes → `simplify`; a fresh ticket or vague
   goal → `work`, or `spec` if the user only wants a design doc to hand off; UI
   files in the diff → `ui`; missing or stale docs → `docs`; outdated/vulnerable
   deps in the manifest → `upgrade`/`audit`; failing or flaky CI → `deflake`).
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
  outline; `simplify` is bounded to scoped changes; `spec` stops at the reviewed doc
  and never starts building; the health commands (`upgrade`/`audit`/`prune`/`debt`/
  `deflake`) present risk-ranked findings and gate before applying any fix. Never
  silently expand scope.
- **Verify, don't assume.** Prove behavior with tests, real output, and code-checked
  facts — never aspirational claims.
- **Never auto-commit.** Every command writes files and reports; the user commits.
