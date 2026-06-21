# Tutorial Playbook

A tutorial is a **lesson**: learning-oriented, practical. The reader is a newcomer
who has decided to invest time in your project. Your job is to take them by the hand
through a small, complete, *successful* experience. Success builds confidence;
confidence makes users.

The tutorial is judged by one metric: **does it work, first try, for every reader?**

## The contract with the learner

- **You decide everything.** The learner has no basis for choices yet. One package
  manager, one OS path (or clearly-fenced tabs), one example app, one happy path.
- **Every step produces a visible result.** The learner needs constant evidence
  that things are working: a command's output, a page in the browser, a passing
  test. Never two consecutive "invisible" steps.
- **Minimum explanation.** Say only what is needed to complete the step. The
  learner cannot absorb theory while following instructions — link to explanation
  pages for "why".
- **Guaranteed repeatability.** Pin versions. Start from a stated, checkable
  beginning state. Avoid anything that depends on the reader's environment more
  than necessary.

## Structure template

```markdown
# Build your first <thing> with <project>

<One or two sentences: what we will build and what the reader will have at the end.
No history, no architecture, no sales pitch.>

## What you'll need

- <runtime> <version or newer> (`<command to verify>` should print ...)
- <other prerequisite, with a verification command>

## 1. <First action — usually install or scaffold>

<Imperative instruction.>

    <exact command>

<What the learner should now see — exact expected output or a description of it.>

## 2. <Next action>

...

## N. <Final action — the payoff step>

<The visible end result: the running app, the response, the rendered page.>

## What you've done

- <capability gained 1>
- <capability gained 2>

## Next steps

- <link to the most natural how-to guide>
- <link to the explanation of the main concept they brushed against>
```

## Writing rules

1. **Title names the outcome**, not the topic: "Deploy your first function", not
   "Getting started with functions".
2. **"We" voice is allowed and encouraged** — the tutorial is a shared journey:
   "Next, we add the route handler."
3. **Number the steps.** Use the stack's `<Steps>` component when one exists.
4. **Show expected output after every command** that produces any. Use the real
   output, captured by actually running the command.
5. **State the time investment honestly** if you state it at all. "10 minutes"
   that takes 40 destroys trust.
6. **Checkpoint after risky steps**: "If you see `Error: EADDRINUSE`, another
   process is using port 3000 — stop it or change the port in step 3." Only
   pre-empt failures that are *likely*, not every imaginable one.
7. **End-state must be self-evidently working.** "The tutorial is done" is not an
   ending; "open http://localhost:3000 and you'll see your three todos" is.
8. **Keep it small.** A tutorial that takes more than ~30 minutes or builds more
   than one thing should be split. The first success should come early.

## What does NOT belong in a tutorial

| Urge | What to do instead |
|---|---|
| Explain the architecture | One sentence max, link to explanation |
| Offer alternatives ("or use yarn") | Pick one; alternatives live in how-tos |
| Document every option of the command used | Link to reference |
| Handle production concerns (auth, scaling) | "Next steps" links |
| Cover error cases exhaustively | Only the 1–2 most likely stumbles |
| Abstract placeholders (`<your-value-here>`) early on | Concrete values; introduce placeholders only when unavoidable |

## Common defects when improving an existing tutorial

- **Branching paths** — "if you're on Docker, do X, otherwise Y" multiplies the
  untested surface. Collapse to one path or use stack tabs with both paths tested.
- **Stale outputs** — the doc shows v1 output, the tool prints v3 output. Re-run
  everything.
- **Hidden prerequisites** — step 4 silently assumes a database from nowhere.
  Every dependency appears in "What you'll need" or is created in a step.
- **Theory bloat** — paragraphs of concept between steps. Move to explanation,
  keep one orienting sentence.
- **No payoff** — the tutorial ends after installation. Installation is step 1,
  not the destination; the learner must *do* something with the thing.

## Verification before shipping

Run the tutorial yourself, in order, from the stated starting state, in this
environment where possible. Every command executed, every output compared. A
tutorial you have not executed is a draft, not a tutorial. Where the environment
makes a step impossible to run (needs a browser, cloud account), verify the
commands and config against the codebase and say in your summary which steps were
verified by execution and which by inspection.
