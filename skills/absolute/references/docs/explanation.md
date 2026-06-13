# Explanation Playbook

Explanation is a **discussion**: understanding-oriented, theoretical. The reader is
studying, away from the keyboard — maybe evaluating the project, maybe trying to
build a mental model after weeks of cargo-culting the how-tos. Your job is to
illuminate: context, reasons, connections, trade-offs.

Explanation is judged by one metric: **does the reader understand the topic more
deeply, and can they now reason about cases the docs never covered?**

## The contract with the thinker

- **No instructions.** The moment numbered steps appear, the page has changed
  quadrant. Describe *that* something is done and *why*; link to the how-to
  for *how*.
- **Perspective is allowed — and must be honest.** Unlike reference, explanation
  may have an opinion ("we chose eventual consistency because..."), but it must
  admit alternatives and costs. An explanation that reads like advocacy teaches
  nothing.
- **Bounded topic.** "About the scheduler" is a topic; "About the project" is a
  landfill. One concept, its context, its connections.
- **Readable away from the machine.** No prerequisites to *execute* anything.
  A good explanation works printed on paper.

## Structure template

```markdown
# <Topic> — but phrased as the question it answers when possible
# e.g. "Why deploys are immutable" / "How the scheduler decides what runs next"

<Opening: the question this page answers and who tends to ask it.>

## The problem / the context

<What situation makes this topic matter. What was true before, what forces apply.>

## How it works / the design

<The mental model. Diagrams welcome. Connect to things the reader already knows;
analogies allowed if honest about where they break.>

## Why this design

<The reasons. The constraints that drove it. What was optimized for.>

## Trade-offs and alternatives

<What this design costs. What the rejected alternatives would have given and
taken. When a different choice would be right.>

## Where this shows up

<Links: the reference entries that embody this, the how-tos it makes sensible.>
```

Adapt freely — explanation is the quadrant where structure may serve the argument.
The template is a default, not a form.

## Writing rules

1. **Lead with the question.** The best explanation titles are questions readers
   actually ask: "Why is there a build step?", "What counts as a breaking change?"
2. **Build from the known to the unknown.** Anchor each new idea to one already
   established — in this page or in a linked one.
3. **Make the forces visible.** Designs are responses to constraints. Name the
   constraints (performance, compatibility, team size, history) and the design
   becomes derivable instead of memorizable.
4. **Honest about history.** "This API is shaped this way because it predates X"
   is more useful than a retconned justification.
5. **Analogies declare their limits.** "Topics are like folders, except a message
   can be in many topics" — the *except* clause is the valuable part.
6. **Diagrams for structure, prose for causality.** A boxes-and-arrows diagram
   shows what connects to what; the text must still say *why*.
7. **No duplicate facts.** Specific signatures, defaults, limits belong in
   reference — link, don't restate, or the copies will diverge.
8. **It may be long, never meandering.** Each section advances the reader's model.
   If a section could be deleted without weakening understanding, delete it.

## Where explanation pages come from

The highest-value explanation topics surface as repeated friction:

- A question asked in three issues/discussions → it wants a page.
- A how-to that keeps growing "background" sections → extract the background.
- A design decision that surprises every new contributor → explain the forces.
- An ADR that users (not just maintainers) keep getting linked to → write the
  public-facing explanation it is standing in for.

## Common defects when improving an existing explanation

- **Steps smuggled in** — a "for example, run..." sequence that became a
  de facto how-to. Extract to a guide, link back.
- **Advocacy without costs** — reads like a launch post. Add the trade-offs
  section; credibility lives there.
- **Unbounded scope** — "Architecture" pages covering nine subsystems at one
  paragraph each. Split into per-concept pages with a short map page on top.
- **Reference duplication** — tables of options with commentary. Keep the
  commentary, link the tables.
- **Staleness of rationale** — "we do X because Y" where Y stopped being true two
  majors ago. Verify claims about the present against the current code; move
  superseded reasoning into an explicit history note.
