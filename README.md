# Absolute

**A development workflow engine for AI coding agents. One skill, ten commands — a build loop you run every day (`think → spec → plan → build → polish → document`) plus an engineering-health family that keeps the codebase maintained (deps, security, dead code, lint/type debt, flaky tests).**

> **Quick start:** `npx skills add maddhruv/absolute`, then run `/absolute work` (or `spec`, `ui`, `simplify`, `docs`, `upgrade`, `audit`, `prune`, `debt`, `deflake`) inside your AI coding tool.

```bash
/absolute work "Add OAuth2 login with Google and GitHub"   # plan it, build it, verify it
/absolute spec "Design a commenting system"                  # write a reviewed design doc, don't build yet
/absolute ui "Design a pricing page"                         # make it look intentional, not generated
/absolute simplify                                           # clean the diff before you commit
/absolute docs "Write a getting-started tutorial"            # document what you shipped
/absolute upgrade                                            # bring dependencies up to date, safely
/absolute audit                                              # scan deps + code for vulnerabilities
/absolute deflake                                            # fix the tests that fail randomly
```

---

## Why Absolute?

Most coding agents are great at the middle of a task and bad at the edges. They start writing code before the design is settled, ship UIs that scream "an AI made this," leave the diff messier than it needed to be, and write docs that drift from the code. You end up babysitting.

Absolute replaces that with **one opinionated workflow engine** that covers the loop end to end:

- **It refuses to guess.** `work` interviews you to a shared design one question at a time, writes a reviewed spec, and decomposes the work into a dependency-graphed task board — *before* any code. Want just the design? `spec` writes a reviewed design doc and stops.
- **It builds safely.** Tasks run one wave at a time, test-first, with an independent evaluator grading each one. Blockers serialize; only provably-independent work parallelizes.
- **It has taste.** `ui` encodes concrete CSS/Tailwind values — spacing scales, color ratios, type scales, motion rules — not vague advice like "make it clean."
- **It cleans up after itself.** `simplify` reduces complexity in your actual git changes and runs your tests to prove nothing broke.
- **It tells the truth.** `docs` writes Diátaxis-correct documentation and verifies every snippet, flag, and default against your real code.
- **It keeps the codebase healthy.** Five maintenance commands run on green `main` — `upgrade` (dependency bumps), `audit` (security/CVE scan), `prune` (dead code & deps), `debt` (lint/type debt), `deflake` (flaky tests) — each a risk-ranked, gated, safe-wave scan-and-fix that never cheats the checker.
- **You stay in control.** Hard gates between phases, bounded scope, and it **never auto-commits** — every command writes files and reports; you commit.

One install. One update. One command surface (`/absolute …`) your agent and your team learn once.

---

## What's Included

