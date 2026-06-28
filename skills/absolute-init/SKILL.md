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
`typecheck`, `format`, `build`. Detect the default branch with `git symbolic-ref --short refs/remotes/origin/HEAD`
(fallback `main`). Anything you can't resolve confidently → leave it out and ask, or omit.

Also detect, from deps and marker files (omit the whole block if nothing is found):

- **`conventions.format`** — formatter script: `package.json` `format`/`fmt` scripts, a
  `Makefile` `format` target, `ruff format`, or `gofmt`.
- **`conventions.ui`** — from `package.json` deps: `framework` (react/vue/svelte/none),
  `styling` (tailwindcss → tailwind; css-modules; styled-components; else vanilla),
  `iconLibrary` (lucide-react, @heroicons/*, @phosphor-icons/*, react-icons…),
  `componentLib` (shadcn marker / @mui/material → mui / @chakra-ui → chakra / none), and
  `tokensPath` (a tokens/theme CSS or TS file if one exists). Omit if the repo has no UI deps.
- **`conventions.docs`** — `stack` + `dir` via `absolute-docs`'s Stack Detection marker-file
  table (`source.config.ts`/fumadocs, `docusaurus.config.*`, Starlight, `mkdocs.yml`,
  `.vitepress/`, Mintlify, else markdown). Omit if no docs are present.

## Step 2 — RESOLVE existing config

Before asking anything, check for existing config (precedence below). If found, print a
compact summary of current values and ask whether to **update** (default) or start fresh.
Treat existing values as the defaults for the interview so the user can keep them with one keystroke.

## Step 3 — INTERVIEW

Use `AskUserQuestion` for every preference question. Present options as structured choices with descriptions; mark the recommended/default option first with "(Recommended)" in its label. Ask questions one at a time — wait for each answer before advancing.

Questions (skip any the existing config already answers, unless user asked to reconfigure):

**Q1 — Output style**
```
question: "Output style?"
header: "Output style"
options:
  - label: "normal (Recommended)"
    description: "Full prose, explanations — good for onboarding or unfamiliar codebases"
  - label: "terse"
    description: "Compressed, minimal prose — matches caveman mode"
```

**Q2 — Autonomy**
```
question: "Autonomy level?"
header: "Autonomy"
options:
  - label: "gate-all (Recommended)"
    description: "Confirm before every change/wave — full control"
  - label: "auto-low-risk"
    description: "Auto-apply obviously-safe health waves, gate risky ones"
```

**Q3 — TDD strictness**
```
question: "TDD strictness?"
header: "TDD"
options:
  - label: "strict (Recommended)"
    description: "Test-first, red→green — failing test must exist before any code"
  - label: "pragmatic"
    description: "Tests required but not strictly written first"
```

**Q4 — Spec / docs output dir**
Ask as free text: `"Where should spec/work write design docs? (default: docs/plans)"`. Accept blank to keep default.

**Q5 — Relevant families**
```
question: "Which command families do you use?"
header: "Families"
multiSelect: true
options:
  - label: "build (Recommended)"
    description: "work, spec, ui, simplify, docs"
  - label: "health (Recommended)"
    description: "upgrade, audit, prune, debt, deflake"
```

**Q6 — Confirm detected conventions**
Show the detected `conventions` block as a code block and ask: `"Are these correct? Reply with any corrections or press enter to accept."`. Accept blank to proceed.

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
    "format": "npm run format",
    "build": "npm run build",
    "ci": ".github/workflows/ci.yml",
    "defaultBranch": "main",
    "ui": {
      "framework": "react",
      "styling": "tailwind",
      "iconLibrary": "lucide-react",
      "componentLib": "shadcn",
      "tokensPath": "src/styles/tokens.css"
    },
    "docs": {
      "stack": "fumadocs",
      "dir": "docs"
    }
  },
  "preferences": {
    "outputStyle": "normal",
    "autonomy": "gate-all",
    "tdd": "strict",
    "specDir": "docs/plans",
    "boardTracking": "gitignored",
    "families": ["build", "health"],
    "health": {
      "protectedPaths": ["dist/**", "vendor/**", "**/*.generated.*"],
      "deflakeRuns": 20
    }
  }
}
```

The `conventions.ui` and `conventions.docs` blocks are omitted entirely when nothing is
detected (no UI deps, no docs stack), keeping the file minimal.

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
| `conventions.format` | string | Formatter script — `simplify`/`debt` run it instead of re-detecting. |
| `conventions.ui` | object | Cached `framework`/`styling`/`iconLibrary`/`componentLib`/`tokensPath` — `ui` adopts these instead of assuming Tailwind / guessing the icon library. |
| `conventions.docs` | object | Cached docs `stack` + `dir` — `docs` skips marker-file re-detection. |
| `preferences.outputStyle` | `normal` \| `terse` | Verbosity of command responses. |
| `preferences.autonomy` | `gate-all` \| `auto-low-risk` | Whether health commands auto-apply safe waves. |
| `preferences.tdd` | `strict` \| `pragmatic` | `work`'s test-first rigor. |
| `preferences.specDir` | path | Where `spec`/`work` write design docs. |
| `preferences.boardTracking` | `gitignored` \| `git-tracked` | Whether `work`'s `.absolute-work/board.md` is committed — set once instead of asked each intake. |
| `preferences.families` | `["build","health"]` subset | Trims the no-arg menu to what you use. |
| `preferences.health.protectedPaths` | glob[] | Never-delete globs `prune` honors (generated/vendored code). |
| `preferences.health.deflakeRuns` | int | Default N repeat-runs `deflake` uses to establish flakiness. |

---

## Precedence (how commands read config)

At the start of any command, resolve effective config by overlaying, highest wins:

1. `./.absolute.config.json` (project, committed)
2. `~/.absolute/config.json` → `projects["<cwd absolute path>"]`
3. `~/.absolute/config.json` → `defaults`

Shallow-merge `conventions` and `preferences` separately; merge the nested `conventions.ui`,
`conventions.docs`, and `preferences.health` blocks one level deeper so a partial override
doesn't drop sibling keys. If none exist, there is no config — the command soft-recommends
`init` and uses on-the-fly detection.

---

## Gotchas

1. **Overwriting a hand-tuned config.** Always RESOLVE first; merge, don't clobber.
2. **Caching wrong commands.** Cached `test`/`lint` that don't match CI poison every later command — verify each detected script actually runs.
3. **Committing the global file.** `~/.absolute/config.json` is machine-local; only `.absolute.config.json` is committed.
4. **Stale conventions.** A renamed branch or swapped package manager silently misroutes commands — re-run `init` after stack changes.
5. **Treating init as a gate.** It isn't. Commands proceed without it; init is an optimization, not a prerequisite.

---

## Companion commands

- **`/absolute work`** — the main consumer of cached conventions + `tdd`/`autonomy`/`boardTracking` prefs.
- **`/absolute spec`** — reads `conventions` + `specDir` instead of re-detecting.
- **`/absolute ui`** — reads `conventions.ui` (framework, styling, icon library, tokens path).
- **`/absolute simplify`** — reads cached `test`/`lint`/`format`/`typecheck` for auto-verify.
- **`/absolute docs`** — reads `conventions.docs` (stack, dir), skipping marker re-detection.
- **`/absolute-upgrade|audit|prune|debt|deflake`** — health family reads `conventions` for DETECT, `autonomy` for gating, and `preferences.health.*` (`prune` protectedPaths, `deflake` deflakeRuns).
- Re-run **`/absolute init`** anytime conventions or preferences change.
