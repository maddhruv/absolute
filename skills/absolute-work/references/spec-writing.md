<!-- Part of the Absolute Work AbsolutelySkilled skill. Load this file during Phase 2 (SPEC) when writing or reviewing the design spec document. -->

# Spec Writing

Reference for producing the design spec during Phase 2. Covers the document template, section
scaling rules, writing style, the decision log, and the scored review protocol.

---

## Spec Document Template

Write to `docs/plans/YYYY-MM-DD-<topic>-design.md` where `<topic>` is a short kebab-case slug
(e.g. `2026-06-03-commenting-system-design.md`).

```markdown
# [Topic] Design Spec

## Summary
<!-- 2-3 sentences. What is being built and why. -->

## Context
<!-- What exists today. Why this change is needed. Link relevant code paths. -->

## Design

### Architecture
<!-- How the pieces fit together. ASCII diagram or description. -->

### Components
<!-- Each new/modified component with its responsibility and file path. -->

### Data Model
<!-- Schemas, tables, types. Code blocks for definitions. -->

### Interfaces / API Surface
<!-- Endpoints, function signatures, event contracts. Code blocks. -->

### Data Flow
<!-- Step-by-step for the key operations. -->

## Error Handling
<!-- Failure modes, retry strategy, user-facing error states. -->

## Testing Strategy
<!-- What to test, how, and at what level (unit/integration/e2e). -->

## Migration Path
<!-- Steps from current to new state. Remove if not applicable. -->

## Open Questions
<!-- Unresolved items. Remove if none remain. -->

## Decision Log
<!-- Key decisions from the interview. See format below. -->
```

---

## Section Scaling Rules

Scale depth to complexity. Remove sections that would only say "N/A".

### Simple (config change, utility, small fix) — ~1 page
Summary (2-3 sentences), Context (1-2 sentences), Components (bullets of what changes), Data
Model / Interfaces only if changed, Testing Strategy (which tests). Skip Architecture, Data Flow,
Migration Path, Open Questions, Decision Log.

### Medium (new component, endpoint, moderate feature) — 2-3 pages
All core sections at moderate depth: Architecture (brief, no diagram needed), Components (table
with name/responsibility/path), full Data Model in a code block, full Interfaces with
request/response shapes, Data Flow (numbered steps), Error Handling (table), Testing Strategy
(specific cases), Decision Log.

### Complex (new system, migration, cross-cutting) — 4-6 pages
Every section at full depth: Architecture with a diagram and component relationships, Components
table with dependencies, full schemas with relationships and indexes, all endpoints/functions
with full types, primary + secondary data flows, comprehensive error handling with retry logic,
test matrix by type, phased Migration Path with rollback, Open Questions with owners, full
Decision Log.

### Complexity heuristic
| Signal | Simple | Medium | Complex |
|---|---|---|---|
| Files touched | 1-2 | 3-8 | 8+ |
| New components | 0 | 1-2 | 3+ |
| External deps | 0 | 0-1 | 2+ |
| Data model changes | none/trivial | new table/type | schema migration |
| Cross-cutting | no | maybe | yes |

---

## Writing Style

### Be concrete, not abstract
| Bad | Good |
|---|---|
| "An endpoint for comments" | `POST /api/posts/:postId/comments` |
| "A component that shows comments" | `src/components/CommentThread.tsx` |
| "Some database table" | `comments` table: `id`, `post_id`, `author_id`, `body`, `created_at` |
| "We'll handle errors" | Return `422 { error: "body_required" }` when the body is empty |

### Include file paths relative to repo root
> The auth middleware at `src/middleware/auth.ts` validates the JWT before the request reaches `src/api/comments/create.ts`.

### Use tables for comparisons and code blocks for interfaces/schemas
```typescript
interface CreateCommentRequest {
  postId: string;
  body: string;
  parentId?: string; // for threaded replies
}
```

### YAGNI
Remove anything not directly needed: don't spec future phases unless they constrain the current
design, don't add "nice to have" sections, don't include sections that only say "N/A", fold
one-sentence sections into a neighbor.

---

## Decision Log Format

Record every decision where more than one reasonable option existed. The Rationale column is the
most important — it prevents future re-litigation.

