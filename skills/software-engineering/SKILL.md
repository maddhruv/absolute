---
name: software-engineering
version: 0.1.0
description: >
  Use this skill for ANY software or code work - building features, fixing bugs,
  refactoring, designing systems, scaffolding projects, writing or modifying code,
  planning implementations, or making architectural decisions. Triggers on feature
  requests, "build this", "add X", "implement", "refactor", "fix this bug", "design",
  "plan and build", "break this into tasks", "build end-to-end", greenfield projects,
  and migrations. Adapts depth to complexity: a light path for trivial fixes, and a
  full design -> decompose -> parallel-execute -> verify lifecycle for complex work.
  Combines a relentless design interview (codebase-first, ultrathink, validated spec)
  with AI-native execution (dependency-graphed waves, TDD, generator-evaluator review).
category: engineering
tags: [sdlc, design, planning, parallel-execution, tdd, architecture, workflow]
recommended_skills: [writing-plans, system-design, clean-architecture, code-review-mastery, test-strategy]
platforms:
  - claude-code
  - gemini-cli
  - openai-codex
  - mcp
sources:
  - url: https://github.com/AbsolutelySkilled/AbsolutelySkilled
    accessed: 2026-05-26
    description: Merge of absolute-brainstorm (design interview + spec) and absolute-human (decompose, parallel waves, TDD verification) into one adaptive software-engineering lifecycle
license: MIT
maintainers:
  - github: maddhruv
---

When this skill is activated, always start your first response with the ⚙️ emoji.

# Software Engineering

A single, adaptive lifecycle for all code work. It fuses two stages: **DESIGN** -
a relentless, codebase-first interview that turns a vague ask into a validated spec
a staff engineer would approve - and **BUILD** - AI-native execution that decomposes
the spec into a dependency graph, runs independent tasks in parallel waves, and proves
correctness with TDD and generator-evaluator verification. The depth of each stage
scales to the task: a typo fix skips most of it; a greenfield system runs the whole
pipeline. The goal is the same at every size - no unexamined assumptions, no unverified
"done".

---

## When to use this skill

Trigger this skill when the user:
- Asks to build, add, implement, or scaffold a feature, component, or project
- Wants to fix a bug, refactor, or modify existing behavior
- Asks to design a system, choose an architecture, or make a technical decision
- Says "plan and build", "break this into tasks", "build this end-to-end", or "sprint plan this"
- Starts a greenfield project or plans a migration
- Writes or changes code of any size - this is the default skill for software work

Do NOT trigger this skill for:
- Pure questions, code explanations, or research with no change to make
- Tasks the user explicitly wants to drive manually with you only advising

---

## Activation Protocol

**At the very start of every invocation**, before other output, print this banner:

```
╔══════════════════════════════════════════════╗
║   S O F T W A R E   E N G I N E E R I N G      ║
║   adaptive design -> build lifecycle           ║
╚══════════════════════════════════════════════╝
```

Then **enter plan mode** before touching any file:
- On platforms with native plan mode (Claude Code `EnterPlanMode`, Gemini CLI planning): invoke it now.
- Without native plan mode: complete the DESIGN stage and present a plan for explicit approval before any code change.

All design and decomposition is planning work. No files are created or modified until
the user approves and BUILD begins. Use extended thinking (ultrathink) on every design
decision, approach proposal, and verification judgment.

---

## Complexity Triage (the router)

Before anything else, classify the task. This decides how much of the pipeline runs.
When in doubt, size up - the cost of over-planning a small task is minutes; the cost of
under-planning a large one is a rewrite.

| Tier | Looks like | DESIGN | BUILD |
|---|---|---|---|
| **Trivial** | typo, one-line fix, rename, obvious config change | Skip interview. State the one-sentence plan and confirm. | Direct implementation + a test that proves it. No board, no DAG. |
| **Small** | single component, clear scope, 1-3 files | 2-3 targeted questions, inline design (a few sentences). | Sequential TDD: test -> implement -> verify. Lightweight board optional. |
| **Medium** | multi-component, some ambiguity, 3-8 files | Full interview (5-7 questions) + written spec. | Build a DAG, run waves, per-task verification. |
| **Complex** | cross-cutting, greenfield, migration, 8+ files | Deep interview (8-10+ questions), spec + spec-review loop. | Full DAG + parallel waves + generator-evaluator + CONVERGE. |

