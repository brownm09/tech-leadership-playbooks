# AI-Assisted Documentation Standards

**Scope:** How to ensure AI-generated documentation is auditable, verifiable, and trustworthy
**Applies to:** Any project where Claude Code or another AI assistant contributes to architectural
documentation, design documents, ADRs, playbooks, or technical recommendations

---

## Background and Motivation

I developed these standards during the Claude Code and Claude integration initiative at Community Tech Alliance (2025–2026). I designed the phased rollout plan with separate use cases for engineers and managers and developed a structured evaluation rubric for measuring impact. The auditability requirements here came out of one observation: AI-generated documentation lacks the provenance chain human-authored documentation carries by default, and in regulated environments or portfolio-quality work the gap has to be closed explicitly.

> **Demonstration sandbox:** [lifting-logbook](https://github.com/brownm09/lifting-logbook)
> is a personal-project monorepo, not a production system at scale. The artifacts linked
> in the Further-reading section illustrate the citation, attribution, and authoring
> discipline described here against a working AI-assisted codebase; production-scale
> application of the same standards is documented in [ORIGINS.md](../ORIGINS.md) where
> applicable.

---

## Core Principle: AI Recommendations Must Be Sourced

When an AI assistant produces documentation naming a technology, pattern, protocol, or
compliance framework as a recommendation, the recommendation has to trace to a primary
source. Unsourced AI output cannot be verified: a reader has no way to distinguish a
well-grounded recommendation from a confident-sounding hallucination.

Hallucination prevention is the secondary concern. The primary concern is intellectual
integrity — the same standard a staff engineer meets when writing a design document also
binds AI-generated output carrying the same weight.

---

## What Must Be Cited

### Technology and framework choices
Link to the official documentation for every tool selected. Official docs carry the
authoritative claims on capability, configuration, and limitations.

**Examples:**
- Choosing NestJS → link to [docs.nestjs.com](https://docs.nestjs.com)
- Choosing Prisma → link to [prisma.io/docs](https://www.prisma.io/docs/getting-started)
- Choosing Turborepo → link to [turbo.build/repo/docs](https://turbo.build/repo/docs)

### Protocol and specification choices
Link to the normative specification for every protocol or standard referenced.

**Examples:**
- OAuth 2.0 → [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)
- PKCE → [RFC 7636](https://datatracker.ietf.org/doc/html/rfc7636)
- GraphQL → [spec.graphql.org](https://spec.graphql.org/October2021/)
- OIDC → [openid.net/specs/openid-connect-core-1_0.html](https://openid.net/specs/openid-connect-core-1_0.html)

### Architectural patterns
Link to the original paper, article, or book chapter introducing the pattern or treating
it canonically. Do not cite secondary summaries or Wikipedia.

**Examples:**
- Hexagonal Architecture → [Cockburn (2005)](https://alistair.cockburn.us/hexagonal-architecture/)
- Clean Architecture / Dependency Rule → [Uncle Bob (2012)](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- Dependency Injection → [Fowler (2004)](https://martinfowler.com/articles/injection.html)

### Compliance and regulatory references
Link to the primary regulatory or standards body source. Do not cite compliance vendor
summaries or blog posts as the authoritative source.

**Examples:**
- GDPR Article 17 → [EUR-Lex — GDPR (Regulation 2016/679), Article 17](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32016R0679)
- HIPAA Security Rule → [hhs.gov/hipaa/for-professionals/security](https://www.hhs.gov/hipaa/for-professionals/security/index.html)

---

## Where Citations Go

### In ADRs
A `## References` section at the bottom of every ADR. Each entry: a linked title, and
one sentence describing what decision in the ADR it supports.

```markdown
## References

- [RFC 6749 — OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc6749) — The authorisation
  code flow used by Clerk and Auth0; defines the token exchange this ADR relies on.
- [Clerk — Documentation](https://clerk.com/docs) — Official docs for the primary identity
  provider chosen in the Decision section.
```

### In playbooks
A `## References` section at the bottom of any playbook citing a specific framework,
methodology, or standard. Playbooks covering internal process guidance with no external
dependencies do not require citations.

### In AI responses
When Claude recommends a technology in a response (in addition to committed documentation),
it should include the official documentation link inline. A reader verifies the
recommendation in one click; no follow-up search needed.

---

## What Does Not Require a Citation

- Internal cross-references to other ADRs, issues, or PRs in the same project
- Decisions that are entirely internal (e.g., a naming convention with no external basis)
- Widely understood facts that do not assert specific capability claims

---

## Consolidated Reference Index

For projects with multiple ADRs, maintain a consolidated reference index (e.g.,
`docs/adr-references.md`) grouping all sources by domain. The index serves as:
- A single place to audit coverage ("are all our technology choices sourced?")
- A reading list for new contributors onboarding to the architecture
- A starting point for due-diligence reviews (security, ARB, compliance audits)

Update the index whenever an ADR's references change.

---

## Enforcing This Standard in Prompts

When opening a session in which documentation will be written or updated, include this in
the opening brief:

> All ADRs and design documents produced this session must include a ## References section
> with links to primary sources for every technology, pattern, and specification cited.
> See docs/adr-references.md for the established pattern.

For projects using CLAUDE.md, encode this as a standing instruction in the
**Documentation and Citations** section so it applies automatically every session.

---

## Further reading: demonstration artifacts

The artifacts below illustrate the citation, attribution, and authoring discipline described in this playbook against the demonstration sandbox introduced after the Background section. See [LINKING.md](../LINKING.md) for the full convention. Citation links pin to commit [`413f8a6`](https://github.com/brownm09/lifting-logbook/tree/413f8a62f43f12fa200be3e3307da7ef72c7b446) per the LINKING.md SHA-pinning rule. Where an artifact is intended to evolve as the codebase does, a `main` link is provided alongside.

### On `## References` sections in ADRs

- **ADR series with consistent References sections** — live state: [`docs/adr/`](https://github.com/brownm09/lifting-logbook/tree/main/docs/adr). Every ADR ends with a `## References` section linking to primary sources — official documentation, IETF RFCs, foundational books, and standards body specifications. The pattern is uniform across the series, which is the playbook's argument: citation hygiene must be a *convention*, not a per-document judgment call, or it decays.
- **Worked example: ADR-018 (Observability Stack)** — citation: [`docs/adr/ADR-018-observability-stack.md` at 413f8a6](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/docs/adr/ADR-018-observability-stack.md). Cites OpenTelemetry specifications, Grafana Cloud product documentation, and the Honeycomb / Datadog vendor pages it explicitly considered against. Demonstrates the playbook's "technology choices link to official documentation" rule applied at a decision the ADR is making *right now*.
- **Worked example: ADR-019 (SLO Methodology)** — citation: [`docs/adr/ADR-019-slo-methodology.md` at 413f8a6](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/docs/adr/ADR-019-slo-methodology.md). Cites the Google SRE Workbook chapter on burn-rate alerting as the methodological primary source. Demonstrates the playbook's "architectural patterns link to the original treatment, not a secondary summary" rule.

### On AI provenance and attribution

- **Co-authorship convention in commit history** — live state: [`git log` on `main`](https://github.com/brownm09/lifting-logbook/commits/main). Commits authored with AI assistance carry a `Co-Authored-By: Claude` trailer. Demonstrates the playbook's principle that AI-assisted authorship needs the same provenance chain as human authorship — the trailer is grep-able and survives squash-merge, so the contribution chain is reconstructable from history alone.
- **Proposal scaffolding configuration** — live state: [`.claude/propose.json`](https://github.com/brownm09/lifting-logbook/blob/main/.claude/propose.json). Encodes the project's proposal taxonomy (milestones, epics, GitHub Project metadata) so the `/propose` skill produces structurally consistent proposal documents. Demonstrates the playbook's "encode as a standing instruction" recommendation: the configuration is read automatically every time the skill runs, eliminating per-session prompt drift.
- **Project CLAUDE.md as the standing instruction surface** — live state: [`CLAUDE.md`](https://github.com/brownm09/lifting-logbook/blob/main/CLAUDE.md). Codifies the project-level documentation conventions Claude must follow on every session, including the citation requirements this playbook establishes.

### What this sandbox does *not* demonstrate

- **Compliance and regulatory citations** are absent because the project has no compliance posture — there is no GDPR / HIPAA / PCI footprint to require citations to primary regulatory sources. The technology, specification, and pattern citation discipline transfers; the regulatory citation discipline gets exercised only in scopes where the rules bind.
- **A consolidated reference index** (the `docs/adr-references.md` artifact recommended above) is not yet maintained. References live per-ADR; aggregating them across the series is a recognized gap.
