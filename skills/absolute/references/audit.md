# Command: `audit` — Vulnerability & Security Scan

> Loaded by the `absolute` router when the user runs `/absolute audit …`.
> Start your first response with the 🔒 emoji.

## Absolute Audit

Find and triage security problems across the repo — vulnerable dependencies (CVEs) and
risky code patterns — then fix the ones worth fixing, safely. Output is a severity-ranked
findings table with a remediation per item, not a raw scanner dump.

Runs the shared engine in **`references/health-engine.md`** — read it for the
DETECT → SCAN → TRIAGE → FIX → VERIFY → REPORT loop and the safety contract. This file
covers only what's specific to security auditing.

> **Authorized defensive use.** This command audits the user's *own* repository to find
> and fix weaknesses. It is for hardening, not for attacking systems or evading detection.

---

## When to use

- "Run a security audit", "are we vulnerable?", "check our deps for CVEs".
- After a CVE disclosure affecting something you use.
- Periodic hygiene on `main`.

Distinct from the built-in **`/security-review`** (reviews the *pending diff* on your
branch) — `audit` scans the **whole committed repo**, deps included. They complement.

---

## What it scans

**1. Dependency vulnerabilities (CVEs)** — primary:

| Ecosystem | Scanner |
|---|---|
| npm / pnpm / yarn | `npm audit --json` / `pnpm audit --json` / `yarn npm audit --json` |
| Python | `pip-audit` (preferred) or `safety check` |
| Go | `govulncheck ./...` |
| Cross-language | `osv-scanner` against the lockfile if available |

**2. Code-level patterns** — read-only grep/static pass for high-signal issues only:
hardcoded secrets/keys/tokens, `eval`/dynamic exec on input, SQL built by string
concatenation, missing authz checks on sensitive routes, disabled TLS verification, unsafe
deserialization, overly-broad CORS. Prefer the project's existing SAST/linter security
rules (`eslint-plugin-security`, `bandit`, `gosec`) if configured.

Report suspected leaked secrets but **never print the secret value** — reference
`path:line` and the kind.

---

## Risk ranking (TRIAGE)

Rank by **severity × exploitability × reachability**, not raw CVSS:

| Severity | Default |
|---|---|
| Critical / High, reachable, fix available | fix now (wave 1) |
| Moderate, reachable | fix this pass |
| Low / not reachable from app code | report, usually defer |
| Transitive-only, no direct upgrade path | flag, note the blocking parent |

Mark each: is it reachable from the app's actual code paths? A CVE in an unused transitive
branch is lower priority than a Moderate one on a hot path. State the fixed version or the
mitigation for each.

---

## Fix & verify

- **Dep CVEs** → resolve via the smallest version move that clears it (delegate the actual
  bump mechanics to the `upgrade` flow's per-ecosystem steps). Prefer patched minors;
  escalate to a major only when that's the only fix, and gate it.
- **Code issues** → apply the concrete fix (parameterize the query, move the secret to env
  + flag the leaked one for rotation, add the authz check). Each fix is its own small wave.
- After each wave, re-run the scanner: the finding must actually disappear, and tests/build
  stay green. Never resolve by suppressing/allowlisting the alert.
- Leaked live secrets: flag for **rotation** — removing from code doesn't undo exposure.

---

## Gotchas

1. **Audit fatigue → blanket ignore.** Triage by reachability instead of muting the scanner.
2. **Fixing a CVE by suppressing it.** An allowlisted advisory is still a vulnerability.
3. **Printing the secret.** Reference location + type only; never echo the value.
4. **Deleting a secret from code ≠ safe.** It's in git history and was exposed — rotate it.
5. **Stopping at deps.** Many real issues are in code, not the dependency tree — run both passes.

---

## Companion commands

- **`/absolute upgrade`** — does the actual version moves for vulnerable deps.
- **`/security-review`** (built-in) — pair with this to also cover your pending diff.
- **`/absolute work`** — if remediation is a real refactor (e.g. replacing an auth flow), hand off.
