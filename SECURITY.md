# Security Policy — transcribator-web

This repository ships within the Arcanada ecosystem. Vulnerabilities are
triaged under the Arcanada Ecosystem Security Policy Mandate.

## Reporting

Preferred channel for public repositories: **GitHub Private Vulnerability
Reporting** (`Security` tab -> `Report a vulnerability`).

Alternative channel: **security@arcanada.ai** with subject prefix
`[security]`. Encrypt with the PGP key published on
[keys.openpgp.org](https://keys.openpgp.org) when disclosing exploit
details.

Please do not file public issues for security findings. Reports include:

1. Affected file(s) and version (commit reference if known).
2. Category (e.g. injection, credential exposure, supply-chain
   compromise, workflow bypass).
3. Reproduction — minimal example showing the attack path.
4. Impact assessment — what an attacker gains and at what scope.
5. Suggested fix (optional but appreciated).

## Disclosure SLA

| Stage | Target |
|-------|--------|
| Acknowledgement of report | <= 72 hours |
| Triage and severity assignment | <= 7 days |
| Fix for HIGH / CRITICAL | <= 90 days |
| Fix for MEDIUM | <= 180 days |
| Fix for LOW | best-effort, batched into next minor |
| Coordinated public disclosure | within 14 days of fix, or 120 days after report (whichever sooner), unless embargo is mutually agreed |

If the reporter does not hear back within the acknowledgement window,
they may publicly disclose without further coordination.

## Supported Versions

This repository ships no released code yet (see § Scope). When the first
release is cut, this table lists the supported version line.

## CI Gate Floor

This repository currently holds no code and runs no CI (see § Scope). The
ecosystem security-audit gate applies from the first code commit; the intended
consumer wiring is recorded below so it is not forgotten at that point.

```yaml
# .github/workflows/ci.yml (consumer side)
jobs:
  security-audit:
    uses: Arcanada-one/datarim/.github/workflows/reusable-security-audit.yml@main
    with:
      stack: framework
      audit_level: high
      accepted_risk_path: accepted-risk.yml
```

Once wired, the reusable workflow enforces:

1. `SECURITY.md` presence at repo root (fail-closed).
2. `accepted-risk.yml` schema validation when present.
3. Stack-specific dependency audit (advisory database, license check).
4. Cross-check of audit findings against the accepted-risk register —
   unsuppressed and stale-suppressed findings fail the job.

## Accepted Risks

Machine-readable source-of-truth: `accepted-risk.yml` at repo root. That
register is **not present in this repository yet**; the table below is the
rendered projection and is empty until it is added.

| Advisory ID | Package | Severity | Scope | Last review | Re-review | Reviewed by | Reason |
|-------------|---------|----------|-------|-------------|-----------|-------------|--------|
| _(no entries)_ | | | | | | | |

Re-review dates MUST satisfy `re_review <= last_review + 90 days`.
Entries past `re_review` raise an ecosystem-wide stale-trigger event
(`warn` at zero days overdue, `fatal` at thirty days overdue).

## Hardening Baseline

Verified in place for this repository:

- Branch protection on the default branch, with force-push and branch
  deletion disabled.

Ecosystem baseline this repository works toward (defined by the Arcanada
Ecosystem Security Policy Mandate — listed as the target, not as a claim that
each control is already active here):

- Static analysis on shell, configuration, and CI manifests
  (`shellcheck`, `actionlint`, `zizmor`).
- Stack-specific lint, type-check, and test gates in `.github/workflows/`.
- Secret scanning on every push (`gitleaks`).
- Dependency vulnerability scanning per stack profile, via the reusable
  security-audit workflow.
- Required reviews and required status checks on the default branch.
- `CODEOWNERS` routing for `SECURITY.md`, `accepted-risk.yml`, and
  `.github/workflows/security*.yml`.

## Standards Mapping

The baseline maps to OWASP ASVS v5, OpenSSF Scorecard, SOC 2 CC,
ISO 27001 Annex A, and CIS Controls v8. Detailed mapping is available
in repository documentation when published.

## Embargo Policy

For pre-disclosure embargoes (e.g. downstream consumers needing time
to patch before public disclosure), email **security@arcanada.ai**
with proposed embargo window. Default embargo length is 30 days from
coordinated patch release.

## Hall of Fame

Researchers who responsibly disclose vulnerabilities will be credited
in release notes unless they prefer to remain anonymous.

## Scope

This repository is a **reserved placeholder**. It currently contains only a
README, a LICENSE, and repository metadata — no application code, no build,
no deployed artefact.

It carries a security policy anyway so that anyone who finds an issue in the
Arcanada organisation has a valid reporting channel here, and so the policy is
already in place when code lands. Do not read the presence of this file as a
claim that a reviewed codebase exists here.

In scope:

- Repository metadata and any GitHub Actions workflows added under
  `.github/workflows/`.
- All code shipped here once development begins.

Out of scope:

- Other ecosystem services — report to that service's own `SECURITY.md`.
- Findings that require an attacker to already have full root access on the
  host.
