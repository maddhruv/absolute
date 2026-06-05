<!-- Part of the Absolute Work AbsolutelySkilled skill. Load this file in Phase 3 for the full .absolute-work/board.md specification — format, status transitions, and example. -->

# Board Format Specification

The `.absolute-work/board.md` file is the single source of truth for an Absolute Work run. It is
the only state — fully local, no external trackers — and is designed to be both human-readable and
machine-parseable. It survives across sessions to enable resume, audit, and handoff.

---

## File Location

```
{project-root}/.absolute-work/board.md
```

The `.absolute-work/` directory may also contain:
- `board.md` — the main board (always present)
- `archive/board-{timestamp}.md` — completed or superseded boards

The user chooses during intake whether `.absolute-work/` is git-tracked (audit trail, resume
across machines) or gitignored (local working state).

---

## Board Metadata (YAML frontmatter)

```yaml
---
id: aw-{timestamp}
title: "{brief description of the overall task}"
type: feature | bug | refactor | greenfield | planning | migration
status: intake | spec | decomposing | planning | executing | verifying | converged | completed | abandoned
created: "{ISO 8601}"
updated: "{ISO 8601}"
git_tracked: true | false
evaluator_enabled: true | false
total_tasks: {N}
completed_tasks: {N}
failed_tasks: {N}
current_wave: {N}
total_waves: {N}
---
```

`evaluator_enabled` defaults to `true` for boards with any M-size tasks; set `false` only when all
tasks are S-size.

---

## Board Sections

### 1. Intake Summary
```markdown
## Intake Summary
- **Task**: {one-line description}
- **Type**: feature | bug | refactor | greenfield | planning | migration
- **Complexity**: simple | medium | complex
- **Problem**: {what needs to be built/fixed}
- **Success Criteria**: {what "done" looks like}
- **Constraints**: {patterns, libraries, conventions to follow}
- **Dependencies**: {external APIs, services, other work}
- **Edge Cases**: {known edge cases} (if complex)
- **Spec**: docs/plans/{date}-{topic}-design.md
- **Board Persistence**: git-tracked | gitignored
```

### 2. Project Conventions
Written during Codebase Convention Detection — package manager, language/runtime, test runner,
linter/formatter, build system, available scripts, directory conventions. Referenced by every
later phase and by every execution agent's prompt.

### 3. Task Graph
```markdown
## Task Graph

### Sub-tasks
| ID | Title | Type | Size | Dependencies | Wave | Run | Status |
|----|-------|------|------|-------------|------|-----|--------|
| AW-001 | {title} | config | S | - | 1 | seq | done |
| AW-002 | {title} | code | M | AW-001 | 2 | seq | in-progress |
| AW-003 | {title} | code | S | AW-001 | 2 | parallel | pending |

### Dependency Graph
{ASCII graph — see execution-model.md}

### Wave Assignments
- **Wave 1** (1 task): AW-001 [sequential — blocker, shared file]
- **Wave 2** (2 tasks): AW-002 [sequential], AW-003 [parallel-safe]
```

The **Run** column records the safety decision: `seq` (blocker/dependent/shared-file → run in
order) or `parallel` (disjoint files, no shared interfaces → may run concurrently).

### 4. Tasks (per-task detail)
```markdown
## Tasks

### AW-001: {title}
- **Type**: code | test | docs | infra | config
- **Size**: S | M
- **Dependencies**: none | [AW-XXX]
- **Wave**: {N}  **Run**: seq | parallel
- **Status**: {current status}

#### Research Notes
- Key files: {list}
- Reusable code: {functions/utilities to reuse}
- Patterns: {conventions observed}
- Risks: {risks identified}

#### Execution Plan
- Files to create/modify: {list}
- Test files: {list}
- Approach: {brief}
- Acceptance criteria:
  - [ ] {criterion 1}
- Test cases: {happy path, edge, error}

#### Verification
- Signals: PASS | FAIL
- Tests: {passed}/{total} ({new} new)
- Lint: clean | {issues}   Type Check: pass | {errors}   Build: pass | fail
- Evaluator Score: {N.N}/5.0 | skipped (S-size)
- Verdict: PASS | NEEDS WORK | FAIL    Iteration: {N}/5
```

