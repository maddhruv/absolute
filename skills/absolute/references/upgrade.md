# Command: `upgrade` — Dependency Upgrades

> Loaded by the `absolute` router when the user runs `/absolute upgrade …`.
> Start your first response with the ⬆️ emoji.

## Absolute Upgrade

Bring dependencies current — safely, in risk-ranked waves, with tests green after each.
Not a blind `npm update`: outdated and vulnerable deps are grouped by blast radius
(patch/minor → safe wave; major/breaking → gated, one at a time, changelog-read), applied
incrementally, and verified against the project's own test suite.

Runs the shared engine in **`references/health-engine.md`** — read it for the
DETECT → SCAN → TRIAGE → FIX → VERIFY → REPORT loop and the safety contract. This file
covers only what's specific to dependency upgrades.

---

## When to use

- Routine "bring deps up to date" / "upgrade our dependencies".
- A specific bump: "upgrade React to 19", "move off the deprecated X package".
- Clearing `npm outdated` / Dependabot backlog without 40 separate PRs.

**Not** for: adding a *new* dependency (that's a `work`/feature decision), or auditing
*vulnerabilities* specifically → use **`/absolute audit`** (it triages CVEs; `upgrade`
moves versions).

---

## What it scans

Per ecosystem, list outdated deps with current → wanted → latest and the jump type:

| Ecosystem | Detect outdated | Lockfile / manifest |
|---|---|---|
| npm | `npm outdated --json` | `package-lock.json` |
| pnpm | `pnpm outdated --format json` | `pnpm-lock.yaml` |
| yarn | `yarn outdated --json` | `yarn.lock` |
| Python (pip) | `pip list --outdated --format=json` | `requirements*.txt` |
| Python (poetry/uv) | `poetry show --outdated` / `uv pip list --outdated` | `pyproject.toml` + lock |
| Go | `go list -u -m -json all` | `go.mod` / `go.sum` |

Also flag: deps with known deprecations, duplicate/multiple versions of the same package,
and direct vs transitive (only direct deps are upgrade targets; transitives follow).

---

## Risk ranking (TRIAGE)

Group the upgrade plan into waves by semver jump — safest first:

| Wave | Jump | Default |
|---|---|---|
| 1 | patch (`x.y.Z`) | batch together, fix now |
| 2 | minor (`x.Y.z`) | batch by package family, fix now |
| 3 | major (`X.y.z`) / pre-1.0 minor | **one at a time, gated** — read the changelog/migration guide first, list breaking changes |

For every major bump: locate breaking changes (CHANGELOG, release notes, codemod if the
package ships one), inventory call sites that touch the changed API, and state the
migration before applying. Peer-dependency conflicts get resolved in the same wave as
their driver.

---

## Fix & verify

- Apply a wave, regenerate the lockfile, run the project's **full** test + build (a passing
  install is not a passing upgrade).
- Majors: apply the version bump *and* the required code migration in the same wave, or the
  build breaks. Use the package's codemod where one exists.
- A wave that can't go green within reason → revert it, report it as blocked with the error,
  keep the green waves. Never `--force` / `--legacy-peer-deps` to mask a real conflict.

---

## Gotchas

1. **Lockfile-only "upgrade".** Bumping the manifest without regenerating + committing the
   lockfile ships untested transitive versions. Always regenerate.
2. **Batching a major in with patches.** One breaking bump fails the whole wave and hides
   which change broke it. Majors are always solo.
3. **Green install ≠ green project.** `npm install` succeeding proves nothing — run tests.
4. **Pinning around a failure.** If a bump breaks something, fix or defer it; don't pin the
   dependency tree to dodge it silently.

---

## Companion commands

- **`/absolute audit`** — if the goal is fixing *vulnerabilities*, start there; it'll route
  back here for the version moves.
- **`/absolute deflake`** — flaky tests can mask whether an upgrade truly passed.
- **`/absolute work`** — if an upgrade needs real feature-level migration work, hand off.
