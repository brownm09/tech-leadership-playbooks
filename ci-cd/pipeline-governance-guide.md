# CI/CD Pipeline Governance Guide

## Leadership Context

Without pipeline governance, the org cannot answer the questions engineering leadership routinely needs answered: what changed, who approved it, could it have caused this incident? DORA metrics carry executive weight only when the pipeline enforces the gates making them meaningful.

## Purpose

This guide covers the decisions, gates, and ownership determining whether a CI/CD pipeline holds up at scale. Written for engineering orgs running pipelines but lacking formalized rules around who can change them, what gates run, and how to audit what ran.

> **Demonstration sandbox:** [lifting-logbook](https://github.com/brownm09/lifting-logbook)
> is a personal-project monorepo at single-operator scale. The artifacts linked
> in the Further-reading section illustrate the techniques described here; production-scale
> application of the same techniques appears in [ORIGINS.md](../ORIGINS.md) where
> applicable.

## Background and Motivation

Two CI/CD programs anchor the framework:

1. **Capital One (2019–2022):** I led the standardization of CI/CD pipelines across multiple engineering departments, completing the migration two months ahead of schedule. Outcome: 300 engineering hours saved per team.

2. **ActBlue Technical Services (2022–2024):** I chartered the DevEx team, which owned application-level delivery tooling (CI/CD pipelines, the internal developer platform, and deployment scaffolding). The cloud infrastructure layer sat with a separate team outside my direct management.

## Pipeline Ownership Model

Every pipeline needs an assigned owner. Pipelines without owners accumulate configuration drift and update last when security requirements change.

**Ownership responsibilities:**
- Keep pipeline configuration current with platform standards
- Review and approve changes to pipeline steps with elevated permissions (deploy steps, secret access)
- Respond to pipeline failures the on-call rotation does not catch

**Ownership model options:**

| Model | When it works | Watch out for |
|-------|--------------|---------------|
| Application team owns their pipeline | Teams move fast, pipelines match team needs | Inconsistency across teams; security guardrails vary |
| Platform team owns shared pipeline templates; app teams extend | Consistency with flexibility | Template updates can break team pipelines if not versioned |
| Platform team owns all pipelines | Full consistency and central control | Bottleneck; teams cannot iterate quickly |

The shared template model (option 2) holds up where 3+ application teams need consistent gates without a central bottleneck. Platform team defines the required gates; teams configure application-specific steps within those boundaries.

## Required Gates

Non-negotiable gates for any pipeline deploying to production:

**Build stage:**
- [ ] Dependency vulnerability scan (SCA): blocks on critical/high CVEs with a defined exception process
- [ ] Static analysis (SAST): results visible to developers; severity thresholds enforced
- [ ] Unit and integration tests: failure blocks promotion; flaky tests get tracked and fixed, never suppressed
- [ ] Container image scan: if building Docker images, scan before push

**Pre-deploy stage:**
- [ ] Branch protection rules enforced: direct commits to `main`/`production` blocked; changes require PR + review
- [ ] Secrets scanning: no credentials, API keys, or tokens in committed code or pipeline configuration
- [ ] Infrastructure-as-code linting and validation (Terraform `validate`, `plan` output reviewed for destructive changes)

**Deploy stage:**
- [ ] Environment promotion runs sequential: dev → staging → production; no skipping
- [ ] Production deploys require explicit approval (manual gate or automated approval from a defined approver group)
- [ ] Rollback procedure defined and tested before the pipeline ships as production-ready

**Post-deploy:**
- [ ] Smoke tests or synthetic monitoring validate the deployment
- [ ] Deployment logged with: triggering user, artifact, SHA, timestamp
- [ ] Alerting in place to detect regressions within a defined window after deploy

## Secret Management

Secrets in pipeline configuration drive credential exposure incidents at high frequency. The rules:

- Secrets never live in repository code, pipeline YAML, or environment variable configuration in plaintext
- Secrets get injected at runtime from a secrets manager (AWS Secrets Manager, HashiCorp Vault, GitHub Actions secrets with OIDC)
- Service accounts used by pipelines hold the minimum permissions required for the steps they run
- Pipeline credentials rotate on a defined schedule and immediately after team membership changes
- Access to production secrets gets logged with a queryable audit trail

**OIDC over long-lived credentials.** Where the CI platform and cloud provider support it (GitHub Actions + AWS, for example), use OIDC federation to issue short-lived credentials per pipeline run instead of storing long-lived access keys. The rotation problem disappears entirely.

## Branch and Environment Strategy

The pipeline governance model matches the branching strategy. Common patterns:

**Trunk-based development:**
- All work merges to `main` via short-lived feature branches
- `main` stays deployable
- Feature flags control what runs live; branches do not
- Pipeline: every merge to `main` triggers a deploy to staging; production deploy comes as a promotion with approval

**GitFlow (or modified GitFlow):**
- `develop` branch for integration, `release` branches for production candidates
- More pipeline complexity; more opportunities for gates to be bypassed if not enforced at each branch
- Appropriate for teams with long release cycles or strict change management requirements

For modern web platforms, trunk-based holds up as the default. GitFlow adds overhead justified only when release cadence faces external constraints (regulatory approval, coordinated launches).

## Audit and Observability

An unauditable pipeline becomes a liability in an incident or compliance review.

**Minimum audit requirements:**
- Every pipeline run logs: triggering user or event, branch/SHA, environment, start/end time, pass/fail per stage
- Logs retained for at least 90 days (12 months if in scope for PCI-DSS or similar)
- Production deploy approver recorded and queryable
- Pipeline configuration changes version-controlled and reviewed like application code

**Metrics worth tracking:**
- Deployment frequency (per team, per service)
- Change failure rate: percentage of deploys requiring a rollback or hotfix
- Mean time to restore (MTTR) after a failed deploy
- Pipeline duration trends; rising duration signals something needs attention

These four metrics map directly to DORA metrics and answer "how is our delivery health?" with data.

## Change Control for Pipeline Configuration

- Pipeline config lives in version control alongside application code, or in a dedicated platform configuration repo
- Changes to pipeline config require code review, the same as application changes
- Changes adding or modifying steps with elevated permissions (secret access, production deploy) require review from the platform team or a designated approver
- Hotfix procedures for pipeline config get defined in advance, never improvised during an incident

## Common Governance Failures

**Tests passing because failures get suppressed.** `|| true` at the end of a test command, or test results uploaded without enforcement. The gate exists in the config without blocking anything.

**Manual steps undocumented.** Someone SSH'd into a server once to fix a deploy; the step gets assumed as part of the process but lives nowhere in the pipeline. The next person running the deploy discovers it at 11pm.

**Production credentials in staging pipelines.** Staging pipelines use production credentials "temporarily" and the temporary state becomes permanent. Isolate credentials per environment.

**No rollback test.** Rollback procedures defined but never executed since the pipeline was built. Test rollback before you need it.

**Drift between environments.** Staging and production pipelines diverge over time because fixes go to production first and never get backported. The staging pipeline stops being representative. Run the same pipeline config across all environments; parameterize what differs.

---

## Further reading: demonstration artifacts

The artifacts below illustrate the techniques described in this guide against the demonstration sandbox introduced after the Purpose section. See [LINKING.md](../LINKING.md) for the full convention. Citation links pin to commit [`413f8a6`](https://github.com/brownm09/lifting-logbook/tree/413f8a62f43f12fa200be3e3307da7ef72c7b446) per the LINKING.md SHA-pinning rule. Where an artifact evolves alongside the pipeline, a `main` link sits alongside.

### On required gates and gate ordering

- **CI workflow.** Citation: [`.github/workflows/ci.yml` at 413f8a6](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/.github/workflows/ci.yml); live state: [same path on `main`](https://github.com/brownm09/lifting-logbook/blob/main/.github/workflows/ci.yml). Defines the build-stage gates: lint and test under Turborepo, an analytics-taxonomy validator running as a separate enforced step, and a parallel `db-integration` job spinning up Postgres as a service container so DB-touching tests run against a real engine instead of a mock. Demonstrates the guide's claim about gates being meaningful only where they actually block — every step here exits non-zero on failure and no `|| true` suppression exists.
- **Deploy workflow.** Citation: [`.github/workflows/deploy.yml` at 413f8a6](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/.github/workflows/deploy.yml); live state: [same path on `main`](https://github.com/brownm09/lifting-logbook/blob/main/.github/workflows/deploy.yml). Worked example of the sequential `terraform staging → build images → deploy staging → smoke test → manual approval → terraform production → deploy production` pipeline this guide describes. Notable: `terraform plan` output reviewed before apply; production gated behind an explicit GitHub Environment approval; smoke tests run between staging deploy and the production gate.

### On Turborepo task dependencies

- **`turbo.json`.** Citation: [`turbo.json` at 413f8a6](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/turbo.json); live state: [same path on `main`](https://github.com/brownm09/lifting-logbook/blob/main/turbo.json). Encodes the build/test/lint dependency graph and input scoping letting the CI run only the affected packages. The explicit `inputs` arrays for each task carry the part worth reading closely: they define what changes invalidate the cache, where most monorepo CI bugs originate.

### On secret management and OIDC

- **OIDC federation in deploy.yml.** Citation: [`google-github-actions/auth@v2` step at 413f8a6](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/.github/workflows/deploy.yml). Demonstrates the guide's recommendation to prefer short-lived OIDC-issued credentials over long-lived service account keys: the workflow exchanges GitHub's OIDC token for GCP credentials per run via `workload_identity_provider` and `service_account` secrets. No long-lived GCP keys live in repo or in GitHub secrets; only the WIF binding identifiers.

### Gaps relative to the standard

- **Branch protection rules** sit invisible from the repo. Configuration happens via GitHub's API/UI; committed YAML does not carry it. The guide's "branch protection enforced" gate stands as a requirement nonetheless; reading the repo cannot verify it. To verify on a project of your own, query `gh api repos/{owner}/{repo}/branches/{branch}/protection`.
- **No CI-specific ADR exists yet.** The pipeline's design decisions (Turborepo over alternatives, the `terraform plan → manual approval` sequencing, OIDC over keys) remain unrecorded as ADRs in `docs/adr/`, a gap against the standard the rest of the stack upholds.
