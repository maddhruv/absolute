# Developer Docs Playbook

Repo-level documents readers meet inside the repository rather than on a docs site.
Each maps onto the Diátaxis quadrants — knowing the blend tells you what belongs
and what to evict.

## README

**Blend:** landing page — pitch (one breath) + mini-tutorial (quickstart) + link hub.
The README's job is routing: in 60 seconds the reader knows what this is, whether
they want it, and where to go next.

```markdown
# project-name

<One sentence: what it is and who it's for. No adjectives that a competitor
couldn't also claim.>

<Optional: badges row. Optional: one screenshot/GIF if visual.>

## Why

<2–4 sentences or bullets: the problem and the angle. Honest, concrete.>

## Quickstart

<The shortest real path to a working result — install, minimal use, expected
output. 5 minutes max. This is a micro-tutorial: one path, no choices, verified
by actually running it.>

## Documentation

<Links into the docs site / docs folder, organized by quadrant if there are
enough: Tutorial · How-to guides · Reference · Concepts.>

## Contributing / License

<One line each, linking to CONTRIBUTING.md and LICENSE.>
```

Rules:
- **Under ~300 lines.** A README hoarding full API tables, six tutorials, and an
  FAQ is a docs site in denial — split and link.
- **Quickstart must be executed before shipping**, exactly like a tutorial step.
- **First sentence carries the page.** "A fast, flexible toolkit" says nothing;
  "Convert OpenAPI specs into typed fetch clients" says everything.
- For a **monorepo package**, the README is shorter still: what it is, install,
  one example, link to the root docs.

## CONTRIBUTING

**Blend:** how-to guide(s) for the contributor audience. Assume a competent
developer who has never seen this repo.

Cover, in order of contributor need:
1. **Dev setup** — clone → install → run tests, with the real commands and the
   real expected output. Note required versions and any secrets/services needed.
2. **Day-to-day loop** — run, test, lint, typecheck commands; where the code
   lives; naming/branch conventions actually enforced.
3. **Submitting** — PR expectations, review process, commit format if enforced
   (link the linter config rather than restating it).
4. **What maintainers want / don't want** — issue triage labels, good-first areas,
   features that will be declined (saves everyone a dead PR).

Rules: verify every command; document the enforced reality, not the aspirational
process; if CI is the source of truth, link the workflow file.

## ARCHITECTURE

**Blend:** explanation, with a reference skeleton (the component inventory).
Audience: a contributor about to make their first non-trivial change. Matklad's
test applies: the doc should tell them **where things are and what crosses what**,
so they can navigate without reading everything.

Structure: bird's-eye view (one paragraph + one diagram) → codemap (top-level
directories/modules, one line each, *why it exists* not just what it contains) →
cross-cutting concerns (boundaries, invariants, layering rules that code review
enforces) → key design decisions with links to ADRs.

Rules: stay at the architecture altitude — no function-level detail that rots in
a week; name the invariants ("nothing in `core/` imports from `adapters/`");
update triggers are structural changes, so keep it short enough that updating is
plausible.

## ADR (Architecture Decision Record)

**Blend:** explanation, decision-shaped, **immutable once accepted** — new
decisions get new ADRs that supersede old ones; history is the point.

```markdown
# ADR-NNNN: <Decision as a verb phrase: "Use Postgres for the job queue">

- **Status:** Proposed | Accepted | Superseded by ADR-MMMM
- **Date:** YYYY-MM-DD

## Context
<The forces: requirements, constraints, what hurts today. Facts, not advocacy.>

## Decision
<What we will do, stated in full sentences, active voice: "We will...">

## Consequences
<What becomes easier, what becomes harder, what we are betting on. Include the
negative consequences — an ADR with no downsides recorded is a sales document.>

## Alternatives considered
<Each rejected option with the honest reason it lost.>
```

Number sequentially in `docs/adr/` (or the project's existing location — detect
before inventing). Never edit an accepted ADR's decision; append status changes.

## CHANGELOG

**Blend:** pure reference, reverse-chronological. Follow **Keep a Changelog**
unless the project already does otherwise (detect: existing format wins).

```markdown
## [Unreleased]

## [2.1.0] - 2026-06-10
### Added
### Changed
### Deprecated
### Removed
### Fixed
### Security
```

Rules: entries describe **observable change for the user of the package**, not
internal refactors ("Fixed crash when config file is empty", not "refactored
ConfigLoader"); breaking changes marked loudly and listed first under Changed/
Removed; if the repo generates the changelog from commits, fix the commits or the
generator config — do not hand-edit generated output.

## Runbook

**Blend:** how-to guide under stress. The reader is on call, paged, possibly at
3am, possibly not the service owner. Optimize for a degraded reader.

```markdown
# Runbook: <symptom or alert name, matching the alert text exactly>

**Severity guide:** <when this is urgent vs can wait until morning>
**Dashboard:** <link> · **Logs:** <query link or exact command>

## Confirm
<1–3 checks that distinguish this failure from look-alikes. Copy-pasteable.>

## Mitigate
<Numbered, imperative, copy-pasteable commands. State the blast radius of each
action BEFORE the command. Safest-first ordering.>

## Escalate
<When to stop self-serving and who to wake, with the actual channel/rotation.>

## Root-cause follow-up
<What to capture for the postmortem while evidence is fresh.>
```

Rules: every command literal and copy-pasteable (no `<your-cluster>` placeholders
where the real value is knowable); destructive mitigations flagged before the
command with reversibility stated; test runbooks the way you test tutorials —
stale runbooks fail at the worst possible moment.

## API reference (hand-written portions)

Full treatment in `reference.md`. Repo-specific note: prefer generating from
docstrings/OpenAPI/typedoc when the toolchain exists, and spend hand-written
effort on the prose around the generated core — overview per module, conceptual
grouping, "start here" pointers.
