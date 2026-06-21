# How-to Guide Playbook

A how-to guide is a **recipe**: goal-oriented, practical. The reader is a competent
user in the middle of real work with a specific problem. They are not here to learn
your project — they are here to get something done and leave.

The how-to is judged by one metric: **does it get the reader from their situation to
their goal with no wasted motion?**

## The contract with the worker

- **Assume competence.** The reader can install software, read code, and use a
  terminal. Do not re-teach basics the tutorial covered — link to it for newcomers.
- **Start from the task, not the tool.** The title and framing match what the
  reader is trying to do ("Rotate API keys without downtime"), not the feature
  that happens to be involved ("Using the KeyManager class").
- **Steps, not lessons.** Action sequence with minimal connective tissue. The
  reader will adapt your recipe to their context — write it so adaptation is easy.
- **Real-world shaped.** How-tos may address messy realities tutorials avoid:
  existing data, production constraints, partial migrations.

## Structure template

```markdown
# How to <accomplish specific goal>

<One sentence: when you'd need this. One sentence: what the approach is.>

## Prerequisites

- <state the reader must already be in — versions, access, prior setup>

## Steps

### 1. <Action>

<Instruction. Code/command.>

### 2. <Action>

...

## Verify it worked

<How the reader confirms success — a command and its expected signal.>

## Troubleshooting        <!-- only if there are known, common failures -->

**<Symptom>** — <cause>. <Fix.>

## Related

- <link to reference for the options used>
- <link to explanation for the concept involved>
```

## Writing rules

1. **Title starts with "How to" + a verb + the user's goal.** If you cannot name
   the goal concretely, you do not have a how-to topic yet.
2. **"You" voice, imperative mood.** "Set `maxRetries` to 0", not "the user
   should consider setting...".
3. **Prerequisites are a checklist of state, not a story.** Each one verifiable.
4. **One reliable path through the task.** Where genuinely different contexts need
   different steps (npm vs pnpm, staging vs prod), use the stack's tabs component —
   never inline "if/otherwise" prose forks.
5. **Every step earns its place.** If a step is optional, either cut it or mark it
   `(optional)` with the condition that makes it needed.
6. **Always end with verification.** A recipe without "how do I know it worked" is
   half a recipe.
7. **Link instead of explaining.** One clause of "why" is fine when it prevents
   misuse; three sentences of why is an explanation page leaking in.
8. **Flag destructive steps before them**, with the stack's warning callout:
   what is destroyed, whether it is reversible, how to back up.

## Scoping: one guide or several?

A how-to covers **one goal**. Signals you have accidentally scoped two guides:

- The title contains "and" joining two outcomes.
- Steps 1–5 and steps 6–11 could each be followed alone usefully.
- You need two different "Verify it worked" sections.

Split them and cross-link. Conversely, do not shred one goal into three stub pages —
a guide shorter than its own boilerplate should be folded into a sibling.

## What does NOT belong in a how-to

| Urge | What to do instead |
|---|---|
| Teach the underlying concept | Link to explanation |
| List every flag of the command | Link to reference; show only flags used |
| Cover all possible starting states | Pick the common one; note assumptions in prerequisites |
| Guarantee beginner success | That's the tutorial's job; assume competence |
| Inline lengthy edge-case handling | Troubleshooting section, or its own guide |

## Common defects when improving an existing how-to

- **Tutorial cosplay** — opens with "What is X?" and installs from scratch.
  Delete the lesson; demand prerequisites instead.
- **Feature-shaped, not task-shaped** — "Using webhooks" tells the reader nothing
  about whether their problem is in here. Re-title around the goal and reorganize.
- **No verification step** — add one; it is the single highest-value fix.
- **Drift** — steps reference UI labels, flags, or file paths that have since
  changed. Verify every name against the current code.
- **Choice paralysis** — three alternative approaches presented equally. Recommend
  one, state when the alternatives win, or split the guides.