A single skill, `absolute`, that dispatches ten commands through a thin router. It works with any agent that supports the SKILL.md format: Claude Code, Gemini CLI, OpenAI Codex, Cursor, and [40+ more agents](#supported-agents).

**Build loop** — the everyday flow:

| Command | Phase | What it does |
|---|---|---|
| [`/absolute work`](skills/absolute/references/work.md) | Think · Plan · Build | End-to-end, phase-gated SDLC — brainstorm → spec → task board → safe-wave TDD execution → verify → converge. Handles features, bugs, refactors, greenfield, planning breakdowns, and migrations. |
| [`/absolute spec`](skills/absolute/references/spec.md) | Plan | Lightweight standalone design spec — codebase scan → bounded clarify pass → reviewed spec to `docs/plans/` → independent scored review → stop. For a doc to discuss or hand off, not build now. |
| [`/absolute ui`](skills/absolute/references/ui.md) | Design | Build polished, modern UIs with concrete values — spacing, color, typography, layout, dark mode, accessibility, animations, components. |
| [`/absolute simplify`](skills/absolute/references/simplify.md) | Polish | Autonomously simplify your working git changes — reduce complexity, flatten nesting, remove redundancy and dead code, keep tests green. |
| [`/absolute docs`](skills/absolute/references/docs.md) | Document | Diátaxis-driven docs — tutorials, how-tos, reference, explanation, plus READMEs, CONTRIBUTING, ADRs. Detects your docs stack; verifies every claim against the code. |

**Engineering health** — standing maintenance on green `main` (scan → risk-ranked gate → safe-wave fix → verify → report):

| Command | What it does |
|---|---|
| [`/absolute upgrade`](skills/absolute/references/upgrade.md) | Dependency upgrades — outdated/vulnerable deps in semver waves (patch/minor batched, majors gated and changelog-read), lockfiles regenerated, tests green after each. |
| [`/absolute audit`](skills/absolute/references/audit.md) | Vulnerability & security scan (defensive, your own repo) — dependency CVEs plus risky code patterns, severity × reachability triaged and remediated without suppressing. Complements `/security-review`. |
| [`/absolute prune`](skills/absolute/references/prune.md) | Dead code & dependency cleanup, repo-wide — unused deps, unreferenced exports, unreachable code, orphaned files, removed only with tool evidence. |
| [`/absolute debt`](skills/absolute/references/debt.md) | Lint & typecheck debt paydown — clear pre-existing violations and suppressions one rule per wave, fixing causes not symptoms. |
| [`/absolute deflake`](skills/absolute/references/deflake.md) | Flaky test fixes — detect nondeterminism empirically, diagnose the root cause, fix it (never retry/skip/sleep), verify across many randomized runs. |

Behind the commands: **10 command flows + 1 shared health engine + 41 deep-dive guides** (typography, color theory, the safe-wave execution algorithm, language-specific simplification catalogs, per-stack docs conventions, and more), loaded only when the task needs them. The five health commands share `references/health-engine.md`; `spec` reuses `work`'s spec template.

---

## Usage

Call a command directly:

```bash
/absolute work "Pick up this ticket: migrate the REST API from v2 to v3"
/absolute work "Grill me on my plan for a real-time chat feature, then build it"
/absolute spec "Draft a design doc for our rate limiter, don't build it yet"
/absolute ui "Make this dashboard feel less like AI slop"
/absolute simplify "clean up my changes in src/api/"
/absolute docs "Improve this README, it's 800 lines of everything"
/absolute upgrade "Bring our dependencies up to date"
/absolute prune "Remove dead code and unused dependencies"
/absolute deflake "Fix the flaky tests in our CI"
```

Or just describe the task and let the router pick the command — and run `/absolute` alone to get a context-aware menu:

```bash
/absolute "clean up my working changes"       # → simplify
/absolute "write a design spec for X"          # → spec
/absolute "write a README for this package"    # → docs
/absolute "are we vulnerable to any CVEs"      # → audit
/absolute "our CI keeps flaking"               # → deflake
/absolute                                      # → shows the menu, recommends a command
```

### The `work` lifecycle

`work` stops at a hard gate between every phase, so you stay in control the whole way. State lives in a local `.absolute-work/board.md` that survives across sessions — resume, audit decisions, or hand off.

```
INTAKE & BRAINSTORM ─ gate ─ SPEC ─ gate ─ DECOMPOSE & PLAN ─ gate ─ EXECUTE ─ gate ─ VERIFY ─ gate ─ CONVERGE
```

---

## Installation

Absolute installs with the [`skills`](https://github.com/vercel-labs/skills) CLI via `npx`. One command brings all ten commands:

```bash
npx skills add maddhruv/absolute
```

Then invoke it as `/absolute <command>` and reload your agent if needed.

```bash
# Globally, available across all projects
npx skills add maddhruv/absolute -g

# To a specific agent
npx skills add maddhruv/absolute -a claude-code

# Non-interactive (CI/CD friendly)
npx skills add maddhruv/absolute -g -a claude-code -y
```

### Managing the install

```bash
npx skills list             # list installed skills
npx skills check            # check for updates
npx skills update           # update everything
npx skills remove absolute  # remove it
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
skills/absolute/
  SKILL.md                 # The router: routing rules + command table
  README.md                # Skill landing page
  references/
    work.md, spec.md, ui.md, simplify.md, docs.md         # build-loop command flows
    upgrade.md, audit.md, prune.md, debt.md, deflake.md   # engineering-health command flows
    health-engine.md                        # shared loop for the 5 health commands
    work/, ui/, simplify/, docs/            # each command's deep-dive guides
```

The router resolves your command — by explicit name, by intent, or via a menu — then loads that command's full playbook from `references/<command>.md`. See [CONTRIBUTING.md](CONTRIBUTING.md) to edit a command or add a new one.

---

## License

[MIT](LICENSE)
