# Absolute

**A development workflow engine for AI coding agents. Eleven separate skills — a one-time `absolute-init` (interview + stack detection → config), a build loop you run every day (`think → spec → plan → build → polish → document`), plus an engineering-health family that keeps the codebase maintained (deps, security, dead code, lint/type debt, flaky tests).**

> **Quick start:** install each skill via `npx skills add maddhruv/absolute-<name>`, then invoke it directly in your AI coding tool.

```bash
/absolute-init                                              # interview + detect stack → write config the commands read
/absolute-work "Add OAuth2 login with Google and GitHub"   # plan it, build it, verify it
/absolute-spec "Design a commenting system"                 # write a reviewed design doc, don't build yet
/absolute-ui "Design a pricing page"                        # make it look intentional, not generated
/absolute-simplify                                          # clean the diff before you commit
/absolute-docs "Write a getting-started tutorial"           # document what you shipped
/absolute-upgrade                                           # bring dependencies up to date, safely
/absolute-audit                                             # scan deps + code for vulnerabilities
/absolute-deflake                                           # fix the tests that fail randomly
```

---

## Why Absolute?

Most coding agents are great at the middle of a task and bad at the edges. They start writing code before the design is settled, ship UIs that scream "an AI made this," leave the diff messier than it needed to be, and write docs that drift from the code. You end up babysitting.

Absolute replaces that with **eleven opinionated skills** that cover the loop end to end:

- **It refuses to guess.** `absolute-work` interviews you to a shared design one question at a time, writes a reviewed spec, and decomposes the work into a dependency-graphed task board — *before* any code. Want just the design? `absolute-spec` writes a reviewed design doc and stops.
- **It builds safely.** Tasks run one wave at a time, test-first, with an independent evaluator grading each one. Blockers serialize; only provably-independent work parallelizes.
- **It has taste.** `absolute-ui` encodes concrete CSS/Tailwind values — spacing scales, color ratios, type scales, motion rules — not vague advice like "make it clean."
- **It cleans up after itself.** `absolute-simplify` reduces complexity in your actual git changes and runs your tests to prove nothing broke.
- **It tells the truth.** `absolute-docs` writes Diátaxis-correct documentation and verifies every snippet, flag, and default against your real code.
- **It keeps the codebase healthy.** Five maintenance skills run on green `main` — `absolute-upgrade` (dependency bumps), `absolute-audit` (security/CVE scan), `absolute-prune` (dead code & deps), `absolute-debt` (lint/type debt), `absolute-deflake` (flaky tests) — each a risk-ranked, gated, safe-wave scan-and-fix that never cheats the checker.
- **You stay in control.** Hard gates between phases, bounded scope, and **never auto-commits** — every skill writes files and reports; you commit.

---

## What's Included

