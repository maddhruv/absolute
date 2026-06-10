---
name: absolute-documentations
version: 0.1.0
description: >
  Diátaxis-driven documentation writing, improvement, and auditing for AI agents.
  Writes public-facing product docs (tutorials, how-to guides, reference,
  explanation) and repo developer docs (README, CONTRIBUTING, ARCHITECTURE, ADRs,
  changelogs, runbooks), improves existing pages to their quadrant's standard, and
  audits whole doc sites against the Diátaxis map. Detects the docs stack
  (Fumadocs, Docusaurus, Starlight, MkDocs, VitePress, Mintlify, plain Markdown)
  and follows its conventions. Triggers on "write docs", "document this",
  "write a tutorial", "write a README", "improve this doc", "audit our docs",
  "restructure the documentation", or "absolute-documentations this".
category: writing
tags:
  - documentation
  - diataxis
  - technical-writing
  - developer-docs
  - readme
  - docs-site
recommended_skills: [absolute-work, absolute-ui]
platforms:
  - claude-code
  - gemini-cli
  - openai-codex
  - mcp
sources:
  - url: https://diataxis.fr/
    accessed: 2026-06-10
    description: The Diátaxis framework for technical documentation authoring
license: MIT
maintainers:
  - github: maddhruv
---

When this skill is activated, always start your first response with the 📚 emoji.

# Absolute Documentations: Diátaxis-Driven Documentation

Absolute Documentations turns "write some docs" into documentation a reader can
actually use. Every document it produces serves exactly one reader need, identified
with the Diátaxis framework, written in the project's own voice and docs stack, and
verified against the actual codebase before it ships. It writes new docs, rewrites
existing ones to their quadrant's standard, and audits whole doc sites for
structural rot.

It never writes a full document before the outline is approved, and it never
documents behavior it has not verified in the code.

---

## The Diátaxis Compass

Every piece of documentation answers exactly one kind of reader need. Classify
before writing — a page that mixes quadrants serves nobody.

|  | **Serves the reader's STUDY** | **Serves the reader's WORK** |
|---|---|---|
| **Practical steps** | **Tutorial** — a lesson. Guides a newcomer through a guaranteed-success experience. | **How-to guide** — a recipe. Helps a competent user accomplish a specific goal. |
| **Theoretical knowledge** | **Explanation** — a discussion. Deepens understanding of a topic, gives context and reasons. | **Reference** — a dictionary. States facts about the machinery, completely and neutrally. |

To classify, ask two questions:

1. **Is the reader studying (acquiring skill) or working (applying skill)?**
2. **Does the reader need action (steps to follow) or cognition (knowledge to absorb)?**

| Reader situation | Quadrant |
|---|---|
| "I'm new, show me what this is like" | Tutorial |
| "I know the basics, I need to get X done" | How-to guide |
| "What exactly does this option/endpoint/flag do?" | Reference |
| "Why does it work this way? What's the bigger picture?" | Explanation |

**The cardinal sin is mixing.** A tutorial that stops to explain architecture loses
the learner. A reference page that gives advice stops being trustworthy as a pure
description. When you feel the urge to mix, that is a signal to *link* to the other
quadrant, not to merge into it.

---

## Modes

Detect the mode from the request:

| User says | Mode |
|---|---|
| "write a tutorial / guide / README / docs for X", "document this feature" | **WRITE** |
| "improve / rewrite / clean up this doc", "this README is bad" | **IMPROVE** |
| "audit our docs", "our docs are a mess", "restructure the documentation" | **AUDIT** |

---

## WRITE Mode

### Step 1 — Recon (codebase first, questions second)

Before asking the user anything, learn everything the repo can teach:

- **Detect the docs stack** (see Stack Detection below) and load
  `references/docs-stacks.md` if writing site pages.
- **Read existing docs** — tone, terminology, heading style, frontmatter schema,
  sidebar/nav structure, where each quadrant lives.
- **Read the code being documented** — public API surface, actual option names,
  actual defaults, actual error messages. The code is the source of truth, not
  your memory of similar tools.
- **Check project metadata** — package.json/pyproject/go.mod for the real name,
  version, install command, supported runtimes.

### Step 2 — Intake

Four things must be pinned down before any outline. Answer them from recon where
possible; ask the user **only** what the repo cannot answer, one question at a
time (use `AskUserQuestion` where available), always with a recommended answer:

1. **Document type** — which Diátaxis quadrant (or which developer-doc form).
2. **Target audience** — novice end user? experienced operator? contributor?
   What can you assume they already know?
3. **Reader's goal** — what will the reader be able to do after reading?
4. **Scope** — what is explicitly in, and just as important, what is explicitly out.

### Step 3 — Outline gate (hard gate)

