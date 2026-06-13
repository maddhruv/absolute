# Absolute

**A development workflow engine for AI coding agents. One skill, four commands, the whole loop you run every day — `think → plan → build → polish → document`.**

> **Quick start:** `npx skills add maddhruv/absolute`, then run `/absolute work` (or `ui`, `simplify`, `docs`) inside your AI coding tool.

```bash
/absolute work "Add OAuth2 login with Google and GitHub"   # plan it, build it, verify it
/absolute ui "Design a pricing page"                         # make it look intentional, not generated
/absolute simplify                                           # clean the diff before you commit
/absolute docs "Write a getting-started tutorial"            # document what you shipped
```

---

## Why Absolute?

Most coding agents are great at the middle of a task and bad at the edges. They start writing code before the design is settled, ship UIs that scream "an AI made this," leave the diff messier than it needed to be, and write docs that drift from the code. You end up babysitting.

Absolute replaces that with **one opinionated workflow engine** that covers the loop end to end:

- **It refuses to guess.** `work` interviews you to a shared design one question at a time, writes a reviewed spec, and decomposes the work into a dependency-graphed task board — *before* any code.
- **It builds safely.** Tasks run one wave at a time, test-first, with an independent evaluator grading each one. Blockers serialize; only provably-independent work parallelizes.
- **It has taste.** `ui` encodes concrete CSS/Tailwind values — spacing scales, color ratios, type scales, motion rules — not vague advice like "make it clean."
- **It cleans up after itself.** `simplify` reduces complexity in your actual git changes and runs your tests to prove nothing broke.
- **It tells the truth.** `docs` writes Diátaxis-correct documentation and verifies every snippet, flag, and default against your real code.
- **You stay in control.** Hard gates between phases, bounded scope, and it **never auto-commits** — every command writes files and reports; you commit.

One install. One update. One command surface (`/absolute …`) your agent and your team learn once.

---

## What's Included

A single skill, `absolute`, that dispatches four commands through a thin router. It works with any agent that supports the SKILL.md format: Claude Code, Gemini CLI, OpenAI Codex, Cursor, and [40+ more agents](#supported-agents).

| Command | Phase | What it does |
|---|---|---|
| [`/absolute work`](skills/absolute/references/work.md) | Think · Plan · Build | End-to-end, phase-gated SDLC — brainstorm → spec → task board → safe-wave TDD execution → verify → converge. Handles features, bugs, refactors, greenfield, planning breakdowns, and migrations. |
| [`/absolute ui`](skills/absolute/references/ui.md) | Design | Build polished, modern UIs with concrete values — spacing, color, typography, layout, dark mode, accessibility, animations, components. |
| [`/absolute simplify`](skills/absolute/references/simplify.md) | Polish | Autonomously simplify your working git changes — reduce complexity, flatten nesting, remove redundancy and dead code, keep tests green. |
| [`/absolute docs`](skills/absolute/references/docs.md) | Document | Diátaxis-driven docs — tutorials, how-tos, reference, explanation, plus READMEs, CONTRIBUTING, ADRs. Detects your docs stack; verifies every claim against the code. |

Behind each command: **4 command flows + 41 deep-dive guides** (typography, color theory, the safe-wave execution algorithm, language-specific simplification catalogs, per-stack docs conventions, and more), loaded only when the task needs them.

---

## Usage

Call a command directly:

```bash
/absolute work "Pick up this ticket: migrate the REST API from v2 to v3"
/absolute work "Grill me on my plan for a real-time chat feature, then build it"
/absolute ui "Make this dashboard feel less like AI slop"
/absolute simplify "clean up my changes in src/api/"
/absolute docs "Improve this README, it's 800 lines of everything"
/absolute docs "Audit our docs site and propose a restructure"
```

Or just describe the task and let the router pick the command — and run `/absolute` alone to get a context-aware menu:

```bash
/absolute "clean up my working changes"       # → simplify
/absolute "write a README for this package"    # → docs
/absolute                                      # → shows the menu, recommends a command
```

### The `work` lifecycle

`work` stops at a hard gate between every phase, so you stay in control the whole way. State lives in a local `.absolute-work/board.md` that survives across sessions — resume, audit decisions, or hand off.

```
INTAKE & BRAINSTORM ─ gate ─ SPEC ─ gate ─ DECOMPOSE & PLAN ─ gate ─ EXECUTE ─ gate ─ VERIFY ─ gate ─ CONVERGE
```

---

## Installation

Absolute installs with the [`skills`](https://github.com/vercel-labs/skills) CLI via `npx`. One command brings all four commands:

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
    work.md, ui.md, simplify.md, docs.md   # one flow per command
    work/, ui/, simplify/, docs/            # each command's deep-dive guides
```

The router resolves your command — by explicit name, by intent, or via a menu — then loads that command's full playbook from `references/<command>.md`. See [CONTRIBUTING.md](CONTRIBUTING.md) to edit a command or add a new one.

---

## License

[MIT](LICENSE)
