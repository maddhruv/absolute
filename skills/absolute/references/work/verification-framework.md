<!-- Part of the `absolute` skill (work command). Load this file in Phase 4/5: TDD per task, verification signals, the generator-evaluator protocol, scored rubric, and the mandatory tail tasks. -->

# Verification Framework

Every task proves it works before closing. Verification runs in two layers — **signals**
(objective, binary) and **evaluator** (subjective, scored) — with generator-evaluator separation
throughout. This reference also defines the three mandatory tail tasks that close every board.

---

## TDD Workflow Per Task

### Red → Green → Refactor
```
RED:      write tests describing the desired behavior → tests FAIL
GREEN:    write the minimum code to pass → tests PASS
REFACTOR: clean up while keeping tests green → tests PASS
```

### Steps
1. Read the acceptance criteria from the task's plan.
2. Write test file(s) encoding each criterion as a test case.
3. Run tests — confirm they FAIL (red proves the tests are meaningful).
4. Implement to make each test pass, one at a time.
5. Run tests — confirm they PASS (green).
6. Refactor — rename, extract, simplify — keeping tests green.
7. Final run — all tests pass, lint clean, types check.

### Test categories per task
| Category | What | Priority |
|---|---|---|
| Happy path | Primary use case works | Required |
| Edge cases | Boundaries, empty, nulls | Required |
| Error handling | Invalid inputs, failure modes | Required |
| Integration | Interaction with other components | If applicable |

Follow the project's existing test-naming convention; if none, use
`describe("Thing", () => it("should X when Y"))`.

---

## Layer 1: Verification Signals (Binary Gate)

Run via the project's own scripts — never raw tools.

| Signal | Example command | Required | Notes |
|---|---|---|---|
| Tests | `npm test` / `pytest` / project cmd | Always | All new + existing tests pass |
| Lint | `npm run lint` / project cmd | Always | Zero new warnings/errors |
| Type Check | `tsc --noEmit` / `mypy` | If typed | No new type errors |
| Build | `npm run build` / project cmd | If applicable | Project still builds |
| Format | `prettier --check` / `black --check` | If configured | Matches project format |

Detect available commands from `package.json` scripts, `Makefile`, `pyproject.toml`, and CI config.
If ANY signal fails, the task goes straight back to the generator to fix (up to 2 signal retries).
Signal failures are unambiguous — no evaluator needed to diagnose a failing test or broken build.

If time-constrained, verify in priority order: Tests → Build → Type Check → Lint → Format.

---

## Layer 2: The Evaluator (Scored Gate)

If all signals pass, dispatch a **separate** evaluator subagent. Self-evaluation has systematic
bias — generators over-praise their own work and talk themselves out of legitimate issues. A
fresh, skeptical context sees the work as a reviewer would.

### Complexity gating
| Size | Evaluator | Rationale |
|---|---|---|
| **S** (< 50 lines) | Signals only | Binary signals catch most issues in small changes |
| **M** (50-200 lines) | Full evaluator | Subjective quality matters at this scale |
| **Any failed task** | Mandatory full evaluator | Failure means the task is at the capability edge |
| **Touches shared interfaces** | Mandatory full evaluator | Integration risk demands independent review |

### Scored rubric (code tasks)
| Dimension | Weight | 1 (Fail) | 3 (Acceptable) | 5 (Excellent) |
|---|---|---|---|---|
| **Correctness** | 30% | Tests fail / criteria unmet | Tests pass, basic criteria met | All criteria incl. edge cases, no regressions |
| **Code Quality** | 20% | Ignores conventions, unclear | Follows conventions, readable | Clean, idiomatic, extends existing patterns |
| **Completeness** | 20% | Partial, TODOs left | All stated criteria addressed | Handles implicit requirements too |
| **Test Coverage** | 15% | No/trivial tests | Happy path tested | Edge, error, and boundary cases tested |
| **Integration Safety** | 15% | Broken imports/types | Builds, existing tests pass | No warnings, clean integration |

**Rubric adaptations:** `test` tasks → swap Code Quality for Assertion Quality, raise Test Coverage
to 25%. `docs` tasks → Clarity/Accuracy replaces Code Quality; Coverage/Completeness replaces Test
Coverage. `config`/`infra` → Integration Safety to 25%, add an Idempotency check.