Propose, before writing any prose:

- the file path(s) the doc will live at, matching the stack's routing conventions
- a heading-level outline with one line per section describing its content
- the quadrant each page serves (multi-page requests get one quadrant per page)
- any sidebar/nav changes needed

**STOP and wait for explicit approval.** Do not write the document until the user
confirms the outline. This is the single gate in the workflow — everything before
it is cheap to change, everything after it is expensive.

### Step 4 — Write

- Follow the per-quadrant playbook in `references/` (load the matching file).
- Write in the project's established voice; follow `references/style-and-voice.md`.
- Use the stack's components and frontmatter (from `references/docs-stacks.md`);
  plain Markdown when no stack is detected.
- Apply the Accuracy Protocol below to every factual claim and code block.

### Step 5 — Self-review

Score the draft against the rubric below. Fix anything scoring under 4 before
presenting. Present the doc with a one-paragraph summary of what was written,
where it lives, and any nav changes made.

---

## IMPROVE Mode

For "fix this README" / "improve this page":

1. **Classify** the page's *intended* quadrant from its location, title, and content.
   If it serves two masters, say so — that is usually the root problem.
2. **Diff against the quadrant's standard** (load its reference playbook). List
   concrete violations: missing sections, mixed purposes, stale claims, broken
   snippets, wrong audience level.
3. **Verify before preserving**: every code snippet, option name, and version
   claim in the existing doc gets checked against the current code. Stale facts
   are the most common defect in old docs.
4. **Rewrite** preserving everything accurate and project-specific. Do not bleach
   the project's voice into generic doc-speak.
5. Single-page improvements need no gate. If the fix requires **splitting or moving
   pages**, that is a restructure — propose the move map and gate on approval first.

---

## AUDIT Mode

For "our docs are a mess" / "audit the documentation":

1. **Inventory** — list every docs page (site pages, README, docs/ folder) with
   path and title.
2. **Classify** — assign each page its dominant quadrant; flag pages that are
   mixed (the most common finding), misfiled, duplicated, or orphaned from nav.
3. **Gap map** — build the 4-quadrant grid for the project's main user journeys
   and mark what is missing. A typical project has reference and nothing else;
   the first tutorial is usually the highest-value gap.
4. **Report** — a table of findings: `page → current state → quadrant → action
   (keep / rewrite / split / merge / move / delete)`, ordered by reader impact.
5. **Gate** — restructuring moves files and breaks links. Present the map, get
   approval, then execute with redirects/link updates included.

The audit deliverable is the report and map. Executing it is a follow-up the user
approves explicitly.

---

## Stack Detection

Check for marker files in this order; first match wins:

| Marker | Stack | Content format |
|---|---|---|
| `source.config.ts` / `fumadocs-*` in package.json | Fumadocs | MDX + fumadocs-ui components |
| `docusaurus.config.*` | Docusaurus | MDX + admonitions (`:::note`) |
| `astro.config.*` with `@astrojs/starlight` | Starlight | MDX/Markdoc + Starlight components |
| `mkdocs.yml` | MkDocs (often Material) | Markdown + admonitions (`!!! note`) |
| `.vitepress/config.*` | VitePress | Markdown + containers (`::: tip`) |
| `mint.json` / `docs.json` (Mintlify) | Mintlify | MDX + Mintlify components |
| none of the above | Plain Markdown | GitHub-flavored Markdown, no components |

Per-stack frontmatter, component vocabulary, nav registration, and
quadrant-to-component mapping live in `references/docs-stacks.md` — load it
whenever writing pages for a detected stack. Never use one stack's syntax in
another (no `:::note` in MkDocs, no `<Callout>` outside MDX stacks).

---

## Quadrant Rules at a Glance

Full playbooks with templates live in `references/`. The non-negotiables:

| Quadrant | Must | Must not |
|---|---|---|
| **Tutorial** | Work first try, every time; concrete single path; visible result at every step; first person plural ("we") | Offer choices, explain theory in-line, assume unstated setup, branch |
| **How-to** | Start from a real task; assume competence; state prerequisites; show the steps and only the steps | Teach basics, explain why at length, cover every edge case inline |
| **Reference** | Be complete, accurate, and neutral; mirror the code's structure; state defaults, types, constraints | Give advice, tell stories, omit "obvious" entries, drift from the code |
| **Explanation** | Give context, reasons, trade-offs, history; admit alternatives; connect concepts | Contain instructions, pretend to be the only valid view, duplicate reference facts |