Eleven skills, each installable independently. Works with any agent that supports the SKILL.md format: Claude Code, Gemini CLI, OpenAI Codex, Cursor, and [40+ more agents](#supported-agents).

**Setup** — run once per repo:

| Skill | Phase | What it does |
|---|---|---|
| [`/absolute-init`](skills/absolute-init/SKILL.md) | Setup | Interview how you want absolute to behave (output style, autonomy/gating, TDD strictness, spec dir, families) + detect the stack once, then write `.absolute.config.json` (project, committed) and optionally `~/.absolute/config.json` (user defaults + per-project overrides). Every other skill reads it instead of re-detecting; non-destructive, never commits. |

**Build loop** — the everyday flow:

| Skill | Phase | What it does |
|---|---|---|
| [`/absolute-work`](skills/absolute-work/SKILL.md) | Think · Plan · Build | End-to-end, phase-gated SDLC — brainstorm → spec → task board → safe-wave TDD execution → verify → converge. Handles features, bugs, refactors, greenfield, planning breakdowns, and migrations. |
| [`/absolute-spec`](skills/absolute-spec/SKILL.md) | Plan | Lightweight standalone design spec — codebase scan → bounded clarify pass → reviewed spec to `docs/plans/` → independent scored review → stop. For a doc to discuss or hand off, not build now. |
| [`/absolute-ui`](skills/absolute-ui/SKILL.md) | Design | Build polished, modern UIs with concrete values — spacing, color, typography, layout, dark mode, accessibility, animations, components. |
| [`/absolute-simplify`](skills/absolute-simplify/SKILL.md) | Polish | Autonomously simplify your working git changes — reduce complexity, flatten nesting, remove redundancy and dead code, keep tests green. |
| [`/absolute-docs`](skills/absolute-docs/SKILL.md) | Document | Diátaxis-driven docs — tutorials, how-tos, reference, explanation, plus READMEs, CONTRIBUTING, ADRs. Detects your docs stack; verifies every claim against the code. |

**Engineering health** — standing maintenance on green `main` (scan → risk-ranked gate → safe-wave fix → verify → report):

| Skill | What it does |
|---|---|
| [`/absolute-upgrade`](skills/absolute-upgrade/SKILL.md) | Dependency upgrades — outdated/vulnerable deps in semver waves (patch/minor batched, majors gated and changelog-read), lockfiles regenerated, tests green after each. |
| [`/absolute-audit`](skills/absolute-audit/SKILL.md) | Vulnerability & security scan (defensive, your own repo) — dependency CVEs plus risky code patterns, severity × reachability triaged and remediated without suppressing. Complements `/security-review`. |
| [`/absolute-prune`](skills/absolute-prune/SKILL.md) | Dead code & dependency cleanup, repo-wide — unused deps, unreferenced exports, unreachable code, orphaned files, removed only with tool evidence. |
| [`/absolute-debt`](skills/absolute-debt/SKILL.md) | Lint & typecheck debt paydown — clear pre-existing violations and suppressions one rule per wave, fixing causes not symptoms. |
| [`/absolute-deflake`](skills/absolute-deflake/SKILL.md) | Flaky test fixes — detect nondeterminism empirically, diagnose the root cause, fix it (never retry/skip/sleep), verify across many randomized runs. |

Behind the skills: **11 full command flows + 1 shared health engine + 41 deep-dive guides** (typography, color theory, the safe-wave execution algorithm, language-specific simplification catalogs, per-stack docs conventions, and more), loaded only when the task needs them.

---

## Usage

```bash
/absolute-init                                                 # one-time: interview + detect stack → config
/absolute-work "Pick up this ticket: migrate the REST API from v2 to v3"
/absolute-work "Grill me on my plan for a real-time chat feature, then build it"
/absolute-spec "Draft a design doc for our rate limiter, don't build it yet"
/absolute-ui "Make this dashboard feel less like AI slop"
/absolute-simplify "clean up my changes in src/api/"
/absolute-docs "Improve this README, it's 800 lines of everything"
/absolute-upgrade "Bring our dependencies up to date"
/absolute-prune "Remove dead code and unused dependencies"
/absolute-deflake "Fix the flaky tests in our CI"
```

### The `absolute-work` lifecycle

`absolute-work` stops at a hard gate between every phase, so you stay in control the whole way. State lives in a local `.absolute-work/board.md` that survives across sessions — resume, audit decisions, or hand off.

```
INTAKE & BRAINSTORM ─ gate ─ SPEC ─ gate ─ DECOMPOSE & PLAN ─ gate ─ EXECUTE ─ gate ─ VERIFY ─ gate ─ CONVERGE
```

---

## Installation

Install skills with the [`skills`](https://github.com/vercel-labs/skills) CLI via `npx`. Install all eleven at once or pick only what you need:

```bash
# Install all eleven skills
npx skills add maddhruv/absolute-init maddhruv/absolute-work maddhruv/absolute-spec \
  maddhruv/absolute-ui maddhruv/absolute-simplify maddhruv/absolute-docs \
  maddhruv/absolute-upgrade maddhruv/absolute-audit maddhruv/absolute-prune \
  maddhruv/absolute-debt maddhruv/absolute-deflake

# Or install individually
npx skills add maddhruv/absolute-work
npx skills add maddhruv/absolute-ui
```

Then invoke each skill by name (e.g. `/absolute-work`) and reload your agent if needed.

```bash
# Globally, available across all projects
npx skills add maddhruv/absolute-work -g

# To a specific agent
npx skills add maddhruv/absolute-work -a claude-code

# Non-interactive (CI/CD friendly)
npx skills add maddhruv/absolute-work -g -a claude-code -y
```

### Managing installs

```bash
npx skills list                      # list installed skills
npx skills check                     # check for updates
npx skills update                    # update everything
npx skills remove absolute-work      # remove a skill
```

| Scope | Flag | Location | Use case |
|---|---|---|---|
| **Project** | (default) | `./<agent>/skills/` | Committed with your project, shared with the team |
| **Global** | `-g` | `~/<agent>/skills/` | Available across all projects |

### Supported agents

The CLI supports 40+ agents including Claude Code, Codex, Cursor, Gemini CLI, GitHub Copilot, Windsurf, Cline, and Roo Code. See the full list in the [skills CLI docs](https://github.com/vercel-labs/skills#supported-agents).

---

## How it's built

```
skills/
  absolute-init/              # One-time setup
    SKILL.md                  # Full flow + frontmatter
    README.md
  absolute-work/              # End-to-end SDLC
    SKILL.md
    README.md
    references/               # intake-playbook, spec-writing, execution-model, etc.
  absolute-spec/              SKILL.md, README.md
  absolute-ui/                SKILL.md, README.md, references/ (25 design guides)
  absolute-simplify/          SKILL.md, README.md, references/ (language guides)
  absolute-docs/              SKILL.md, README.md, references/ (doc-type guides)
  absolute-upgrade/           SKILL.md, README.md, references/health-engine.md
  absolute-audit/             SKILL.md, README.md, references/health-engine.md
  absolute-prune/             SKILL.md, README.md, references/health-engine.md
  absolute-debt/              SKILL.md, README.md, references/health-engine.md
  absolute-deflake/           SKILL.md, README.md, references/health-engine.md
```

Each skill is self-contained. See [CONTRIBUTING.md](CONTRIBUTING.md) to edit a skill or add a new one.

---

## License

[MIT](LICENSE)
