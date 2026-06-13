# AbsolutelySkilled

A focused toolkit of production-ready skills for AI coding agents: absolute-work, absolute-ui, absolute-simplify, and absolute-documentations.

## Project Structure

```
skills/<skill-name>/        # Each skill is a self-contained folder
  SKILL.md                  # Core skill content (required, under 500 lines)
  README.md                 # Skill landing page (required)
  evals.json                # Test suite (required, 10-15 evals)
  sources.yaml              # Crawl provenance (required for URL-based skills)
  references/               # Deep-dive files loaded on demand (under 400 lines each)
CONTRIBUTING.md             # Full contribution guide
```

## Skills

- Every skill lives in `skills/<skill-name>/` and must contain both a `SKILL.md` and a `README.md`
- Follow the README pattern from existing skills (e.g., `skills/absolute-work/README.md`): description, install command, overview, tags, platforms, recommended skills, maintainers, and footer link

## SKILL.md Requirements

- Frontmatter must include: `name`, `version`, `description`, `category`, `tags`, `platforms`, `license`, `maintainers`
- Valid categories: engineering, operations, marketing, ai-ml, design, product, devtools, sales, data, infra, monitoring, cloud, writing, workflow, analytics, game-development, developer-tools, communication, video
- `license` should be `MIT`
- Description must name the tool/domain and list 3-5 concrete tasks
- Body follows standard section order (see existing skills for examples)
- Total file must be under 500 lines
- All code examples must be syntactically valid

## evals.json Requirements

Write 10-15 evals covering:
- 2-3 trigger tests
- 4-5 core task tests
- 2-3 gotcha/edge case tests
- 1-2 anti-hallucination tests
- 1 references load test

## Maintaining Skills

This is a focused 4-skill toolkit (absolute-work, absolute-ui, absolute-simplify, absolute-documentations) — not an open registry. When editing a skill, keep its `SKILL.md`, `README.md`, and `evals.json` in sync.

## Package Manager

Use `npm` for this project (not pnpm/yarn/bun).