| Developer doc | Quadrant blend | Playbook |
|---|---|---|
| README | Landing page: pitch + quickstart (mini-tutorial) + links out | `references/developer-docs.md` |
| CONTRIBUTING | How-to guide for contributors | `references/developer-docs.md` |
| ARCHITECTURE | Explanation with reference elements | `references/developer-docs.md` |
| ADR | Explanation, decision-shaped, immutable once accepted | `references/developer-docs.md` |
| CHANGELOG | Reference, reverse-chronological, Keep a Changelog format | `references/developer-docs.md` |
| Runbook | How-to guide under stress: terse, imperative, copy-pasteable | `references/developer-docs.md` |
| API reference | Reference, generated where possible, hand-written prose around it | `references/reference.md` |

---

## Accuracy Protocol

Documentation that lies is worse than no documentation. For every draft:

1. **Code snippets come from the codebase**, not from memory. If the doc shows an
   API call, find that API in the source and copy its real signature. If a snippet
   is runnable in this environment, run it.
2. **Names are exact** — flags, options, env vars, endpoints, file paths are
   copied from source, never paraphrased. `--dry-run` and `--dryrun` are
   different products.
3. **Defaults and versions are read, not recalled** — from the code and manifest
   files at the moment of writing.
4. **Never document what does not exist.** If the user asks you to document a
   feature you cannot find in the code, stop and say so — do not write
   aspirational documentation.
5. **Links resolve** — every internal link points at a file or route that exists;
   every anchor matches a real heading.
6. **Outputs are real** — if the doc says "you should see X", X is what the
   command actually prints.

---

## Style Core

The full guide is `references/style-and-voice.md`. The rules that are never waived:

1. One idea per sentence. One purpose per paragraph. One quadrant per page.
2. Address the reader as "you" (tutorials may use "we" for shared journey).
3. Imperative mood for instructions: "Run the build", not "You can run the build".
4. Ban the condescension words: *simply, just, easy, obviously, of course*.
   If it were simple, the reader wouldn't be here.
5. Present tense, active voice. "The server returns 404", not "a 404 will be
   returned by the server".
6. Every code block declares its language. File-content blocks name the file.
7. Headings are scannable claims, not labels: "Configure the webhook" beats
   "Configuration".
8. Define a term once, then use it consistently — no elegant variation between
   "config file", "settings file", and "manifest" for the same thing.
9. Warnings come *before* the dangerous step, never after.
10. Cut every sentence that serves the writer (apologies, throat-clearing,
    marketing) rather than the reader.

---

## Self-Review Rubric

Score 1–5 on each axis before presenting. Anything under 4 gets fixed first.

| Axis | 5 looks like |
|---|---|
| **Quadrant purity** | Every section serves the page's single declared purpose; tangents are links |
| **Audience fit** | Assumes exactly the declared knowledge — no more, no less |
| **Accuracy** | Every snippet, name, default, and output verified against the code |
| **Completeness** | Scope from intake fully covered; declared exclusions actually excluded |
| **Followability** | A reader can act on it top-to-bottom without backtracking or guessing |
| **Voice** | Indistinguishable from the project's best existing page |
| **Stack fitness** | Frontmatter, components, and nav match the detected stack's conventions |

---

## Red Flags — stop and fix

- A tutorial offering the reader choices ("you can use npm or pnpm or...") —
  pick one, mention alternatives in a how-to.
- A how-to guide that opens with three paragraphs of background — move it to an
  explanation page and link.
- Reference material with personality ("this handy option...") — neutralize it.
- An explanation page containing numbered steps — extract them to a how-to.
- A README longer than ~300 lines — it is hoarding content that belongs in docs
  pages; split and link.
- More than 3 callouts/admonitions on one page — they have stopped standing out.
- A code block with no language tag, or a snippet you have not verified.
- "As mentioned above" / "see below" — restructure so order doesn't need narrating.
- Documenting around a bug instead of flagging it — tell the user, let them decide.

---

## References

Load on demand from `references/`:

| File | Load when |
|---|---|
| `tutorials.md` | Writing or fixing a tutorial / getting-started page |
| `how-to-guides.md` | Writing or fixing a how-to / task guide |
| `reference.md` | Writing or fixing reference / API docs |
| `explanation.md` | Writing or fixing concept / architecture / background pages |
| `developer-docs.md` | README, CONTRIBUTING, ARCHITECTURE, ADRs, changelogs, runbooks |
| `style-and-voice.md` | Any prose-heavy writing; calibrating to project voice |
| `docs-stacks.md` | A docs stack was detected; writing site pages |

---

## What this skill does not do

- **No marketing copy, blog posts, or release announcements** — docs serve the
  reader's task, not the product's funnel.
- **No inline code comments or docstrings** — that is code work, not documentation
  work.
- **No invented features** — if it is not in the code, it is not in the docs.
- **No commits** — it writes files and reports; the user reviews and commits.