Announce the tier you picked and why. The user can override - if they say "just do it"
on something you'd size as Medium, drop to the Small path but still write a test.

For trivial and small tasks, the rest of this document is mostly skippable - resolve the
obvious design inline and implement. The heavy machinery below is for Medium and Complex.

---

## Stage A: DESIGN

Turn the request into a design everyone is confident in. Search the codebase before
asking anything it can already answer.

1. **Deep context scan** - read `docs/` (README first), root `README.md`, `CLAUDE.md`,
   `CONTRIBUTING.md`, `docs/plans/` for overlapping specs, recent commits, and package
   manifests. Synthesize what matters; don't dump a file listing. Be selective in large
   repos to protect the context window.
2. **Codebase-first intelligence** - before every question, search the code for the
   answer (stack, auth, test framework, conventions). Only ask what code genuinely can't
   tell you (aesthetics, product calls, priorities). Tell the user what you found.
3. **Scope assessment** - if the request spans multiple independent subsystems, decompose
   into sub-projects first and design the first one.
4. **Relentless interview** - use `AskUserQuestion` (one question at a time, strictly
   linear, dependency-resolved). Walk the design tree depth-first. Prefer multiple choice;
   mark one option **(Recommended)** with rationale; only offer real forks, not fake ones.
   Continue until both you and the user are fully confident - probe any hesitation.
5. **Confidence self-check** - ultrathink every decision before presenting. If any is
   below full confidence or rests on an unverified assumption, return to the interview.
6. **Design presentation** - section by section (architecture, components, data flow,
   error handling, testing), getting approval per section. Scale length to complexity.
7. **Write the spec** - save to `docs/plans/YYYY-MM-DD-<topic>-design.md`. Confirm the
   target directory in monorepos.
8. **Spec review loop** (Medium/Complex) - dispatch a *separate* reviewer subagent with a
   scored rubric (Completeness, Consistency, Clarity, Scope, Testability). 4.0+/5 passes;
   3.0-3.9 fix and re-run (max 3); below 3.0 surface to the user. Reviewer approval never
   replaces user approval.

Detailed guidance: `references/interview-playbook.md`, `references/approach-analysis.md`,
`references/spec-writing.md`.

---

## Stage B: BUILD

Once the design is approved (or for small tasks, once the inline plan is confirmed),
execute. The full model has 7 phases: **INTAKE -> DECOMPOSE -> DISCOVER -> PLAN ->
EXECUTE -> VERIFY -> CONVERGE**. Lower tiers collapse phases.

State lives in `.software-engineering/board.md` at the project root - the single source
of truth that survives across sessions. Ask once whether it should be git-tracked (audit
trail) or gitignored (local). If a board already exists, read it, summarize status, and
resume from the last incomplete wave rather than restarting. Never overwrite a board
without explicit confirmation. See `references/board-format.md`.

1. **INTAKE** - the approved spec (or inline plan) is the intake. Auto-detect project
   conventions (package manager, language, test runner, linter, build, CI) and record
   them on the board; reference them in every later phase.
2. **DECOMPOSE** - break work into atomic sub-tasks (ID, title, type, S/M complexity,
   dependencies). No L tasks - split them. Build a DAG, verify it's acyclic, assign
   waves by depth. Every graph ends with three mandatory tail tasks: self code review,
   requirements validation, full project verification. See
   `references/dependency-graph-patterns.md`.
3. **DISCOVER** - research each task docs-first then source: find reusable code,
   conventions, and risks. Parallelizable per wave.
4. **PLAN** - per task, specify files to create/modify, test files (written first),
   approach, acceptance criteria, and test cases.
5. **EXECUTE** - record a git rollback point *before* the first file changes. Then run
   wave by wave; within a wave, parallel agents handle independent tasks via TDD
   (red -> green -> refactor). Identify shared files during DECOMPOSE and assign single
   ownership to avoid wave-boundary conflicts. See `references/wave-execution.md` and
   `references/execution-patterns.md`.
6. **VERIFY** - signals first (tests, lint, type check, build); fix failures before
   judging. Then generator-evaluator separation: a *different* subagent grades against a
   scored rubric (4.0+/5 passes). S-tasks may skip the evaluator if all signals pass;
   M and any failed task get the full evaluator. See `references/verification-framework.md`
   and `references/evaluator-protocol.md`.
