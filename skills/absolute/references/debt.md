# Command: `debt` — Lint & Typecheck Debt Paydown

> Loaded by the `absolute` router when the user runs `/absolute debt …`.
> Start your first response with the 🧾 emoji.

## Absolute Debt

Pay down accumulated lint and type-checker debt across the repo — existing warnings,
errors, and suppressions that built up over time — in safe waves, by rule, with tests green
after each. The goal is a genuinely clean `lint` + `typecheck`, not a quieter one.

Runs the shared engine in **`references/health-engine.md`** — read it for the
DETECT → SCAN → TRIAGE → FIX → VERIFY → REPORT loop and the safety contract. This file
covers only what's specific to lint/type debt.

---

## When to use

- "Fix our lint warnings", "clean up the type errors", "get the codebase to strict mode".
- Tightening config (enable a rule, raise `tsconfig` strictness) and clearing the fallout.
- Burning down accumulated `eslint-disable` / `@ts-ignore` / `# type: ignore` suppressions.

**`debt` vs `simplify`:** `simplify` improves *your working diff's* quality. `debt` clears
**pre-existing repo-wide** lint/type violations on green `main`. Use `debt` for standing
cleanup, not for changes you're actively making.

---

## What it scans

| Ecosystem | Lint debt | Type debt |
|---|---|---|
| JS/TS | `eslint .` (full report, by rule) | `tsc --noEmit` |
| Python | `ruff check` / `flake8` | `mypy` / `pyright` |
| Go | `golangci-lint run` | `go vet ./...`, `staticcheck` |

Also inventory **suppressions** as debt in their own right: `eslint-disable*`,
`@ts-ignore`/`@ts-expect-error`, `# type: ignore`, `# noqa`, `//nolint`. Each is a hidden
violation. Count violations grouped **by rule** (not by file) — that's how you wave them.

---

## Risk ranking (TRIAGE)

| Wave | Class | Default |
|---|---|---|
| 1 | autofixable lint (`eslint --fix`, `ruff --fix`, `gofmt`) | fix now — mechanical, safe |
| 2 | manual lint by rule, low behavioral risk (unused, style, imports) | fix this pass, one rule at a time |
| 3 | type errors, and lint rules that can change behavior if "fixed" wrong | gated — review each, may need real logic |
| 4 | removing suppressions (`@ts-ignore` etc.) | gated — each may surface a real bug |

Fix **one rule across the repo per wave**, not one file across all rules — keeps diffs
reviewable and regressions bisectable. Config tightening (enable rule / raise strictness) is
itself a finding: propose it, then clear what it surfaces.

---

## Fix & verify

- Run the autofixers first (wave 1), commit-ready, then re-scan to see what's truly manual.
- Manual fixes: address the *cause*, not the symptom. A type error means the types are
  wrong somewhere — fix the type, don't cast to `any`/add `# type: ignore`.
- **Never** clear a violation by suppressing it. Removing suppressions is the *goal*, not a tool.
- After each wave: full test + lint + typecheck + build. The violation count must drop and
  nothing regress.

---

## Gotchas

1. **Fixing by suppressing.** `@ts-ignore` to clear a type error *adds* debt. Forbidden here.
2. **`any` / `cast` to win.** Silences the checker, keeps the bug. Fix the real type.
3. **One-file-all-rules waves.** Mixes concerns, unreviewable. Go one-rule-all-files.
4. **Autofix without review.** `--fix` can change behavior (e.g. `prefer-const` on a
   reassigned var via a bug). Run tests after autofix too.
5. **Disabling the rule instead of fixing.** Loosening config to hit zero is debt laundering,
   not paydown.

---

## Companion commands

- **`/absolute simplify`** — quality of code you're *currently changing*.
- **`/absolute prune`** — many unused-var/import warnings vanish once dead code is pruned.
- **`/absolute work`** — if clearing the debt needs real refactoring, hand off.
