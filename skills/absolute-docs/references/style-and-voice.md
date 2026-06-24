# Style and Voice

Prose craft for documentation. The Style Core in SKILL.md is the law; this file is
the case law.

## Calibrating to the project's voice

Before writing a word, read the project's best existing pages and extract:

- **Formality register** — "Heads up: this bites" vs "Note: this behavior may be
  unexpected". Match it; a register clash reads as outsourced docs.
- **Terminology map** — the project's words for its own concepts. Build a small
  glossary from existing docs and code identifiers, and never introduce a synonym.
  If the code says `workspace`, the docs never say "project folder".
- **Person and mood norms** — some projects say "we", some never do.
- **Structural conventions** — heading capitalization (sentence case vs Title
  Case), whether pages open with frontmatter descriptions, callout frequency.

When the project has no docs yet, default to: sentence-case headings, "you",
imperative instructions, contractions allowed, low-key register.

## Sentences

- **Lead with the point.** Main clause first, qualifications after: "Restart the
  worker after changing the config" — not "After changing the config, in most
  cases, depending on your setup, you may need to restart the worker."
- **One instruction per sentence.** "Run X and then edit Y and restart Z" hides
  three steps; readers execute sentences one at a time.
- **Active voice, named actor.** "The CLI writes a lockfile" — the reader learns
  *what does what*. Passive voice hides the actor exactly where docs need it most.
- **Present tense for behavior.** "Returns null if missing", not "will return".
  Future tense only for genuinely future things (roadmap, deprecation timelines).
- **Concrete over abstract.** "Responses over 8 MB are truncated" beats "large
  responses may be subject to truncation".

## Words

Banned, with replacements:

| Banned | Why | Instead |
|---|---|---|
| simply, just, easily | Shames the struggling reader | Delete the word |
| obviously, of course, clearly | If true, unnecessary; if false, insulting | Delete |
| should work | Hedging — does it or doesn't it? | Verify, then state what happens |
| please | Docs are not asking a favor | Imperative verb |
| utilize, leverage | Inflation | use |
| in order to | Inflation | to |
| note that | Throat-clearing | Delete, or use a callout if it truly interrupts |
| etc. | Reads as "I stopped thinking" | Finish the list or say what bounds it |
| robust, powerful, seamless | Marketing in docs clothing | The concrete property meant |

- **Acronyms expand on first use** per page (pages are entered mid-site from
  search), except those the declared audience certainly knows.
- **UI labels, flags, filenames, and identifiers** are verbatim in backticks.
  Bold for UI elements being clicked is acceptable if the project already does it.

## Paragraphs and structure

- **First sentence of each section does the section's work** — readers scan
  first sentences. If the first sentence is setup, swap it with the payoff.
- **Lists for parallel items, prose for reasoning.** Three parallel options are a
  list; a chain of cause and effect is a paragraph. Bullet-pointing an argument
  destroys its logic.
- **Tables for facts with shared attributes** (name/type/default). Never put
  reasoning in table cells.
- **Heading depth ≤ 3** (`###`). Deeper means the page wants splitting.
- **Headings make sense alone** — the right-rail TOC is read out of context.
  "Configure the webhook" works there; "Configuration" and "More details" don't.

## Code in prose

- Language tag on every fence, always. `text` for output, `bash` for shell.
- Shell blocks: no `$` prompt prefixes (breaks copy-paste); output goes in a
  separate `text` block or after a clear marker.
- Long commands: backslash-continuations at logical boundaries, one flag per line.
- Placeholders only where the value is genuinely the reader's: `<YOUR_API_KEY>`
  in SCREAMING form, explained at first use. Knowable values are written out.
- A snippet that depends on earlier snippets says so — or better, repeat the
  minimum context so each block is independently runnable.

## Callouts and emphasis

- Callouts interrupt; that is their cost. **Max 3 per page.** When everything is
  highlighted, nothing is.
- Severity honestly: info for "good to know", warning for "you may lose time",
  danger for "you may lose data". Crying danger over a cosmetic issue burns the
  signal for the real one.
- **Warnings precede the step they protect.** A warning after the dangerous
  command is a postmortem.
- Bold sparingly for the one load-bearing phrase in a section; italics for term
  introduction; never underline; never ALL CAPS prose.

## Audience calibration

State the assumed knowledge in your head before writing, then hold it:

- **Novice-facing** (tutorials): every term either common knowledge, defined
  in-line, or linked at first use. No unexplained jargon, no "as you know".
- **Practitioner-facing** (how-to, reference): domain terms used freely; *project*
  terms still linked at first use. Explaining what JSON is insults this reader.
- **Contributor-facing** (CONTRIBUTING, ARCHITECTURE): codebase vocabulary
  expected, but team folklore ("the old pipeline", "Dave's service") is not —
  write for the contributor who joined today.

The most common calibration failure is drift *within* a page: a tutorial that
opens gently and casually drops "just configure the reverse proxy" by step 5.
Re-read at the declared level, start to finish.

## Inclusive and global readership

- Plain words over idiom — "straightforward", not "a piece of cake"; readers and
  translators outside your dialect thank you.
- Gender-neutral: "they", or restructure to "you"/plural.
- Examples use placeholder domains (`example.com`), diverse names, and no
  real-person data.
- Don't hinge meaning on color alone ("the red button") — name the label.
