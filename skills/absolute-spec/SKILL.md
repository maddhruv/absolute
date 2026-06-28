---
name: absolute-spec
version: 0.5.0
description: >
  Lightweight standalone design spec for AI coding agents: codebase scan → bounded
  clarify pass (3–5 questions, not a grill) → reviewed design doc written to
  docs/plans/ → independent scored review → stop. No task board, no build.
  Use when you want a spec to discuss, hand off, or review before committing to
  implementation. Chains into absolute-work when ready to build.
  Triggers on "absolute spec", "write a spec", "spec out this feature",
  "draft a design doc", "I want a spec to hand off / review, don't build it yet".
category: workflow
tags:
  - workflow
  - spec
  - specification
  - planning
  - design
platforms:
  - claude-code
  - gemini-cli
  - openai-codex
  - mcp
user-invocable: true
argument-hint: "[target]"
license: MIT
maintainers:
  - github: maddhruv
---

> Start your first response with the 📋 emoji.

## Absolute Spec

Absolute Spec turns a fuzzy intent into a **reviewed design-spec document** — then
stops. It is the planning artifact of `work`, lifted out of the full lifecycle: scan
the codebase, ask only the questions the code can't answer, write the spec, run an
independent scored review, and hand back a doc you can discuss, hand off, or feed into
`/absolute work` to build.

It is deliberately **lightweight**. No relentless interview, no task board, no
decompose, no execution. One deliverable: a spec good enough that an unfamiliar
developer could build from it.

---

## When to use this command

**Use `spec` when:**
- You want a design spec to **discuss, review, or hand off** — not build right now.
- You need to think a feature through on paper before committing to implementation.
- A teammate or another agent will do the building from your spec.
- You want a fast, reviewed design doc without the full phase-gated lifecycle.

**Do NOT use `spec` when:**
- You want to **design AND build** in one flow → use **`/absolute work`** (spec is its
  Phase 2, followed by decompose + safe-wave execution).
- You're documenting code that **already exists / already shipped** → use
  **`/absolute docs`** (reference/explanation docs, not a forward design).
- The change is a one-line obvious fix needing no design.

The line vs `work`: **`spec` produces a doc and stops; `work` produces a doc and then
builds it.** If you're unsure whether you'll build it now, start with `spec` — you can
chain into `work` afterward and it will pick the spec up.

---

## Key Principles

1. **Codebase before questions.** Read what exists first; ask only what code can't answer.
2. **Bounded, not relentless.** A short clarify pass (3–5 questions), not the depth-first
   grill `work` runs. Batching questions is fine.
3. **The spec is the deliverable.** Quality matters more here than anywhere — keep the
   independent scored review.
4. **Reuse, don't reinvent.** Template, scaling rules, and review rubric come from
   `absolute-work`'s `references/spec-writing.md`. This command is the thin flow around them.
5. **Stop after review.** No code, no board, no execution. Hand off cleanly.
6. **Never auto-commit.** Write the spec file and report; the user commits.

---

## The Flow

```
SCAN ─→ CLARIFY ─→ WRITE ─→ REVIEW ─→ HANDOFF
```

