# Contributing to Absolute

Thanks for your interest in contributing! Absolute is a single skill,
`absolute`, that dispatches four commands (`work`, `ui`, `simplify`, `docs`) via a
thin router. Contributions edit a command flow, its deep-dive references, or the
router itself — not standalone skill folders.

## Structure

```
skills/absolute/
  SKILL.md                 # Required - the router (frontmatter + routing rules + command table)
  README.md                # Required - skill landing page
  references/
    work.md, ui.md, simplify.md, docs.md   # one flow per command
    work/, ui/, simplify/, docs/            # each command's deep-dive guides
```

## Editing or adding a command

1. The command's full flow lives in `references/<command>.md`. Edit it there.
2. Deep-dive guides go under `references/<command>/` and load on demand.
3. To add a new command, create `references/<command>.md`, then add a row to the command
   table and the routing rules in `SKILL.md`.

### SKILL.md (router) requirements

- Frontmatter must include: `name`, `version`, `description`, `category`, `tags`, `platforms`, `license`, `maintainers`, `user-invocable`, `argument-hint`
- Description must name each command and list its concrete tasks
- The command table and routing rules must stay in sync with the `references/<command>.md` files that exist
- Total file must be under 500 lines
- All code examples must be syntactically valid

## Updating a command

- Read the `references/<command>.md` flow first
- Bump `version` in `SKILL.md` frontmatter (semver)
- Keep `SKILL.md`, `README.md`, and the command's `references/<command>.md` in sync

## Pull request checklist

- [ ] Changes live under `skills/absolute/`
- [ ] `SKILL.md` is under 500 lines and its command table/routing match the `references/<command>.md` files
- [ ] All `references/` files are under 400 lines
- [ ] `license: MIT` in frontmatter

## Code of conduct

Be respectful, constructive, and focused on making skills useful. We follow the [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct/).
