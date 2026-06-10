# absolute-documentations

absolute-documentations is a production-ready AI agent skill for claude-code, gemini-cli, openai-codex, and mcp. A Diátaxis-driven documentation lifecycle that writes, improves, and audits public-facing product docs and repo developer docs — in your project's own voice and docs stack, verified against the actual code.

## Quick Facts

| Field | Value |
|-------|-------|
| Category | writing |
| Version | 0.1.0 |
| Platforms | claude-code, gemini-cli, openai-codex, mcp |
| License | MIT |
| References | 7 deep-dive guides |
| Evals | 14 test cases |

## How to Install

1. Make sure you have Node.js installed on your machine.
2. Run the following command in your terminal:

```bash
npx skills add AbsolutelySkilled/AbsolutelySkilled --skill absolute-documentations
```

3. The absolute-documentations skill is now available in your AI coding agent (Claude Code, Gemini CLI, OpenAI Codex, etc.).

## Overview

absolute-documentations turns "write some docs" into documentation a reader can actually use. Every document serves exactly one reader need, classified with the Diátaxis framework: tutorials (lessons), how-to guides (recipes), reference (dictionaries), and explanation (discussions). It covers the full docs surface — product docs site pages plus repo developer docs like READMEs, CONTRIBUTING guides, ARCHITECTURE docs, ADRs, changelogs, and runbooks.

It works codebase-first: detects your docs stack, reads your existing pages for voice and terminology, reads the code being documented, and asks you only what the repo can't answer. It proposes an outline and stops at a hard gate for your approval before writing a word of prose, then self-reviews the draft against a scored rubric before presenting it.

### The 3 modes

1. **WRITE** — new docs through a gated workflow: recon → intake (type, audience, goal, scope) → outline gate → write → self-review
2. **IMPROVE** — classify an existing page's intended quadrant, diff it against that quadrant's standard, verify every stale fact against the code, rewrite
3. **AUDIT** — inventory a whole docs surface, classify every page, flag mixed/misfiled/missing quadrants, and propose a restructure map

### What makes this skill different

- **Diátaxis-strict** — one quadrant per page, enforced. Mixed-purpose pages are the root cause of most bad docs, and the skill refuses to write them.
- **Stack-aware** — detects Fumadocs, Docusaurus, Starlight, MkDocs, VitePress, Mintlify, or plain Markdown, and uses each stack's real frontmatter, admonition, tabs, and nav conventions. Never `:::note` in MkDocs.
- **Accuracy protocol** — code snippets come from your source, not from model memory. Names, defaults, and outputs are read at writing time. Features that don't exist in the code don't get documented; the skill says so instead.
- **One gate, not six** — a single outline-approval gate gives control where changes are cheap, then writes without ceremony.
- **Voice preservation** — calibrates to your project's existing register and terminology instead of bleaching everything into generic doc-speak.

### Reference Guides

| Guide | Coverage |
|-------|----------|
| Tutorials | The learner's contract, structure template, single-path rule, verification by execution |
| How-to Guides | Task-shaped titles, prerequisites-as-state, one goal per guide, verification steps |
| Reference | Entry templates (function/CLI/config/endpoint), neutrality rules, generated-vs-hand-written |
| Explanation | Question-shaped topics, forces and trade-offs, history honesty, scope bounding |
| Developer Docs | README, CONTRIBUTING, ARCHITECTURE, ADR, CHANGELOG, and runbook templates |
| Style and Voice | Voice calibration, sentence/word rules, banned words, callout budgets, audience drift |
| Docs Stacks | Detection markers plus per-stack frontmatter, components, and nav registration |

### Key Principles

1. One quadrant per page — link to other quadrants, never merge into them
2. Codebase before questions — only ask what the repo can't answer
3. Outline before prose — one hard approval gate
4. Code is the source of truth — verify every snippet, name, and default
5. Never document what doesn't exist
6. Detect the stack, then conform to it
7. Write in the project's voice, not generic doc-speak
8. Warnings before the dangerous step, never after

## Tags

`documentation` `diataxis` `technical-writing` `developer-docs` `readme` `docs-site` `tutorials` `api-reference` `changelog`

## Platforms

- claude-code
- gemini-cli
- openai-codex
- mcp

## Related Skills

Pair absolute-documentations with these complementary skills:

- [absolute-work](https://www.absolutelyskilled.pro/skill/absolute-work)
- [absolute-ui](https://www.absolutelyskilled.pro/skill/absolute-ui)

## Frequently Asked Questions

### What is absolute-documentations?

A Diátaxis-driven documentation skill for AI agents. It writes, improves, and audits documentation — product docs (tutorials, how-to guides, reference, explanation) and repo developer docs (README, CONTRIBUTING, ARCHITECTURE, ADRs, changelogs, runbooks) — detecting your docs stack and verifying every claim against your actual code.

### What is Diátaxis?

A framework that splits documentation into four kinds by reader need: tutorials (learning by doing), how-to guides (accomplishing a task), reference (looking up facts), and explanation (understanding a topic). Most documentation problems trace back to one page trying to be two of these at once — the skill classifies first and keeps each page pure.

### Does it work with my docs site generator?

It detects Fumadocs, Docusaurus, Starlight, MkDocs, VitePress, and Mintlify from marker files and follows each one's frontmatter, component, and navigation conventions. With no docs stack present, it writes clean GitHub-flavored Markdown.

### Will it invent APIs or features that don't exist?

No. The accuracy protocol requires every snippet, flag, default, and output to be read from the source code at writing time. If asked to document a feature it can't find in the code, it stops and tells you instead of writing aspirational docs.

### How do I install absolute-documentations?

Run `npx skills add AbsolutelySkilled/AbsolutelySkilled --skill absolute-documentations` in your terminal. The skill will be immediately available in your AI coding agent.

### What AI agents support absolute-documentations?

This skill works with claude-code, gemini-cli, openai-codex, mcp. Install it once and use it across any supported AI coding agent.

## Maintainers

- [@maddhruv](https://github.com/maddhruv)

---

*Generated from [AbsolutelySkilled](https://www.absolutelyskilled.pro/skill/absolute-documentations)*
