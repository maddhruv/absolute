---
name: absolute-prune
version: 0.5.0
description: >
  Dead code and dependency cleanup, repo-wide: unused deps, unreferenced exports, unreachable code, orphaned files — removed only with tool evidence, in reversible waves. Runs on green main. For diff-scoped cleanup use absolute-simplify.
  Triggers on "absolute prune", "remove dead code", "find unused deps/exports", "what can we delete", "clean up orphaned files".
category: workflow
tags:
  - workflow
  - maintenance
  - dead-code
  - cleanup
  - pruning
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

> Start your first response with the ✂️ emoji.

## Absolute Prune

Cut dead growth from the repo: unused dependencies, unreferenced exports, unreachable code,
orphaned files. Evidence-based — every removal is backed by a tool that proves nothing
references it — applied in safe waves with tests green after each.

Runs the shared engine in **`references/health-engine.md`** — read it for the
DETECT → SCAN → TRIAGE → FIX → VERIFY → REPORT loop and the safety contract. This file
covers only what's specific to pruning.

---

## When to use

- "Remove dead code", "clean up unused deps", "what can we delete?".
- Bundle/install bloat from packages nothing imports anymore.
- Post-refactor orphans: files and exports left behind after a feature was rerouted.

**`prune` vs `simplify`:** `simplify` polishes *your working git diff*. `prune` sweeps the
**whole committed repo** for things that are dead repo-wide. Use `simplify` mid-change;
`prune` as standing cleanup on green `main`.

---

## What it scans

**Dead dependencies** — declared but never imported (and missing deps that are imported but
undeclared):

| Ecosystem | Tool |
|---|---|
| JS/TS | `depcheck` or `knip` (knip also covers exports/files) |
| Python | `deptry` |
| Go | `go mod tidy` (diff), unused module detection |

**Dead code** — unreferenced exports, unreachable branches, orphaned files:

| Ecosystem | Tool |
|---|---|
| JS/TS | `knip` (exports/files), `ts-prune`, `eslint no-unused-vars` |
| Python | `vulture`, `ruff` unused rules |
| Go | `deadcode ./...`, `staticcheck` (U1000) |

Prefer tools already in the project. Treat results as *candidates* — verify each isn't
reached via dynamic import, reflection, DI, public API, or a config string before removing.

---

## Risk ranking (TRIAGE)

| Wave | Removal | Default |
|---|---|---|
| 1 | unused devDeps, unreferenced *local* exports/functions | fix now — lowest risk |
| 2 | unused runtime deps, orphaned internal files | fix this pass after confirming no dynamic ref |
| 3 | anything reachable via public API, plugin system, dynamic require, reflection, or config | **gated / usually defer** — high false-positive risk |

Static tools miss dynamic references. Anything in wave 3, or anything a tool flags but you
can't *prove* is dead, gets confirmed with the user or deferred — never auto-removed.

---

## Fix & verify

- Remove in small, reversible waves. One category per wave (e.g. "unused devDeps", then
  "orphaned files").
- After each wave: full test + build + typecheck. A green typecheck/build is the proof the
  removed symbol truly had no references. If anything goes red, the symbol wasn't dead —
  revert and reclassify.
- Removing a dep: drop it from the manifest, regenerate the lockfile, rebuild.
- Don't delete: generated files, vendored code, public-API surface, or anything load-bearing
  for a config/plugin you can't trace. Report those as "suspected dead, needs human call".

---

## Gotchas

1. **Trusting the tool blindly.** Dynamic imports, DI containers, reflection, and string-keyed
   lookups defeat static analysis. Confirm before deleting.
2. **Removing public API.** A library's unused-internally export may be its whole point. Check
   the package entry points / `exports` map.
3. **Deleting generated or vendored files.** They look orphaned but are rebuilt/checked-in on purpose.
4. **Big-bang prune.** Deleting everything flagged in one commit makes regressions un-bisectable.
   Wave it.
5. **Scope creep into refactor.** `prune` removes dead things; it does not restructure live code.
   Restructuring → `simplify` or `work`.

---

## Companion commands

- **`/absolute simplify`** — for restructuring/clarity of *live* code in your diff.
- **`/absolute upgrade`** — pairs well: prune unused deps, then upgrade what remains.
- **`/absolute debt`** — pruning often clears a batch of lint/unused-var warnings too.
