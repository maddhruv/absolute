<!-- Part of the `absolute` skill (work command). Load this file when the work type is a migration — alongside intake-playbook.md. Covers call-site inventory, codemods, incremental rollout, backwards-compat, and rollback. -->

# Migration Playbook

Migrations are the highest-risk work type Absolute Work handles: the code already runs in
production, real users depend on it, and a botched migration breaks things that worked yesterday.
This playbook makes migrations a first-class flow — safe, incremental, and reversible.

The core principle: **never big-bang a migration you can do incrementally.** Keep old and new
coexisting behind a seam, move call sites in small verified batches, and keep a rollback at every
step.

---

## Migration Types

| Type | Example | Primary Risk |
|---|---|---|
| **Language/syntax** | JS → TS, CommonJS → ESM | Type errors, build config, partial coverage |
| **Library swap** | moment → date-fns, Enzyme → Testing Library | API mismatch, behavioral differences |
| **Framework version** | React 17 → 18, Next 13 → 14 | Breaking changes, deprecated APIs |
| **API/contract** | REST v2 → v3, schema change | Consumer breakage, data shape drift |
| **Data/schema** | column rename, table split, DB engine | Data loss, downtime, dual-write complexity |
| **Infrastructure** | provider A → B, monolith → services | Config drift, cutover coordination |

Identify the type during intake — it determines which sections below apply.

---

## Phase A: Call-Site Inventory (before any code)

You cannot safely migrate what you have not counted. Build a complete inventory of everything
that touches the thing being migrated.

### Steps
1. **Find the surface.** Grep for the symbol, import, endpoint, or pattern being migrated. Capture every hit with `file:line`.
2. **Classify each call site** by shape — most migrations have 3-5 distinct usage patterns plus a long tail of one-offs.
3. **Count and bucket.** Record totals per pattern on the board. This sizes the migration and reveals whether a codemod is worth writing.
4. **Find the blind spots.** Dynamic usage (string-built imports, reflection, config-driven dispatch) won't show up in a grep. List where these could hide.
5. **Map consumers.** For API/contract/library migrations, identify external consumers (other services, published packages, clients) that a grep of *this* repo will miss.

### Inventory table (write to the board)
```
## Migration Inventory
| Pattern | Example call site | Count | Codemod-able? | Notes |
|---------|-------------------|-------|---------------|-------|
| direct import | src/a.ts:12 | 47 | yes | mechanical rename |
| wrapped helper | src/lib/fmt.ts:8 | 1 | n/a | central shim point |
| dynamic dispatch | src/router.ts:30 | 3 | no | manual review |
Total call sites: 51 across 23 files
```

---

## Phase B: Choose the Strategy

| Strategy | Use When | Trade-off |
|---|---|---|
| **Incremental (strangler)** | Large surface, production code, old+new can coexist | Safest; slower; needs a coexistence seam |
| **Codemod-driven** | Many mechanical, uniform call sites | Fast for the uniform 80%; the tail is still manual |
| **Parallel-run / shadow** | Behavior must be proven identical (data, money) | Highest confidence; most setup |
| **Big-bang** | Tiny surface (< ~10 sites) or hard cutover required | Fast; only safe when blast radius is small and fully tested |

Default to **incremental** unless the surface is genuinely tiny. State the chosen strategy and
its rationale on the board and in the spec.

---

## Phase C: Establish the Coexistence Seam

For incremental migrations, old and new must run side by side. Create a seam so call sites can be
moved one batch at a time without a flag day.

- **Adapter/shim** — a thin wrapper exposing the new implementation behind the old signature (or vice versa), so call sites switch without changing shape.
- **Feature flag** — gate new behavior so it can be toggled per environment or per user, and reverted instantly.
- **Dual-write (data migrations)** — write to both old and new stores during transition; read from old until new is verified, then flip reads, then stop writing old.
- **Version negotiation (APIs)** — serve both v-old and v-new; deprecate v-old only after consumers move.

The seam is itself a task on the board, and usually a **blocker** that must complete sequentially
before any call-site batch runs.

---

## Phase D: Incremental Rollout

Move the surface in small, independently verifiable batches — this is the onion-peel applied to
migrations.

```
for each batch of call sites (grouped by module or pattern):
  1. migrate the batch (codemod for mechanical, manual for the tail)
  2. run tests for the affected modules → must stay green
  3. typecheck / build → must pass
  4. commit-worthy checkpoint (suggest commit; user commits)
  5. update the inventory: migrated N / total
```

Batch sizing: keep each batch small enough that if it breaks, the cause is obvious. Group by
module boundary or by usage pattern. Migrate the central shim/helper first (one change unblocks
many), then the mechanical bulk, then the manual tail last.

### Codemod guidance
- Write the codemod against the patterns found in the inventory; dry-run it and diff before applying.
- Codemods handle the uniform majority; they will not handle dynamic dispatch, comments, or unusual formatting. Always hand-review the tail.
- Re-run the inventory grep after the codemod to confirm the count dropped to the expected remainder.

---

## Phase E: Backwards Compatibility

| Concern | Handling |
|---|---|
| **External consumers** | Keep the old surface working (deprecated) until consumers migrate; announce a removal timeline |
| **Persisted data** | New code must read old-format data; migrate data lazily on read or via a background job |
| **Serialized contracts** | Version payloads; tolerate missing/extra fields during transition |
| **Public API** | Additive changes only during transition; breaking removals happen in a later, separate release |

Backwards-compat shims are temporary debt: add a task to the Deferred Work section to remove them
once the migration completes and consumers have moved.

---

## Phase F: Rollback Plan

Every migration records how to undo it, at every checkpoint — not just at the end.

1. **Snapshot** — the pre-migration commit hash on the board (`## Rollback Point`).
2. **Per-batch reversibility** — each batch is a clean checkpoint the user can revert to.
3. **Flag kill-switch** — if behind a feature flag, the rollback is flipping the flag, no redeploy.
4. **Data rollback** — for dual-write, document how to stop writing new and resume reading old; for destructive schema changes, ensure a backup exists *before* the change.
5. **Cutover criteria** — define what must be true before the old path is removed (all call sites moved, consumers migrated, monitoring clean for N days).

Never remove the old path in the same step that introduces the new one. Removal is its own task,
gated on the cutover criteria being met.

---

## Migration Anti-Patterns

| Anti-Pattern | Better Approach |
|---|---|
| Migrating before inventorying call sites | Grep and count everything first — you can't migrate what you haven't found |
| Big-bang on a large surface | Incremental batches behind a coexistence seam |
| Removing the old path alongside adding the new | Coexist first; remove only after cutover criteria are met |
| Codemod with no dry-run/diff review | Dry-run, diff, then apply; always hand-review the tail |
| Ignoring dynamic/reflective usage | List blind spots explicitly; grep won't catch string-built dispatch |
| No rollback until the very end | Every batch is a reversible checkpoint; snapshot before Wave 1 |
| Forgetting external consumers | Keep old contract alive and deprecated until consumers move |
| Leaving compat shims forever | Track shim removal in Deferred Work, gated on cutover |