7. **CONVERGE** - merge, run the full suite, update docs in scope, write a change summary,
   mark the board completed, and propose a commit message.

For intentional context resets in long sessions (3+ waves), see
`references/context-reset-protocol.md`.

---

## Key principles

- **Adaptive depth** - match rigor to complexity; don't force a typo through a DAG, and
  don't wing a migration. Announce the tier and let the user override.
- **Codebase before questions** - respect the user's time; only ask genuine unknowns.
- **One question at a time** - strictly linear, dependency-resolved, via `AskUserQuestion`.
- **Honest options** - real forks get multiple approaches, each with a **(Recommended)**.
- **Test-first** - a task is done only when its tests pass. No exceptions for "simple".
- **Generator != evaluator** - the agent that builds a task does not grade it.
- **Persistent state** - the board is the source of truth; resume, don't restart.
- **Mutual confidence** - never advance a decision while you or the user have doubt.
- **YAGNI** - cut unnecessary features from every design.

---

## Gotchas

1. **Skipping triage and defaulting to the heavy pipeline** - running a full interview +
   DAG for a one-line fix wastes the user's time and trains them to bypass the skill.
   Classify first; most tasks are Trivial or Small.

2. **Asking what the codebase already answers** - "what database?" when `schema.prisma`
   is right there erodes trust. Search before every question.

3. **Board marked done but tests never ran** - the mandatory "full project verification"
   tail task gets skipped on subjective confidence. Never mark the board completed until
   the actual test/lint/build commands have run and their output is recorded.

4. **Rollback point recorded mid-wave** - capturing the git hash after Wave 1 started
   bakes partial changes into the "clean" point. Snapshot before any file is touched.

5. **Parallel agents editing a shared file** - two tasks in one wave that both edit a
   utility or fixture collide at the boundary. Assign single ownership during DECOMPOSE.

6. **Scope creep absorbed silently** - adjacent improvements discovered mid-build expand
   the work invisibly and break the scope agreement. Everything outside the original
   scope goes to a Deferred Work list, surfaced to the user.

7. **Reviewer approval treated as user approval** - the spec reviewer and the evaluator
   subagent reduce risk but don't replace the user's sign-off on design and result.

8. **DISCOVER skipped for "obvious" tasks** - assuming codebase familiarity leads to
   duplicated utilities and wrong import paths. A 2-minute scan beats a rewrite.

---

## Anti-patterns

For the full anti-pattern tables - design-interview anti-patterns and execution/scope/
convention anti-patterns - see `references/interview-playbook.md` (Anti-Patterns) and
`references/execution-patterns.md`.

---

## References

Load a reference only when the current phase needs it - they are long.

**Design stage:**
- `references/interview-playbook.md` - design-tree traversal, question banks by project type, codebase-first patterns, calibration, anti-patterns
- `references/approach-analysis.md` - when to propose multiple approaches, trade-off dimensions, project decomposition, decision trees
- `references/spec-writing.md` - spec template, section scaling, decision log, spec review rubric, example spec

**Build stage:**
- `references/dependency-graph-patterns.md` - DAG patterns, ASCII rendering, wave-assignment algorithm
- `references/wave-execution.md` - parallel agent orchestration, prompt templates, blocked-task and error recovery
- `references/execution-patterns.md` - agent handoff, wave-boundary checks, scope-creep handling, 16-row anti-patterns table
- `references/verification-framework.md` - TDD per task, verification signals, evaluator integration
- `references/evaluator-protocol.md` - generator-evaluator separation, scored rubric, sprint contracts, refinement loop
- `references/board-format.md` - full `.software-engineering/board.md` spec, status transitions, example board
- `references/context-reset-protocol.md` - when/how to reset context in long sessions, handoff format, resume

---

## Companion check

> On first activation in a conversation: check which companion skills are installed by running `ls ~/.claude/skills/ ~/.agents/skills/ .claude/skills/ .agents/skills/ 2>/dev/null`. Compare against the `recommended_skills` field. For any missing, mention them once and offer to install:
> ```
> npx skills add AbsolutelySkilled/AbsolutelySkilled --skill <name>
> ```
> Skip entirely if `recommended_skills` is empty or all companions are already installed.
