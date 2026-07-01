# auto-engineer

> One unified skill for all engineering work: code, UI, docs, and repo maintenance.

## Install

```bash
npx skills add mohitmayank/auto-engineer
```

## Usage

```bash
/auto-engineer "Add OAuth2 login with Google and GitHub providers"
/auto-engineer "Redesign the dashboard to look less like AI slop"
/auto-engineer "Write a spec for the notifications system, don't build yet"
/auto-engineer "Run a security audit and fix the flaky CI tests"
/auto-engineer "Build the user profile feature and write documentation for it"
```

## Overview

`auto-engineer` replaces several separate commands with a single adaptive entry point that detects from your prompt which specialization to activate — then loads only the relevant reference files into context.

**ENGINEER** — Full phase-gated SDLC for code work: relentless design interview, reviewed spec, dependency-graphed task board, safe-wave TDD execution, generator-evaluator verification. Adapts depth to complexity: a typo fix gets a one-sentence plan; a greenfield system runs the full pipeline. Includes a **SPEC-lite** mode for design-doc-only handoffs — SCAN → CLARIFY → WRITE → REVIEW, no execution.

**UI** — Design system knowledge with concrete, opinionated rules: exact spacing values, proven color ratios, typography scales, battle-tested component patterns. Makes the difference between AI-generated slop and a polished, crafted UI.

**DOCS** — Diátaxis-driven documentation: correctly classifies every doc into tutorial / how-to / reference / explanation, writes an outline before prose, verifies every claim against the code before it ships.

**MAINTAIN** — Repo-wide health, five sub-modes sharing one DETECT → SCAN → TRIAGE → FIX → VERIFY engine: **AUDIT** (security/CVE scan), **DEBT** (lint/type suppression paydown), **DEFLAKE** (flaky-test root-cause fixing), **PRUNE** (dead code/unused dependency removal), **UPGRADE** (semver-waved dependency bumps). Operates on green main, not the working diff.

Blending is supported — "build this feature and document it" runs ENGINEER then DOCS in the same session, sharing detected conventions.

## Tags

`sdlc` `ui` `docs` `design` `tdd` `spec` `diataxis` `planning` `workflow` `security` `maintenance`

## Platforms

- claude-code
- gemini-cli
- openai-codex
- mcp

## Maintainers

- [@mohitmayank](https://github.com/mohitmayank)
