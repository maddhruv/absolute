---
name: absolute-deflake
version: 0.5.0
description: >
  Flaky test fixes: detect nondeterministic tests empirically (repeat/shuffle/parallel runs), diagnose the root cause, fix it — never retry/skip/sleep — and verify across many randomized runs.
  Triggers on "absolute deflake", "fix flaky tests", "CI is flaky", "this test fails randomly/intermittently".
category: workflow
tags:
  - workflow
  - testing
  - flaky-tests
  - maintenance
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

> Start your first response with the 🧪 emoji.

## Absolute Deflake

Find tests that pass and fail nondeterministically, diagnose the **root cause** of each,
and fix it — not by retrying or skipping, but by removing the source of nondeterminism.
Output is evidence (failure rate per test) → cause → fix, verified by repeated runs.

Runs the shared engine in **`references/health-engine.md`** — read it for the
DETECT → SCAN → TRIAGE → FIX → VERIFY → REPORT loop and the safety contract. This file
covers only what's specific to flaky tests.

---

## When to use

- "Our CI is flaky", "this test fails randomly", "fix the intermittent failures".
- A test passes locally but fails in CI (or vice versa), or fails ~1 in N runs.
- Burning down a backlog of `retry`/`skip`-marked tests that mask real flakiness.

Not for tests that fail *deterministically* — that's a real bug or a real regression
(`/absolute work` for a fix, or just fix it). `deflake` targets *nondeterministic* failures.

---

## What it scans

Establish flakiness empirically — a test isn't flaky because someone said so. Use
`preferences.health.deflakeRuns` from config as the default N for repeat-runs (else 20):

| Ecosystem | Repeat-run / detect |
|---|---|
| Jest/Vitest | run suite N× (`--run` loop), randomize order (`--shuffle` / `testSequencer`) |
| pytest | `pytest-randomly` + `pytest --count=N` (`pytest-repeat`); `-p no:randomly` to A/B |
| Go | `go test -count=N -shuffle=on ./...`, `-race` |

Also mine signals: existing `retry`/`flaky`/`skip` annotations, CI history if reachable, and
run the suite both **in isolation** and **in full/parallel** — order- and concurrency-
dependent failures only show one way. Record a **failure rate** per suspect test.

---

## Common root causes (diagnose, don't guess)

| Cause | Tell | Fix |
|---|---|---|
| Test-order / shared state | passes alone, fails in suite (or vice versa) | isolate state; reset/teardown between tests |
| Time / clock | fails near midnight, DST, or under load | fake timers / inject clock; no real `sleep` |
| Async race / missing await | fails under parallelism or slow CI | await the actual condition; no fixed timeouts |
| Randomness | fails ~X% with no pattern | seed the RNG; fix the seed in tests |
| Network / external I/O | fails offline or on slow links | mock/stub the boundary |
| Unordered collections | fails on map/set iteration order | sort before asserting |
| Resource leak / port reuse | fails on repeat or parallel runs | unique resources; clean up |

---

## Risk ranking (TRIAGE)

| Wave | Class | Default |
|---|---|---|
| 1 | clear, isolated cause (seed, await, fake clock, sort) | fix now |
| 2 | shared-state / ordering — needs fixture refactor | fix this pass, per test |
| 3 | flakiness pointing at a **real product race**, not just the test | gated — surface; may be a genuine bug to fix in code |

A flaky test sometimes means the *code* has a race, not the test. Don't "stabilize" the test
into hiding a real concurrency bug — flag wave-3 cases for a real fix.

---

## Fix & verify

- Fix the **cause**. Then prove it: re-run the test **many times** (and shuffled / parallel /
  with `-race`) — green once is not deflaked; green across N randomized runs is.
- Remove the `retry`/`skip`/`flaky` annotation that was masking it once the cause is fixed.
- **Never** "fix" by adding retries, raising timeouts blindly, `sleep`, or skipping the test —
  that hides flakiness, doesn't remove it.
- Re-run the **full** suite to confirm the fix didn't destabilize neighbors.

---

## Gotchas

1. **Retry/skip as a fix.** Masks the flake, ships the nondeterminism. Forbidden here.
2. **`sleep` to dodge a race.** Slows the suite and still flakes under load. Await the condition.
3. **One green run = done.** Flakes are probabilistic — verify with many randomized runs.
4. **Stabilizing a real product race.** If the *code* races, fix the code, not just the assertion.
5. **Ignoring order/parallel dimension.** Run isolated *and* in-suite; the bug hides in whichever you skip.

---

## Companion commands

- **`/absolute upgrade`** — a flaky suite makes upgrade verification unreliable; deflake first.
- **`/absolute debt`** — flaky-test annotations are test debt; this clears them at the root.
- **`/absolute work`** — when the flake is a genuine product-code race needing real design.
