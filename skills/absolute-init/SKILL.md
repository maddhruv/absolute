---
name: absolute-init
version: 0.5.0
description: >
  One-time setup for absolute: interview how you want it to behave (output style,
  autonomy, TDD strictness, spec dir, families) + detect the stack once, then write
  `.absolute.config.json` (project, committed) and `~/.absolute/config.json` (user
  defaults + per-project overrides). Every other absolute-* command reads it instead
  of re-detecting; non-blocking — commands proceed without it and soft-recommend it.
  Triggers on "absolute init", "set up absolute", "initialize absolute",
  "configure absolute", "first-time setup", "remember my conventions for this repo".
category: workflow
tags:
  - workflow
  - configuration
  - init
  - setup
  - sdlc
platforms:
  - claude-code
  - gemini-cli
  - openai-codex
  - mcp
user-invocable: true
argument-hint: ""
license: MIT
maintainers:
  - github: maddhruv
---

> "set up / initialize / configure absolute").
> Start your first response with the ⚙️ emoji.

## Absolute Init

One-time setup. Detect the project's real conventions, ask a few questions about how you
want absolute to behave, then write that to JSON config the other ten commands read on
every run. Result: commands stop re-detecting your stack from scratch and respect your
preferences (output style, gating, TDD strictness) without being told each time.

This is the only command that writes config. It is non-destructive: an existing config is
shown and updated, never blindly overwritten. It never commits.

---

## When to use

- First time using absolute in a repo — gives every command cached conventions + your preferences.
- Conventions changed (new package manager, new test/lint scripts, branch rename) — re-run to refresh.
- You want to change how absolute behaves globally (output style, autonomy) across projects.

Other commands run fine without init — they fall back to on-the-fly detection and emit a
one-line suggestion to run it. init just makes them faster and tailored.

---

## Key principles

1. **Codebase before questions.** Detect everything detectable first. Only ask what the repo can't tell you (preferences, ambiguous choices).
2. **A few questions, not a grill.** ~4-6 max, one at a time. This is setup, not design review.
3. **Non-destructive.** Existing config → show it, confirm each change, merge — don't clobber.
4. **Never auto-commit.** Write the project file; tell the user to commit it. (Commit policy itself is not configurable — absolute never commits.)
5. **Two levels, project wins.** Project config is team-shared and authoritative; global is your personal default + per-project overrides.

---

## Step 1 — DETECT

Auto-detect the stack using the **Codebase Convention Detection** table in
`references/work.md` (package manager, language/runtime, test runner, linter/formatter,
build, CI, available scripts). Resolve each to the project's **own script** form
(`npm test`, `make lint`) so cached commands match CI — not raw tools.

Read the actual `package.json` `scripts` / `Makefile` targets to fill `test`, `lint`,
`typecheck`, `build`. Detect the default branch with `git symbolic-ref --short refs/remotes/origin/HEAD`
(fallback `main`). Anything you can't resolve confidently → leave it out and ask, or omit.

## Step 2 — RESOLVE existing config

Before asking anything, check for existing config (precedence below). If found, print a
compact summary of current values and ask whether to **update** (default) or start fresh.
Treat existing values as the defaults for the interview so the user can keep them with one keystroke.

## Step 3 — INTERVIEW

Ask only these, one at a time, pre-filled with detected/sensible defaults:

1. **Output style** — `normal` (default) or `terse` (compressed, minimal prose).
2. **Autonomy** — `gate-all` (default; confirm before every change/wave) or `auto-low-risk` (auto-apply obviously-safe health waves, still gate risky ones).
3. **TDD strictness** — `strict` (default; test-first, red→green) or `pragmatic` (tests required but not strictly first).
4. **Spec / docs output dir** — default `docs/plans`.
5. **Relevant families** — `build`, `health`, or both (default both). Trims the no-arg menu.
6. **Confirm detected conventions** — show the detected `conventions` block; let them correct any command.

Skip any question the existing config already answers unless the user wants to change it.

## Step 4 — WRITE

Write **`.absolute.config.json`** at the repo root (pretty-printed, 2-space). This is the
committed, team-shared file. Then ask whether to also persist to the global file
**`~/.absolute/config.json`**:
- `defaults.preferences` — your cross-project preference defaults.
- `projects["<absolute repo path>"]` — a per-project override entry (machine-local, not committed).

Merge into any existing global file; never drop unrelated keys. After writing, print the
file paths and remind the user to **commit `.absolute.config.json`** (you never commit).

---

## Config schema

Both files share `conventions` + `preferences`; the global file wraps them.

**`.absolute.config.json` (project, committed):**

```json
{
  "version": 1,
  "conventions": {
    "packageManager": "npm",
    "languages": ["typescript"],
    "test": "npm test",
    "lint": "npm run lint",
    "typecheck": "npm run typecheck",
    "build": "npm run build",
    "ci": ".github/workflows/ci.yml",
    "defaultBranch": "main"
  },
  "preferences": {
    "outputStyle": "normal",
    "autonomy": "gate-all",
    "tdd": "strict",
    "specDir": "docs/plans",
    "families": ["build", "health"]
  }
}
```

**`~/.absolute/config.json` (user/global):**

```json
{
  "version": 1,
  "defaults": {
    "preferences": {
      "outputStyle": "normal",
      "autonomy": "gate-all",
      "tdd": "strict",
      "specDir": "docs/plans",
      "families": ["build", "health"]
    }
  },
  "projects": {
    "/Users/you/dev/your-repo": {
      "conventions": { "packageManager": "pnpm", "test": "pnpm test" },
      "preferences": { "outputStyle": "terse" }
    }
  }
}
```

Field meanings:

| Field | Values | Effect |
|---|---|---|
| `conventions.*` | strings | Cached stack/scripts every command runs through instead of re-detecting. |
| `preferences.outputStyle` | `normal` \| `terse` | Verbosity of command responses. |
| `preferences.autonomy` | `gate-all` \| `auto-low-risk` | Whether health commands auto-apply safe waves. |
| `preferences.tdd` | `strict` \| `pragmatic` | `work`'s test-first rigor. |
| `preferences.specDir` | path | Where `spec`/`work` write design docs. |
| `preferences.families` | `["build","health"]` subset | Trims the no-arg menu to what you use. |

---

## Precedence (how commands read config)

At the start of any command, resolve effective config by overlaying, highest wins:

1. `./.absolute.config.json` (project, committed)
2. `~/.absolute/config.json` → `projects["<cwd absolute path>"]`
3. `~/.absolute/config.json` → `defaults`

Shallow-merge `conventions` and `preferences` separately. If none exist, there is no
config — the command soft-recommends `init` and uses on-the-fly detection.

---

## Gotchas

1. **Overwriting a hand-tuned config.** Always RESOLVE first; merge, don't clobber.
2. **Caching wrong commands.** Cached `test`/`lint` that don't match CI poison every later command — verify each detected script actually runs.
3. **Committing the global file.** `~/.absolute/config.json` is machine-local; only `.absolute.config.json` is committed.
4. **Stale conventions.** A renamed branch or swapped package manager silently misroutes commands — re-run `init` after stack changes.
5. **Treating init as a gate.** It isn't. Commands proceed without it; init is an optimization, not a prerequisite.

---

## Companion commands

- **`/absolute work`** — the main consumer of cached conventions + `tdd`/`autonomy` prefs.
- **`/absolute-upgrade|audit|prune|debt|deflake`** — health family reads `conventions` for DETECT and `autonomy` for gating.
- Re-run **`/absolute init`** anytime conventions or preferences change.
