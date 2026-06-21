# Reference Playbook

Reference is a **dictionary**: information-oriented, theoretical. The reader is
working and needs a fact — a signature, a default, a constraint — fast. They will
land mid-page from search, grab the fact, and leave.

Reference is judged by one metric: **is it complete, correct, and findable?**
Trust is the product. One wrong default and the reader stops believing the page.

## The contract with the looker-upper

- **Describe, never instruct or advise.** "Returns the user object or `null`" —
  not "you should check for null" (that's a how-to) and not "this elegant API..."
  (that's marketing).
- **Be complete.** Every public option, parameter, error code, and field — no
  "and other minor options". Omissions read as nonexistence.
- **Mirror the code's structure.** Organize by module/command/endpoint exactly as
  the machinery is organized, so location in docs predicts location in code. Do
  not organize reference by use case — that's how-to territory.
- **Be boringly consistent.** Identical section shapes for every entry. The reader
  learns the pattern once and navigates by it.

## Entry templates

### Function / method

```markdown
### `functionName(param1, param2?)`

<One-sentence factual description of what it does.>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `param1` | `string` | — (required) | <what it is, constraints> |
| `param2` | `number` | `100` | <what it is, valid range> |

**Returns:** `Promise<Result>` — <shape of the result>.

**Throws:** `ConfigError` when <condition>.

​```ts
const result = await functionName("orders", 50);
​```
```

### CLI command

```markdown
### `tool deploy [target]`

<One-sentence description.>

| Flag | Default | Description |
|---|---|---|
| `--env <name>` | `production` | <effect> |
| `--dry-run` | `false` | <effect> |

**Exit codes:** `0` success · `1` <condition> · `2` <condition>
```

### Config option

```markdown
### `server.maxConnections`

- **Type:** `number`
- **Default:** `512`
- **Constraints:** 1–65535
- **Since:** v2.3

<What it controls, stated factually. Interactions with other options if any.>
```

### HTTP endpoint

```markdown
### `POST /v1/orders`

<One-sentence description.>

**Request body** / **Query parameters** — table with field, type, required, description.
**Responses** — one block per status code with a real example body.
**Errors** — table of error codes with meaning.
```

Use the stack's structured component (`TypeTable`, OpenAPI integration) when one
exists rather than hand-rolled tables — see `docs-stacks.md`.

## Writing rules

1. **Every fact comes from the source code**, read at writing time. Signature,
   types, defaults, error conditions — find them in the code and copy them.
   Reference written from memory is fiction with a confident tone.
2. **Neutral register throughout.** No "powerful", "simple", "handy", "note that
   you'll love". Adjectives describe constraints ("immutable", "case-sensitive"),
   not quality.
3. **State the negative space**: what happens with invalid input, what `null`
   means, what is *not* guaranteed (ordering, stability, thread-safety).
4. **Examples are minimal and legal** — the smallest call that type-checks and
   shows the shape. Tutorials motivate; reference examples just demonstrate form.
5. **Mark lifecycle states inline**: `**Deprecated** since v3.1 — use X instead.`
   `**Since:** v2.0.` Readers on old versions live here.
6. **Cross-link types**: when a return type or parameter type has its own entry,
   link it.
7. **Alphabetize or code-order consistently** within a section and say nothing —
   the reader should never wonder how a list is sorted.

## Generated vs hand-written

If the project has docstring/OpenAPI/typedoc extraction, **prefer fixing the
source annotations and regenerating** over hand-writing parallel reference that
will drift. Hand-write the connective prose (section intros, conceptual
groupings) around generated cores. If reference must be hand-written, note in
your summary that it will need maintenance on API change.

## Common defects when improving existing reference

- **Advice contamination** — "we recommend setting this to 10" embedded in an
  option table. Move recommendations to a how-to; keep the factual range here.
- **Incompleteness** — code has 14 options, the table has 9. Diff against source
  and fill, flagging any option whose purpose you cannot determine from code.
- **Default drift** — the most damaging stale fact. Verify every default against
  current source.
- **Use-case organization** — entries grouped by scenario instead of structure.
  Reorganize to mirror the code; let how-tos own scenarios.
- **Example-as-tutorial** — 40-line motivated examples per entry. Trim to minimal
  demonstrations; link one good how-to.
