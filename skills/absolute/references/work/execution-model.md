<!-- Part of the `absolute` skill (work command). Load this file in Phase 3 (decomposition) and Phase 4 (execution): DAG patterns, the safety-first wave algorithm, agent prompts, conflict handling, scope creep, and failure recovery. -->

# Execution Model

Absolute Work executes a dependency-graphed task board **one safe wave at a time** — the
onion-peel. The defining rule is **safety over speed**: blockers and dependents run
**sequentially**; only **provably-independent** tasks run in parallel. When in doubt, serialize.

---

## Identifying Dependencies

Task B depends on task A if:
- B needs code/files that A creates
- B imports or uses a function/type/interface A defines
- B tests, extends, modifies, or documents A's output
- B configures infrastructure A requires

B does NOT depend on A if they modify different files with no shared interfaces and can be tested
in isolation.

### Dependency checklist (per task pair)
1. Does B need any file A creates? → dependency
2. Does B import any symbol A defines? → dependency
3. Does B test code A writes? → dependency
4. Can B's tests pass without A complete? → if yes, no dependency

---

## Common DAG Patterns

```
Linear chain:   AW-001 → AW-002 → AW-003          (no parallelism; each task is a wave)
Fan-out:        AW-001 → {AW-002, AW-003, AW-004}  (W1: 001; W2: the rest)
Fan-in:         {AW-001, AW-002, AW-003} → AW-004  (W1: the three; W2: 004)
Diamond:        001 → {002, 003} → 004             (setup → parallel features → integration)
Independent clusters: A:001→002  B:003→004  C:005  (disconnected sub-graphs)
Layered:        infra → data → logic → ui → tests  (one wave per layer)
```

---

## The Safety-First Wave Algorithm

Two passes: depth grouping, then a safety partition.

### Pass 1 — Depth grouping (topological)
```
for each task:
  depth = 0 if no dependencies
          else max(dependency.depth) + 1
waves = group tasks by depth     // Wave 1 = depth 0, Wave 2 = depth 1, ...
```
Waves execute in strict serial order — Wave N+1 never starts until Wave N is fully verified and
the user confirms.

### Pass 2 — Safety partition (within each wave)
Classify every task in the wave as `seq` (sequential) or `parallel`:

A task is **`parallel`-safe only if ALL hold:**
- It touches a **disjoint set of files** from every other task in the wave
- It shares **no interfaces/types** being defined by another task in the wave
- It is not a **blocker** that a later task in the same wave reads from
- Its outcome does not change another task's plan

Otherwise it is **`seq`** — run it alone, in dependency order. **Default to `seq` when uncertain.**
Record the decision in the board's **Run** column with a one-line reason.

### Shared-file ownership
If two same-wave tasks would touch the same file, assign that file to **one** owner task; the
others treat it as read-only until the owner completes, or get moved to a later wave. This is the
single most common source of wave-boundary conflicts — resolve it at decomposition time.

### Execution within a wave
```
run all `seq` tasks first, one at a time, in dependency order
then run `parallel`-safe tasks concurrently (separate agents)
wait for the whole wave to finish → wave boundary checks → GATE → next wave
```
Running `seq` tasks first means the riskiest/shared work lands before independent work fans out,
so parallel agents build on a settled base.

### Worktree isolation (optional)
On platforms that support it, run `parallel` tasks in isolated worktrees when there is any residual
risk of file overlap; merge back at the wave boundary. Skip it when tasks touch clearly different
directories — the merge overhead isn't worth it.

---

## ASCII Graph Rendering

```
Task Graph:
  [W1] AW-001 [config: Init structure]           (seq — shared config)
         ├──> [W2] AW-002 [code: DB schema]        (seq)
         └──> [W2] AW-003 [code: API router]       (parallel-safe)

Wave Summary:
  Wave 1 (1): AW-001  [seq]
  Wave 2 (2): AW-002 [seq], AW-003 [parallel]
```

---

## Pre-Execution Snapshot

Before any file is touched in Wave 1:
1. Ensure the working tree is clean (commit or stash existing changes).
2. Record the current commit hash on the board under `## Rollback Point`.
3. If execution goes catastrophically wrong, the user can `git reset --hard` to this commit.