No hard gates between steps (that's `work`). The one checkpoint is the clarify pass —
ask, get answers, then proceed straight through to a reviewed spec.

---

### Step 1 — SCAN

Ground the spec in reality before asking anything.

1. **Convention detection** — read cached config first: if `.absolute.config.json` or
   `~/.absolute/config.json` exists (from `/absolute init`), resolve the effective config
   (project file → global `projects["<cwd>"]` → global `defaults`) and use its `conventions`,
   detecting only what's missing. No config → soft-suggest `init` and run `work`'s Codebase
   Convention Detection table (package manager, language/runtime, test runner, linter, build
   system, directory conventions; see `absolute-work`'s SKILL.md under *Codebase Convention
   Detection*). The spec must speak the project's real stack, scripts, and paths.
2. **Deep context scan** — read what exists: `docs/` (README first), root `README.md`,
   `CLAUDE.md`, `CONTRIBUTING.md`, `docs/plans/` (overlapping/related designs), recent
   commits (last 10–20), package manifests, and the directories the feature touches.
3. **Synthesize** — state what you learned in 2–4 lines (stack, relevant existing code,
   any overlapping prior design). Do not dump a file listing.

Detect the work **type** (feature / refactor / greenfield / migration) only enough to
shape the spec — `spec` does not run the full per-type question banks.

---

### Step 2 — CLARIFY (bounded pass)

Ask **only** the questions the codebase genuinely cannot answer — the preference and
scope forks. Cap at **3–5 questions**; batching is allowed (one `AskUserQuestion` call
with multiple questions where available).

Good questions to ask (when code can't answer them):
- **Scope boundary** — what's in v1 vs explicitly deferred.
- **Behavior forks** — real-time vs batch, sync vs async, the genuine "which way" decisions.
- **Data/contract shape** — only where the design hinges on it and code gives no signal.
- **Non-goals** — what this explicitly will NOT do.

Do NOT ask: anything a config, manifest, test file, or existing pattern already states.
When the code answers it, say so in the spec instead of asking.

If the user said "no questions, just draft it" — skip to WRITE and capture every guess
in an `## Open Questions` / assumptions section instead.

---

### Step 3 — WRITE

Write the spec to `<specDir>/YYYY-MM-DD-<topic>-design.md` (`<topic>` = short kebab-case
slug). `<specDir>` is `preferences.specDir` from config when present, else `docs/plans`.

Use the template, **section-scaling rules**, writing style, and Decision Log format
from **`absolute-work`'s `references/spec-writing.md`** — that file is the single source of truth;
do not restate it here, load it. In particular:

- Pick the complexity tier (**Simple / Medium / Complex**) using the complexity
  heuristic table, and scale sections accordingly. Remove sections that would only say
  "N/A".
- Be **concrete**: real file paths, real endpoints, real schemas in code blocks — never
  "an endpoint for X".
- Fill the **Decision Log** with the forks resolved in CLARIFY plus any you recommended,
  each with a one-line rationale.

---

### Step 4 — REVIEW (scored, independent)

Dispatch a **separate** reviewer subagent — the agent that wrote the spec does not grade
it (generator ≠ evaluator). Use the **rubric and reviewer prompt template verbatim** from
`absolute-work`'s `references/spec-writing.md` (*Scored Spec Review Protocol*): graded on
Completeness, Consistency, Clarity, Scope, Testability (1–5 each, weighted).

| Weighted Score | Verdict | Action |
|---|---|---|
| 4.0 – 5.0 | Approved | Proceed to HANDOFF |
| 3.0 – 3.9 | Needs Work | Fix the flagged issues, re-dispatch (max 3 iterations) |
| < 3.0 | Major Gaps | Surface to the user immediately; do not silently iterate |

Reviewer approval is necessary but not the end — the user still has the final say at
HANDOFF.

---

### Step 5 — HANDOFF

1. **Present** — the spec path, the complexity tier, and the reviewer's weighted score +
   verdict. Summarize the key decisions in 2–4 lines.
2. **Offer to build** — chain into **`/absolute work`**, which will pick up the spec at
   its decompose phase rather than re-running intake. Make this an explicit, optional
   next step.
3. **Stop.** No code, no board. **Never run `git commit`** — suggest a message if asked;
   the user commits.

---

## Gotchas

1. **Drifting into `work`.** If you start decomposing into tasks or writing code, you've
   left `spec`. Stop at the reviewed doc.
2. **Asking what the code answers.** The bounded pass is for preferences and forks only —
   scan first, ask second.
3. **Skipping the review to "stay light".** The review is the cheap quality gate that
   makes a standalone spec trustworthy. Keep it.
4. **Restating `spec-writing.md`.** Load it; don't copy the template/rubric in here —
   two copies drift.
5. **Over-asking.** More than ~5 questions means you're running `work`'s interview. Tighten.

---

## Output / Response Style

Respond terse like smart caveman. All technical substance stay. Only fluff die. Drop
articles, filler, pleasantries, hedging. Fragments OK. Technical terms exact. Code blocks
and quoted errors unchanged. Drop caveman for security warnings, irreversible-action
confirmations, and any multi-step sequence where fragment order risks misread; resume
after. Spec prose itself is written normally (it's a deliverable, not chat).

---

## Companion commands

Sibling commands in this skill chain naturally around `spec`:

- **`/absolute work`** — build what the spec describes (picks the spec up at decompose).
- **`/absolute ui`** — design the interface for a feature the spec defines.
- **`/absolute docs`** — once it's built and shipped, document it for readers.

Suggest them where relevant; they're always available (same skill, no extra install).
