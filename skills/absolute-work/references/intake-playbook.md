<!-- Part of the Absolute Work AbsolutelySkilled skill. Load this file during Phase 1 (INTAKE & BRAINSTORM) for question banks, codebase-first intelligence, design-tree traversal, and calibration. -->

# Intake Playbook

The design interview is the engine of Absolute Work. A relentless, structured interview
extracts every requirement, constraint, and edge case before a single line of code is written.
This playbook covers design-tree traversal, adaptive question banks per work type, codebase-first
intelligence, question calibration, implicit-requirement extraction, and anti-patterns.

---

## Design Tree Traversal

Every unit of work is a tree of decisions. Walk it **depth-first**, resolving each branch
completely before moving to siblings. This prevents half-explored requirements from haunting
the implementation later.

### Rules
1. **Root first** — start with purpose. Why does this exist? What problem does it solve?
2. **Depth before breadth** — explore the first child fully before siblings.
3. **Resolve before advancing** — a node is resolved when you have a clear answer, a concrete decision, or an explicit deferral. Never leave a node ambiguous.
4. **Backtrack on dead ends** — if a branch leads to "we don't need this," mark it explicitly out of scope and backtrack.
5. **Dependency edges** — if a node depends on another branch, resolve the blocker first.

### Example tree: "Add a commenting system"
```
commenting-system
├── purpose (who comments? what is commentable?)
├── data-model (schema, threading flat vs nested, storage)
├── permissions (create/edit/delete, moderation)
├── ui (input, list, threading display, empty state)
├── real-time (needed? transport? optimistic updates?)
├── notifications (notify on reply? channel?)
└── edge-cases (deleted parent, deleted post, concurrent edits, spam)
```
By the time you reach `notifications`, the threading decision is already resolved, so you know
whether replies even exist. Upstream decisions constrain downstream ones — always interview in
tree order: purpose → data model → behavior → UI → edge cases.

---

## Adaptive Question Banks by Work Type

Detect the work type, then use its bank. Scale depth to complexity (see Scaling Rules below).

### Feature
| # | Question | Purpose |
|---|----------|---------|
| 1 | What is the feature and what user problem does it solve? | Root purpose |
| 2 | Who is the target user? Different roles? | Scope actors |
| 3 | Walk me through the user flow start to finish. | Map the journey |
| 4 | What existing features does this interact with? | Dependency map |
| 5 | What does the happy path look like? | Core behavior |
| 6 | What happens when things go wrong (network, invalid input, missing data)? | Error handling |
| 7 | Any performance requirements (response time, data volume)? | Non-functional |
| 8 | Behind a flag or always-on? | Rollout |
| 9 | What is explicitly out of scope for this version? | Scope boundary |
| 10 | How will we know it works in production? | Observability |

### Bug
| # | Question | Purpose |
|---|----------|---------|
| 1 | Expected vs actual behavior? | Problem statement |
| 2 | How do we reproduce it (steps, environment)? | Reproduction |
| 3 | What is the impact and who is affected? | Priority |
| 4 | When did it start? Any recent changes? | Root-cause hints |
| 5 | Related bugs or known issues? | Context |
| 6 | When is this bug considered fixed? | Success criteria |

### Refactor
| # | Question | Purpose |
|---|----------|---------|
| 1 | What is the specific pain point? | Root cause |
| 2 | What does the ideal end state look like? | Target architecture |
| 3 | Blast radius — how many files, modules, consumers? | Risk |
| 4 | Is there test coverage for the code being changed? | Safety net |
| 5 | Incremental or all-or-nothing? | Strategy |
| 6 | Downstream consumers or public APIs affected? | Breaking changes |
| 7 | Rollback plan if regressions appear? | Safety |

### Greenfield
| # | Question | Purpose |
|---|----------|---------|
| 1 | What problem does this solve, and for whom? | Problem/user fit |
| 2 | The 3-5 core features for v1 — no more? | Scope discipline |
| 3 | Tech stack and hard constraints? | Foundation |
| 4 | Reference implementations or designs to study? | Prior art |
| 5 | High-level data model / core entities? | Data layer |
| 6 | Auth and user roles? | Auth model |
| 7 | Third-party services or APIs? | External deps |
| 8 | Deployment target? | Infrastructure |
| 9 | Testing strategy (unit, integration, e2e)? | Quality gates |
| 10 | If you could ship only one feature, which? | Prioritization |

### Planning / Breakdown
Use when the user wants a vague goal turned into a sequenced plan rather than an immediate build.
| # | Question | Purpose |
|---|----------|---------|
| 1 | What is the end goal, stated as an outcome? | North star |
| 2 | What are the milestones between here and there? | Sequencing |
| 3 | What must ship first to unblock the rest? | Critical path |
| 4 | What is already done or in flight? | Current state |
| 5 | What are the hard deadlines or constraints? | Boundaries |
| 6 | What can be deferred to a later phase? | Scope control |

### Migration
| # | Question | Purpose |
|---|----------|---------|
| 1 | What is being migrated, and to what? (v2→v3, JS→TS, lib A→B) | Problem statement |
| 2 | Full migration or incremental? | Strategy |
| 3 | Must old and new coexist during migration? | Constraints |
| 4 | Rollback plan if something breaks? | Safety |
| 5 | Known breaking changes? | Risk |
| 6 | Test coverage of the code being migrated? | Safety net |
| 7 | Priority order of modules to migrate? | Sequencing |

