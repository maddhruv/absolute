# Absolute

A focused development workflow engine for AI coding agents: one skill, `absolute`, with eleven commands — a one-time setup (`init`), a build loop (`work`, `spec`, `ui`, `simplify`, `docs`) and an engineering-health family (`upgrade`, `audit`, `prune`, `debt`, `deflake`) — dispatched as `/absolute <command> [target]`.

## Project Structure

```
skills/absolute/            # The single skill
  SKILL.md                  # Router: frontmatter + routing rules + command table (under 500 lines)
  README.md                 # Skill landing page (required)
  references/
    init.md                                          # one-time setup: interview + detect → config
    work.md, spec.md, ui.md, simplify.md, docs.md   # build-loop command flows
    upgrade.md, audit.md, prune.md, debt.md, deflake.md   # engineering-health command flows
    health-engine.md          # shared loop for the 5 health commands (not a command)
    work/, ui/, simplify/, docs/            # each command's deep-dive guides (under 400 lines each; spec reuses work/spec-writing.md)
CONTRIBUTING.md             # Full contribution guide
```

## The Skill

- The skill lives in `skills/absolute/` with a `SKILL.md` (the router) and a `README.md`
- Each command's full flow lives in `references/<command>.md`; deep-dive guides are namespaced under `references/<command>/`
- The `SKILL.md` command table and routing rules must stay in sync with the `references/<command>.md` files that exist

## SKILL.md Requirements

- Frontmatter must include: `name`, `version`, `description`, `category`, `tags`, `platforms`, `license`, `maintainers`, `user-invocable`, `argument-hint`
- Valid categories: engineering, operations, marketing, ai-ml, design, product, devtools, sales, data, infra, monitoring, cloud, writing, workflow, analytics, game-development, developer-tools, communication, video
- `license` should be `MIT`
- Description must name each command and list its concrete tasks
- Total file must be under 500 lines
- All code examples must be syntactically valid

## Maintaining the Skill

This is a focused single-skill toolkit — `absolute` with eleven commands (setup: `init`; build loop: `work`, `spec`, `ui`, `simplify`, `docs`; engineering health: `upgrade`, `audit`, `prune`, `debt`, `deflake`), not an open registry. The five health commands share `references/health-engine.md`. When editing a command, keep `SKILL.md` (router), `README.md`, and the command's `references/<command>.md` in sync.

## Package Manager

Use `npm` for this project (not pnpm/yarn/bun).