| Decision | Options Considered | Chosen | Rationale |
|---|---|---|---|
| Database for comments | PostgreSQL, MongoDB, SQLite | PostgreSQL | Already in stack, ACID for threading, full-text search |
| Comment nesting depth | unlimited, flat, 2-level | 2-level | Simple UI, covers 90% of cases, avoids recursive queries |
| Auth for commenting | anonymous, logged-in, mixed | logged-in only | Reduces spam, simplifies moderation, matches existing auth |

Include both decisions the user made explicitly and ones you recommended. Keep each cell to 1-2
sentences.

---

## Scored Spec Review Protocol

After writing the spec, dispatch a **separate** reviewer subagent (generator-evaluator
separation — the agent that wrote the spec does not review it).

### Rubric
| Criterion | Weight | 1 (Fail) | 3 (Acceptable) | 5 (Excellent) |
|---|---|---|---|---|
| **Completeness** | 25% | TODOs, missing sections | Required sections present but thin | Every section substantive for its tier |
| **Consistency** | 20% | Names/types contradict | Mostly consistent, minor mismatches | All names, types, paths match perfectly |
| **Clarity** | 20% | Ambiguous, needs author to interpret | Clear to someone with project context | An unfamiliar dev can build from it |
| **Scope** | 15% | Creep or missing agreed features | Covers discussed topics | Exactly what was discussed |
| **Testability** | 20% | Vague "test the happy path" | Test cases listed but generic | Specific cases with inputs/outputs |

### Thresholds
| Weighted Score | Verdict | Action |
|---|---|---|
| 4.0 - 5.0 | Approved | Proceed to user review |
| 3.0 - 3.9 | Needs Work | Fix flagged issues, re-dispatch (max 3 iterations) |
| < 3.0 | Major Gaps | Surface to the user immediately, do not iterate |

### Reviewer prompt template
```
You are an independent spec reviewer. Grade this spec skeptically.
Do not give benefit of the doubt on vague sections.

Spec complexity tier: [SIMPLE | MEDIUM | COMPLEX]

--- BEGIN SPEC ---
{spec_content}
--- END SPEC ---

--- BEGIN INTERVIEW CONTEXT ---
{interview_summary}
--- END INTERVIEW CONTEXT ---

Score each criterion 1-5 using the rubric. Output (STRICT):

## Spec Review
- **Completeness**: {score}/5 - {justification}
- **Consistency**: {score}/5 - {justification}
- **Clarity**: {score}/5 - {justification}
- **Scope**: {score}/5 - {justification}
- **Testability**: {score}/5 - {justification}
- **Weighted Score**: {calculated}/5.0
- **Verdict**: Approved | Needs Work | Major Gaps

## Specific Issues (required if score < 4.0)
- [Section]: what is wrong and how to fix it

## What Was Done Well
- {1-2 strengths}
```

Reviewer approval is necessary but not sufficient — the user gate in Phase 2 is mandatory
regardless of the reviewer's verdict.

---

## Example Spec (abbreviated, medium tier)

```markdown
# Commenting System Design Spec

## Summary
Add threaded comments (one level deep) to blog posts for logged-in users.

## Context
Blog at `src/app/blog/` uses Prisma + PostgreSQL. No commenting today.

## Design
### Architecture
New API routes at `/api/posts/:postId/comments`, new `comments` table,
React components in `src/components/comments/`.

### Components
| Component | Responsibility | File Path |
|---|---|---|
| CommentThread | Renders comments + replies | `src/components/comments/CommentThread.tsx` |
| CommentForm | Input form | `src/components/comments/CommentForm.tsx` |
| comments API | CRUD endpoints | `src/app/api/posts/[postId]/comments/route.ts` |

### Data Model
Comment: id, body, postId, authorId, parentId (nullable), createdAt, updatedAt.
Indexes on [postId, createdAt] and [parentId].

## Testing Strategy
11 tests: 8 integration (CRUD + auth + pagination + nesting), 2 unit, 1 e2e.

## Decision Log
| Decision | Chosen | Rationale |
|---|---|---|
| Nesting depth | 2-level | Avoids recursive queries, covers 90% of cases |
| Pagination | Cursor-based | Reliable with concurrent inserts |
```