For migrations, **also load `migration-playbook.md`** — it covers call-site inventory, codemods,
incremental rollout, and backwards-compatibility in depth.

---

## Codebase-First Intelligence

Before asking the user a question, check whether the codebase already has the answer. Every
question the codebase could have answered is a wasted round-trip that erodes trust.

| Before Asking About | Search For | Where |
|---|---|---|
| Database / ORM | prisma, typeorm, mongoose, drizzle deps + config | `package.json`, `prisma/schema.prisma`, `*.config.*` |
| Authentication | auth middleware, JWT, session, next-auth, clerk | `middleware/auth*`, `lib/auth*`, `package.json` |
| Testing framework | test config + existing test files | `jest.config*`, `vitest.config*`, `package.json` scripts |
| State management | stores, context, redux/zustand/jotai | `**/store/**`, `**/context/**`, `package.json` |
| Styling | tailwind/postcss config, styled-components | `tailwind.config*`, `*.module.css` |
| Deployment | CI/CD, Dockerfiles, deploy config | `.github/workflows/*`, `Dockerfile`, `vercel.json` |
| Lint / format | eslint, prettier, biome | `.eslintrc*`, `.prettierrc*`, `biome.json` |

### Protocol
For every question you are about to ask:
1. **Can I find it in the codebase?** Search; if found, state it and skip the question.
2. **Can I infer it?** If the project uses Prisma + Postgres, don't ask "what database?" — state it and ask the deeper question.
3. **Is this a fact or a preference?** Facts live in code (test framework). Preferences require asking (desired coverage level, visual style, real-time vs batch).

---

## Question Calibration

### Multiple choice vs open-ended
- **Multiple choice** when there are 2-4 known options, the user may not know terminology, or speed matters. Always include a **(Recommended)** option with rationale.
- **Open-ended** when the answer space is unbounded or you need the user's mental model.

### When a question is too broad
If the user would need more than 3 sentences to answer well, split it.

| Too broad | Better |
|---|---|
| "How should the notification system work?" | "In-app, email, or both?" then "Badge, dropdown, or full page?" |
| "What are the security requirements?" | "Who can access this resource?" then "Do we need rate limiting?" |

### Rules
1. **One decision per question.** If your question contains "and," consider splitting.
2. **No compound conditionals.** Resolve X first, then ask the follow-up.
3. **Ground in the codebase.** "I see you use Express with middleware routing — should new auth endpoints follow the same pattern?"
4. **Offer a recommendation when you can,** tied to project context, not popularity.
5. **Timebox complexity.** If a question opens a 20-minute rabbit hole, flag it and offer to defer with a placeholder.

---

## Extracting Implicit Requirements

Users say what they want; they rarely say what they need. Surface hidden requirements as
follow-up questions — do not assume.

| User Says | Hidden Requirements |
|---|---|
| "Add notifications" | Channel (in-app/email/push), read state, preferences, digest mode, notification center |
| "Make it real-time" | Transport, reconnection, optimistic updates, conflict resolution, offline handling |
| "Add user roles" | Permission model, assignment UI, hierarchy, admin override, audit logging |
| "Support file uploads" | Max size, formats, virus scanning, storage backend, progress, resume, thumbnails |
| "Add search" | Full-text vs exact, indexing, debounce, highlighting, facets, empty state, pagination |
| "Make it work offline" | Sync strategy, conflict resolution, storage limits, cache invalidation, sync status |
| "Deploy to production" | CI/CD, env config, monitoring, rollback plan |

### Extraction protocol
1. Acknowledge the stated requirement.
2. Surface the 2-3 most **architecture-affecting** hidden requirements as questions.
3. Do not dump all hidden requirements at once — prioritize, then circle back to polish.

---

## Scaling Rules

| Tier | When | Questions |
|---|---|---|
| **Simple** | 1-2 files, clear scope, no external deps | 3 — always: problem, success criteria, constraints |
| **Medium** | 3-5 files / 2+ components, some ambiguity | 5 — add: existing code context, dependencies |
| **Complex** | 5+ files, cross-cutting, greenfield, migration | 8-10 — add: edge cases, testing, docs, rollout, priority |

Heuristic: count files touched, presence of external deps, scope definition, and whether data
migration/backwards-compat is involved. When in doubt, ask one more question, not one fewer.

---

## Anti-Patterns

1. **Asking what the codebase can answer** — search configs and deps first.
2. **Batching unrelated questions** — the user answers the easy one and skips the hard one. One at a time.
3. **Implementation before purpose** — resolve what and why before how. Transport choices without context are coin flips.
4. **Accepting vague answers** — "handle errors gracefully" means something different to everyone. Ask for a concrete example.
5. **Skipping error/edge branches** — edge cases are where bugs live. Ask "what happens at 0 items? 10,000? on failure?"
6. **Leading questions** — "we should use Redis here, right?" confirms your bias. Ask the open question.
7. **Skipping the out-of-scope conversation** — without explicit scoping, the feature grows silently.
8. **Interviewing out of order** — designing UI before the data model risks designing something the data can't support.
