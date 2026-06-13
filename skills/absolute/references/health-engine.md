<!-- Shared engine for absolute's engineering-health commands: `upgrade`, `audit`, `prune`, `debt`, `deflake`. NOT a command itself — each of those command flows loads this file for the common loop + safety rules. Keep it command-agnostic; edits here affect all five. -->

# Engineering-Health Engine (shared)

The five engineering-health commands — `upgrade`, `audit`, `prune`, `debt`, `deflake` —
all run the **same loop** on a green `main`. Only *what they scan for* and *how they
fix it* differ. This file is that shared loop and the safety contract. Each command's
own reference (`references/<command>.md`) covers the part that's specific to it: what to
detect, which tools to run per ecosystem, and how to risk-rank findings.

```
DETECT ─→ SCAN ─→ TRIAGE (gate) ─→ FIX (safe waves) ─→ VERIFY ─→ REPORT
```

These commands are **standing-health** work on the whole repo, distinct from `simplify`
(which polishes *your working diff*) and the built-in `/security-review` / `/code-review`
(which review *pending changes*). They operate on committed, green code.

---

## Shared principles

1. **Start from green.** Run the project's tests/build first. If `main` is already red,
   stop and surface it — don't pile health changes onto a broken tree.
2. **Gate before fixing.** Always present risk-ranked findings and get an explicit "go"
   before changing anything. Findings ≠ a mandate to fix everything.
3. **Safe waves.** Apply fixes lowest-risk first, in batches that can be independently
   verified and reverted. Re-run checks between waves. When unsure, smaller waves.
4. **Bounded scope.** Fix only what the command targets. Adjacent problems → report as
   deferred, don't absorb them.
5. **No cheating the gate.** Never silence a check to make it pass — no `@ts-ignore`,
   `eslint-disable`, `# type: ignore`, `--no-verify`, skipped tests, or pinning a vuln
   scanner to "ignore". Fix it or defer it.
6. **Never auto-commit.** Apply to the working tree, report, suggest a commit/PR per wave;
   the user commits.

---

## Step 1 — DETECT

Auto-detect the stack so every command speaks the project's real tools. Use the
**Codebase Convention Detection** table in `references/work.md` (package manager,
language/runtime, test runner, linter/formatter, build, CI, available scripts).

Always drive fixes through the project's own scripts (`npm test`, `make lint`), never raw
tools, so results match CI. Note the ecosystem(s) — each command reference lists the
concrete commands per ecosystem (JS/TS, Python, Go).

---

## Step 2 — SCAN

Run the command-specific read-only scan (see each command's reference). Produce a raw
findings list. Do not change anything yet. Capture the exact tool output — it's the
evidence in the report and the baseline to diff against after fixing.

---

## Step 3 — TRIAGE (gate)

Turn raw findings into a **risk-ranked table** and present it. Generic shape:

| # | Finding | Risk | Effort | Recommend |
|---|---|---|---|---|
| 1 | … | low / med / high | S / M / L | fix now / defer / skip |

Rank by blast radius and reversibility, not by count. Group obviously-safe items into a
first wave. Call out anything risky (major version bumps, breaking removals, behavioral
test changes) explicitly.

**━━ GATE: user picks which findings/waves to apply before any change. ━━**

---

## Step 4 — FIX (safe waves)

Before touching files: ensure the tree is clean (commit or stash) and record the current
commit hash as the rollback point. Then apply the approved findings in waves, lowest-risk
first. Reuse the safe-wave mechanics in `references/work/execution-model.md` (disjoint,
independently verifiable batches; serialize when in doubt).

One concern per wave. Keep each wave small enough to revert cleanly if VERIFY fails.

---

## Step 5 — VERIFY

After each wave, run the project's real signals — test, lint, typecheck, build — through
its own scripts. Any failure: revert that wave (or fix-forward if trivial and obvious),
never paper over it. For risky waves, use the generator-evaluator check in
`references/work/verification-framework.md`. The bar is "green, the honest way."

---

## Step 6 — REPORT

Summarize: what was scanned (with tool versions/output), what changed (per wave, with
diffs/line counts), what was deliberately deferred and why, and the before/after signal
state. End with copy-pasteable verification steps and a suggested commit/PR message per
wave. **Never run `git commit` yourself.**

---

## Output / Response Style

Respond terse like smart caveman. Technical substance stays exact; code blocks and quoted
errors unchanged. Drop caveman for security warnings, irreversible-action confirmations
(deleting code, removing deps, major version bumps), and multi-step sequences where order
matters — resume after. Findings tables and reports stay normally formatted.