Record the hash **before** Wave 1 begins — a mid-wave snapshot already contains partial changes.

---

## Agent Prompt Template

Each execution agent receives a self-contained prompt from the board:

```
## Task: {AW-XXX} - {Title}

### Context
{description from the board}

### Project Conventions
{detected conventions — package manager, test runner, linter, directory patterns}

### Research Notes
{key files, reusable code, patterns, risks for this task}

### Execution Plan
- Files to create/modify: {list}
- Test files: {list}
- Approach: {from PLAN}

### Acceptance Criteria
{specific, verifiable conditions}

### Rules
1. Write tests FIRST, watch them fail (red), then implement (green), then refactor.
2. Run lint and type-check on modified files via the project's scripts.
3. Do NOT modify files outside your task scope.
4. Reuse existing utilities named in the research notes — do not reinvent them.
5. If blocked, STOP and report the blocker — never work around it.
6. Report: files changed, tests written, tests passing, any issues.
```

### Template by task type
- **code** → full TDD template above.
- **test** → skip "write tests first" (the task *is* the tests); write happy/edge/error cases and run against the implementation.
- **docs** → no TDD; follow existing doc style; verify code examples are syntactically valid.
- **config/infra** → verify by running the relevant tool/build; check idempotency.

---

## Wave Boundary Checks

After every task in a wave completes, before the gate:

1. **Conflict check** — did any two agents modify the same file? Merge intelligently (prefer the change that better satisfies its acceptance criteria); if unmergeable, present both versions to the user.
2. **Interface compatibility** — types defined by one task match those expected by another.
3. **Import resolution** — all cross-task imports resolve.
4. **Combined build + tests** — run the build and the wave's tests together.
5. **Progress report** — print a compact status table:
```
Wave 2 complete (2/4 waves)
| Task   | Run      | Status | Notes |
|--------|----------|--------|-------|
| AW-003 | seq      | done   |       |
| AW-004 | parallel | done   |       |
```
Then **GATE** — confirm before starting the next wave.

---

## Scope Creep Guard

- **Blocking discovery** (can't finish the current task without it): add a new visible task to the DAG, place it in the current or next wave, flag it on the board, continue with other tasks.
- **Non-blocking discovery** (nice-to-have, adjacent cleanup): do NOT absorb it. Add it to `## Deferred Work`, mention it at CONVERGE. The user decides whether to start a new session.
- **Never silently expand scope** — every DAG addition is visible on the board and called out in the next progress report.

---

## Blocked Tasks

```
When a task is blocked:
  1. Mark status `blocked` with a reason and the blocking task ID.
  2. Continue executing non-blocked tasks in the wave.
  3. After the wave, reassess:
     - blocker resolved → add to the next wave
     - blocker persists → flag for user attention
     - approachable differently → revise plan and retry
```
If a Wave N task fails and Wave N+1 tasks depend on it, mark the dependents `blocked` (not
`failed`); run the non-dependent Wave N+1 tasks normally; unblock dependents if the failure is
later fixed.

---

## Failure Recovery

| Failure | Action | Max Retries |
|---|---|---|
| Test failure (code bug) | Fix code, re-run tests | 2 |
| Lint/type error | Fix, re-run check | 2 |
| Build failure | Find root cause, fix | 1 |
| Agent crash/timeout | Restart with same prompt | 1 |
| Merge conflict | Resolve, re-verify | 1 |
| Fundamental approach failure | Revise plan, flag for user | 0 (needs user input) |

On retry, append the error to the agent prompt: "Previous attempt failed because: {error}. Fix
and retry." When retries are exhausted, mark the task `failed`, record all attempt logs, flag the
user with the rollback hash, and continue with non-dependent tasks. Never bypass tests or checks
to force a pass.

---

## Performance Guidelines

- **Optimal wave size**: 1-3 parallel tasks (low overhead); 4-6 (good throughput); 7+ → split into sub-waves to limit failure blast radius.
- Each parallel agent consumes context and compute; in constrained environments, cap concurrency at 3-4.
- **Skip parallelism** when a wave has one task, when all tasks touch the same file, or when any dependency isn't fully captured in the DAG (a sign the decomposition needs another pass).
