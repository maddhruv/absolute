# Contributing to Absolute

Thanks for your interest in contributing! Absolute is a collection of eleven separate skills — `absolute-init` (setup), a build loop (`absolute-work`, `absolute-spec`, `absolute-ui`, `absolute-simplify`, `absolute-docs`) and an engineering-health family (`absolute-upgrade`, `absolute-audit`, `absolute-prune`, `absolute-debt`, `absolute-deflake`). Each skill is independent.

## Structure

```
skills/
  absolute-<name>/
    SKILL.md          # Required — frontmatter + full command flow
    README.md         # Required — skill landing page
    references/       # Optional — deep-dive guides loaded on demand
```

The five health skills (`upgrade`, `audit`, `prune`, `debt`, `deflake`) each carry a copy of `references/health-engine.md` — the shared loop. `absolute-work` carries the deep-dive guides (`references/spec-writing.md` etc.) that `absolute-spec` cross-references.

## Editing a skill

1. Edit `skills/absolute-<name>/SKILL.md` for flow changes.
2. Deep-dive guides live in `skills/absolute-<name>/references/` — add or edit there.
3. Bump `version` in the skill's frontmatter (semver).
4. Keep `SKILL.md` and `README.md` in sync.

## Adding a new skill

1. Create `skills/absolute-<name>/SKILL.md` with required frontmatter.
2. Create `skills/absolute-<name>/README.md`.
3. Add an entry to `skills.yaml`.

### SKILL.md frontmatter requirements

- Required fields: `name`, `version`, `description`, `category`, `tags`, `platforms`, `license`, `maintainers`, `user-invocable`, `argument-hint`
- `license`: `MIT`
- `category`: one of — engineering, operations, marketing, ai-ml, design, product, devtools, sales, data, infra, monitoring, cloud, writing, workflow, analytics, game-development, developer-tools, communication, video
- All code examples must be syntactically valid

## Updating health-engine.md

`references/health-engine.md` is copied into each of the five health skill directories. When you change it, update all five copies:

```
skills/absolute-upgrade/references/health-engine.md
skills/absolute-audit/references/health-engine.md
skills/absolute-prune/references/health-engine.md
skills/absolute-debt/references/health-engine.md
skills/absolute-deflake/references/health-engine.md
```

## Pull request checklist

- [ ] Changes live under `skills/absolute-<name>/`
- [ ] `SKILL.md` frontmatter is complete and valid
- [ ] `README.md` is updated if the skill's purpose or usage changed
- [ ] `license: MIT` in frontmatter
- [ ] If editing `health-engine.md`: all five health skill copies updated
- [ ] `skills.yaml` updated if adding a new skill or changing version/description

## Code of conduct

Be respectful, constructive, and focused on making skills useful. We follow the [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct/).
