<p align="center">
  <img src="https://avatars.githubusercontent.com/u/268155241?s=200&v=4" width="120" alt="AbsolutelySkilled logo" />
</p>

<h1 align="center">AbsolutelySkilled</h1>

<p align="center">
  A focused toolkit of production-ready skills for AI coding agents.
  <br />
  <a href="https://www.absolutelyskilled.pro/">www.absolutelyskilled.pro</a>
  <br /><br />
  <a href="https://skills.sh/registry/absolutelyskilled/absolutelyskilled">
    <img src="https://img.shields.io/badge/dynamic/json?url=https://raw.githubusercontent.com/AbsolutelySkilled/AbsolutelySkilled/main/data/installs.json&query=%24._total&label=total%20installs&color=brightgreen" alt="Total Installs" />
  </a>
</p>

---

## What is this?

AbsolutelySkilled is a focused toolkit that turns an AI coding agent into an autonomous development partner. Instead of a sprawling registry, it ships **four opinionated skills** that cover the loop you actually run every day: **think → plan → build → polish → document**.

Skills work with any agent that supports the SKILL.md format: Claude Code, Gemini CLI, OpenAI Codex, Cursor, and [40+ more agents](#supported-agents).

| Skill | What it does |
|---|---|
| [absolute-work](skills/absolute-work/) | End-to-end, phase-gated SDLC — brainstorm → spec → task board → safe-wave execution → verify |
| [absolute-ui](skills/absolute-ui/) | Build polished, modern UIs — spacing, color, typography, components, accessibility, animations |
| [absolute-simplify](skills/absolute-simplify/) | Autonomously simplify your working changes — reduce complexity, remove redundancy, keep tests green |
| [absolute-documentations](skills/absolute-documentations/) | Write, improve, and audit docs with Diátaxis — tutorials, how-tos, reference, explanation, READMEs, ADRs |

---

## The Skills

### Absolute Work — End-to-End Development Lifecycle

One continuous, phase-gated lifecycle that takes any unit of work — a ticket, task, plan, or migration — from fuzzy intent to verified code. It relentlessly interviews you to a shared design, writes and self-reviews a spec, decomposes the work into a dependency-graphed task board, then peels tasks off one safe wave at a time with test-first verification.

**Install:**

```bash
npx skills add AbsolutelySkilled/AbsolutelySkilled --skill absolute-work
```

**Usage:**

```bash
/absolute-work "Add OAuth2 login with Google and GitHub providers"
/absolute-work "Pick up this ticket: migrate the REST API from v2 to v3"
/absolute-work "Grill me on my plan for a real-time chat feature, then build it"
```

**How it works:**

1. **Intake & Brainstorm** — deep context scan, codebase-first questioning, adaptive question banks per work type, relentless one-at-a-time interview to mutual 100% confidence
2. **Spec** — writes the approved design to `docs/plans/`, then a separate reviewer subagent grades it on a scored rubric
3. **Decompose & Plan** — atomic tasks, a dependency graph, and safe-wave assignment on `.absolute-work/board.md`
4. **Execute** — onion-peel one wave at a time; blockers run sequentially, only provably-independent tasks parallelize; TDD per task
5. **Verify** — binary signals (test/lint/typecheck/build), then an independent scored evaluator
6. **Converge** — full suite, summary, close the board, suggest a commit (never auto-commit)

It **stops at a hard gate between every phase** so you stay in control, and tracks everything on a persistent local `.absolute-work/board.md` that survives across sessions — resume where you left off, audit decisions, or hand off to another developer.

```
INTAKE & BRAINSTORM ─ gate ─ SPEC ─ gate ─ DECOMPOSE & PLAN ─ gate ─ EXECUTE ─ gate ─ VERIFY ─ gate ─ CONVERGE
```

### Absolute UI — Polished Interface Design

The most comprehensive UI design knowledge base for AI coding agents — 25 reference guides covering typography, color theory, layout systems, dark mode, animations, accessibility, dashboards, and more. Every recommendation ships with concrete CSS/Tailwind values, not vague advice.

**Install:**

```bash
npx skills add AbsolutelySkilled/AbsolutelySkilled --skill absolute-ui
```

**Usage:**

```bash
/absolute-ui "Design a pricing page for my SaaS"
/absolute-ui "Make this dashboard feel less like AI slop"
```

### Absolute Simplify — Autonomous Code Simplification

Detects your staged or unstaged git changes, analyzes them for simplification opportunities following clean code and clean architecture principles, applies the improvements directly, runs your tests to verify nothing broke, and shows a structured summary with reasoning.

**Install:**

```bash
npx skills add AbsolutelySkilled/AbsolutelySkilled --skill absolute-simplify
```

**Usage:**

```bash
/absolute-simplify
/absolute-simplify "clean up my changes in src/api/"
```

### Absolute Documentations — Diátaxis-Driven Documentation

Writes, improves, and audits documentation using the Diátaxis framework — one quadrant per page (tutorial, how-to, reference, explanation), plus repo developer docs like READMEs, CONTRIBUTING guides, ADRs, changelogs, and runbooks. It detects your docs stack (Fumadocs, Docusaurus, Starlight, MkDocs, VitePress, Mintlify, or plain Markdown), writes in your project's own voice, gates on an outline before writing prose, and verifies every snippet, flag, and default against your actual code.

**Install:**

```bash
npx skills add AbsolutelySkilled/AbsolutelySkilled --skill absolute-documentations
```

**Usage:**

```bash
/absolute-documentations "Write a getting-started tutorial for the CLI"
/absolute-documentations "Improve this README, it's 800 lines of everything"
/absolute-documentations "Audit our docs site and propose a restructure"
```

---

## Install Skills

Skills are installed using the [`skills`](https://github.com/vercel-labs/skills) CLI via `npx`.

### Quick Start

```bash
# Install all skills from this toolkit
npx skills add AbsolutelySkilled/AbsolutelySkilled

# List available skills before installing
npx skills add AbsolutelySkilled/AbsolutelySkilled --list

# Install a specific skill
npx skills add AbsolutelySkilled/AbsolutelySkilled --skill absolute-work

# Install multiple specific skills
npx skills add AbsolutelySkilled/AbsolutelySkilled --skill absolute-work --skill absolute-ui

# Install globally (available across all projects)
npx skills add AbsolutelySkilled/AbsolutelySkilled -g

# Install to a specific agent
npx skills add AbsolutelySkilled/AbsolutelySkilled -a claude-code

# Non-interactive install (CI/CD friendly)
npx skills add AbsolutelySkilled/AbsolutelySkilled --skill absolute-work -g -a claude-code -y

# Install all skills to all agents
npx skills add AbsolutelySkilled/AbsolutelySkilled --all
```

### Install Options

| Option | Description |
|---|---|
| `-g, --global` | Install to user directory instead of project |
| `-a, --agent <agents...>` | Target specific agents (e.g., `claude-code`, `codex`, `cursor`) |
| `-s, --skill <skills...>` | Install specific skills by name (use `'*'` for all) |
| `-l, --list` | List available skills without installing |
| `--copy` | Copy files instead of symlinking |
| `-y, --yes` | Skip all confirmation prompts |
| `--all` | Install all skills to all agents without prompts |

### Installation Scope

| Scope | Flag | Location | Use Case |
|---|---|---|---|
| **Project** | (default) | `./<agent>/skills/` | Committed with your project, shared with team |
| **Global** | `-g` | `~/<agent>/skills/` | Available across all projects |

### Managing Skills

```bash
# List installed skills
npx skills list

# Search for skills
npx skills find <query>

# Check for updates
npx skills check

# Update all installed skills
npx skills update

# Remove a skill
npx skills remove <skill-name>
```

### Supported Agents

The CLI supports 40+ agents including Claude Code, Codex, Cursor, Gemini CLI, GitHub Copilot, Windsurf, Cline, Roo Code, and many more. See the full list in the [skills CLI documentation](https://github.com/vercel-labs/skills#supported-agents).

## Structure

```
skills/
  <skill-name>/
    SKILL.md           # Core skill content (under 500 lines)
    README.md          # Skill landing page (powers the website)
    evals.json         # Test suite validating the skill works
    references/        # Deep-dive files loaded on demand
      <topic>.md
website/               # Astro + Tailwind site, reads skills/*/README.md
```

## Recommended Workflow

The four skills are designed to chain into one loop:

1. **Think & Plan & Build** → `absolute-work` takes the work from idea to verified code
2. **Design the UI** → `absolute-ui` makes the interface look intentional, not generated
3. **Polish** → `absolute-simplify` cleans up the diff before you commit
4. **Document** → `absolute-documentations` writes the docs for what you shipped

## License

[MIT](LICENSE)