### 5. Rollback Point
```markdown
## Rollback Point
- Pre-execution commit: {hash}
- Recorded: {timestamp}
```

### 6. Execution Log
```markdown
## Execution Log
### Wave 1 — {timestamp}
- Tasks: AW-001 (seq)
- Completed: {timestamp} — Result: all passed
```

### 7. Deferred Work
```markdown
## Deferred Work
- {non-blocking discovery}, found during {task}, not in original scope
```

### 8. Convergence Summary
```markdown
## Convergence Summary
### Files Changed
| File | Action | Lines |
|------|--------|-------|
| src/api/auth.ts | created | +120 |

### Tests Added
- Total new tests: {N}    Coverage: {% if available}

### Key Decisions
- {decision and why}

### Suggested Commit Message
{emoji} {type}: {subject}

{body}
```

---

## Status Transitions

### Board-level
```
intake → spec → decomposing → planning → executing → verifying → converged → completed
                                                                      └→ abandoned
```

### Task-level
```
pending → researching → planned → in-progress → verifying → done
                                      │             └→ failed (retry)
                                      └→ blocked
```

| From | To | Trigger |
|------|-----|---------|
| pending | planned | Research + plan written in DECOMPOSE & PLAN |
| planned | in-progress | EXECUTE starts for this task |
| in-progress | verifying | Implementation complete, running checks |
| in-progress | blocked | Dependency failed or external blocker |
| verifying | done | All signals pass and evaluator (if run) passes |
| verifying | failed | Verification failed after max retries |
| blocked | in-progress | Blocker resolved |

---

## Resuming a Board

At the start of any invocation, if `.absolute-work/board.md` exists:
1. Read the board; parse frontmatter and current state.
2. Display a compact status summary.
3. Identify the current phase and incomplete tasks (anything not `done`/`failed`).
4. Resume from the current point — `executing` → next unfinished wave; `verifying` → re-run
   verification on unverified tasks; `planning` → finish remaining plans.
5. Add a "Resumed at {timestamp}" entry to the Execution Log.
6. Reconcile: if the codebase changed since `updated`, flag conflicts before continuing.

If the board is `completed`, ask whether to start fresh (archive to `archive/board-{timestamp}.md`)
or review. Never overwrite a board without explicit confirmation.

---

## Example Board (abbreviated)

```yaml
---
id: aw-1717400000
title: "Add user authentication to Next.js app"
type: feature
status: executing
git_tracked: false
total_tasks: 8
completed_tasks: 3
current_wave: 2
total_waves: 4
---

## Intake Summary
- **Task**: Add email/password + Google OAuth authentication
- **Type**: feature  **Complexity**: complex
- **Constraints**: NextAuth.js v5, existing Prisma + PostgreSQL
- **Spec**: docs/plans/2026-06-03-auth-design.md
- **Board Persistence**: gitignored

## Task Graph
### Sub-tasks
| ID | Title | Type | Size | Dependencies | Wave | Run | Status |
|----|-------|------|------|-------------|------|-----|--------|
| AW-001 | NextAuth config + providers | config | S | - | 1 | seq | done |
| AW-002 | User + Account Prisma models | config | S | - | 1 | parallel | done |
| AW-003 | Auth API route handler | code | M | AW-001, AW-002 | 2 | seq | in-progress |
| AW-004 | Auth middleware | code | M | AW-001 | 2 | parallel | done |

### Wave Assignments
- **Wave 1** (2): AW-001 [seq — shared config], AW-002 [parallel-safe]
- **Wave 2** (2): AW-003 [seq], AW-004 [parallel-safe]

## Rollback Point
- Pre-execution commit: a1b2c3d
```