### Thresholds
| Weighted Score | Verdict | Action |
|---|---|---|
| 4.0 - 5.0 | PASS | Proceed |
| 3.0 - 3.9 | NEEDS WORK | Generator gets specific feedback, retries |
| < 3.0 | FAIL | Escalate to the user |

### Evaluator prompt template
```
## Evaluator Task: {AW-XXX} - {Title}

You are an independent evaluator. Grade this work skeptically and honestly.
Do not assume good intent. Look for gaps, shortcuts, incomplete work, hidden bugs.

### Acceptance Criteria
{criteria — what "done" means}

### Files Modified / Git Diff
{the actual diff of all changes}

### Test Output / Lint / Type / Build Output
{verification signal results}

### Project Conventions
{detected conventions}

Score each dimension 1-5 using the rubric. Output (STRICT):

#### Evaluation: AW-{XXX}
- **Correctness**: {score}/5 - {justification}
- **Code Quality**: {score}/5 - {justification}
- **Completeness**: {score}/5 - {justification}
- **Test Coverage**: {score}/5 - {justification}
- **Integration Safety**: {score}/5 - {justification}
- **Weighted Score**: {calculated}/5.0
- **Verdict**: PASS | NEEDS WORK | FAIL

#### Specific Feedback (required if score < 4.0)
- What is wrong, what the fix looks like, which files need changes.

#### Bugs Found
- {file:line references}

#### What Was Done Well
- {1-2 strengths}
```

The evaluator receives only outputs and criteria — never the generator's reasoning. This prevents
it from rationalizing decisions it should be questioning.

### Iterative refinement loop
```
for iteration in 1..5:
  evaluator grades the work
  if weighted score >= 4.0: PASS, exit
  if iteration >= 3 AND score stagnant/declining: FAIL, escalate with full history
  if score < 3.0 on any iteration: FAIL immediately, escalate
  else: generator makes TARGETED fixes from the feedback (not a rewrite); loop
```
Track scores per iteration on the board. A dropping score means the generator is thrashing — stop
and escalate. On retry the generator receives: per-dimension scores, the specific-feedback section,
the bugs list, and the instruction "Fix ONLY what the evaluator flagged."

### Platform fallback
Without subagent support, switch context explicitly: "— EVALUATOR MODE — I evaluate the work I
just completed as a different, skeptical reviewer. I do not reference my implementation intent; I
judge only what is visible in the code and test output." Weaker than true separation, but strictly
better than no gate.

---

## Integration Verification (post-wave)

After a wave, if its tasks share dependencies or feed the next wave: verify import resolution,
run the combined test suite (or tests for all files modified in the wave), run the build (watch
for circular-dependency warnings), and smoke-test at runtime if applicable. After the final wave,
run the FULL suite and build to catch regressions before CONVERGE.

---

## What NOT to Do on Failure

- Do NOT suppress or skip failing tests.
- Do NOT add `@ts-ignore`, `// eslint-disable`, or `# type: ignore` to pass checks.
- Do NOT reduce coverage or modify existing passing tests to accommodate a bug.
- Do NOT mark a task `done` while any signal fails.

---

## Mandatory Tail Tasks

Every task graph ends with these three, in order. They are real tasks on the board with
acceptance criteria — not afterthoughts.

### Third-to-last — Self Code Review (`review`)
Review all changes since the rollback point. Work the review pyramid bottom-up: Security →
Correctness → Performance → Design → Readability → Convention → Testing. Classify findings
`[MAJOR]` / `[MINOR]`. Fix all `[MAJOR]` immediately and reasonable `[MINOR]`. Re-run after fixes.
**Acceptance:** zero `[MAJOR]` remaining; all `[MINOR]` documented (fixed or explicitly deferred).
Depends on all implementation/test/docs tasks. Run the `/absolute simplify` command on the
working changes here.

### Second-to-last — Requirements Validation (`verify`)
Compare all changes against the original prompt, intake summary, and spec. Verify every success
criterion and constraint is satisfied. **Acceptance:** every success criterion demonstrably met;
gaps loop back to EXECUTE until resolved. Depends on the self code review.

### Last — Full Project Verification (`verify`)
Run all available checks via the project's package-manager scripts, skipping any not configured:
Tests → Lint → Typecheck → Build. **Acceptance:** all available checks pass; failures are fixed
and re-run until green. **Do not mark the board `completed` until every available check passes and
its output is recorded on the board.** Depends on requirements validation.
