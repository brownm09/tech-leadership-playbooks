# Build vs. Buy Framework

## Leadership Context

Build-vs-buy decisions compound across years. Individual decisions are reversible; the pattern over time determines what the engineering organization becomes. An org that builds everything accumulates custom systems requiring specialized knowledge and ongoing maintenance, pulling capacity away from differentiated work. An org that buys everything accumulates vendor dependencies that constrain architecture, create data portability risk, and erode the engineering team's ability to solve novel problems.

The failure mode is rarely choosing wrong; it is choosing without a framework — following instinct, optimizing for the wrong dimension (usually initial cost), or letting the decision get made by whoever drives the conversation in the architecture meeting. This playbook provides a structured, repeatable approach to build-vs-buy decisions that produces a documented rationale, surfaces the trade-offs explicitly, and creates an audit trail for post-decision review.

## Background and Motivation

This framework was developed from two high-stakes build-vs-buy decisions at ActBlue Technical Services:

1. **Heroku → Kubernetes on AWS (2022–2024):** I identified the migration priority from incident data, built the business case, and cleared the organizational path. The DevEx team handled application-level delivery; the cloud platform team owned the infrastructure layer. Outcome: $64K/year infrastructure savings and simplified incident management.

2. **Payment processor migration to Stripe (2022–2025):** I led the program and defined scope and sequencing. My team executed the migration. Outcome: 2,600+ entity accounts migrated; payments codebase shrunk by 7,000+ lines.

> **Demonstration sandbox:** [lifting-logbook](https://github.com/brownm09/lifting-logbook)
> is a personal-project monorepo, not a production system at scale. The single sandbox
> citation in this playbook (footnote at Dimension 5) illustrates one worked decision
> against the framework; production-scale application is documented in
> [ORIGINS.md](../ORIGINS.md) where applicable. See [LINKING.md](../LINKING.md) for the
> full convention.

---

## When to Use This

| Trigger | Notes |
|---------|-------|
| Evaluating a new tool or platform category | Before any vendor demo or POC is scheduled |
| Vendor contract renewal (>$50K annually) | Renewal is an implicit re-commit to buy; evaluate it explicitly |
| Platform consolidation after acquisition | Two tools doing the same job; need a principled basis for which survives |
| New engineering capability entering the org (observability, feature flags, data pipeline) | First-time adoption decisions set precedents that are hard to reverse |
| Engineering leader taking over a vendor relationship | Inherited contracts deserve a fresh evaluation against current strategy |

Do not use this framework for commodity tooling where the decision is obvious (e.g., "should we use AWS S3 for object storage" — buy). Use it for decisions where the answer is genuinely ambiguous, the cost is material, or the choice will constrain the engineering organization for 2+ years.

---

## The Three-Way Decision

This framework evaluates three options, not two. "Build vs. buy" is a false binary for most decisions.

**Build:** The engineering org designs, builds, and operates the capability internally. The org owns the code, the data, the roadmap, and the operational burden.

**Buy:** Procure a commercial product or SaaS platform. The vendor owns the code and the roadmap; the org owns the integration and the configuration.

**Partner:** Adopt an open-source solution with optional commercial support, or form a deep integration partnership with a vendor where the org contributes to the roadmap. Falls between build and buy — the org takes on more ownership than pure SaaS but less than full build.

Most engineering orgs default to a buy-or-build framing and miss the partner option, which is often optimal for infrastructure-category decisions (databases, message brokers, observability agents) where the open-source ecosystem is mature and the commercial-support tier addresses the operational risk.

---

## The Five Decision Dimensions

Evaluate every build-vs-buy decision on these five dimensions. Each dimension is scored 1–5 in the scoring matrix below. Score independently before combining — do not collapse the dimensions into a single judgment before seeing all five.

### Dimension 1: Strategic Differentiation

Does the capability in question constitute a source of competitive advantage? Would differentiated implementation of this capability meaningfully improve the business's competitive position?

| Score | Description |
|-------|-------------|
| 5 | This capability is core to the product's value proposition; differentiated implementation is a competitive moat |
| 4 | Differentiated implementation would provide meaningful advantage; competitors with better implementations are winning |
| 3 | Neutral — competitors do not differentiate on this; it is a table-stakes capability |
| 2 | Below the threshold of differentiation; customers do not notice or care about the implementation |
| 1 | Commodity infrastructure; no customer or competitive relevance to the implementation |

**Score 4–5 → Strong build signal.** Buying a capability that is core to your differentiation means your roadmap is owned by a vendor. **Score 1–2 → Strong buy or partner signal.** Building commodity capabilities diverts engineering capacity from differentiated work.

### Dimension 2: Total Cost of Ownership (5-Year)

Calculate the full cost of each option across a 5-year horizon. Do not compare initial cost — compare total cost.

**Build cost components:**
- Engineering time to design and build (months × engineer cost)
- Ongoing engineering time to maintain, extend, and operate (annual FTE fraction × 5 years)
- Infrastructure costs (compute, storage, networking)
- Incident response load: estimated hours per year × engineer cost
- Opportunity cost: what does the team not build while building this?

**Buy cost components:**
- License or SaaS fees (annual × 5, accounting for seat expansion as the org grows)
- Integration engineering time (one-time)
- Ongoing integration maintenance (annual FTE fraction × 5 years)
- Migration cost if you switch vendors at year 3 (probability-weighted)

**Partner cost components:**
- Open-source setup and configuration (one-time)
- Commercial support tier if applicable (annual × 5)
- Engineering time to contribute to or customize the project
- Operational burden (similar to build, but reduced by the community maintaining the core)

| Score | Description |
|-------|-------------|
| 5 | Build is significantly cheaper at 5 years (>40% cost advantage) |
| 4 | Build is modestly cheaper (20–40% cost advantage) |
| 3 | Costs are comparable within modeling uncertainty |
| 2 | Buy is modestly cheaper (20–40% cost advantage) |
| 1 | Buy is significantly cheaper (>40% cost advantage) |

**Common mistake:** Underestimating the operational cost of the build option. A custom-built system always requires ongoing engineering attention: dependency updates, scaling, incident response, onboarding new engineers. At a fully-loaded engineer cost of $250K/year, even 0.25 FTE of ongoing maintenance is $62.5K/year — $312K over 5 years, before the initial build cost.

### Dimension 3: Integration Complexity

How difficult is it to integrate the buy/partner option with existing systems? And how difficult would the build option be to integrate with downstream consumers?

| Score | Description |
|-------|-------------|
| 5 | Buy option is deeply embedded in existing vendor ecosystem; integration is trivial or pre-built |
| 4 | Integration is straightforward; standard APIs, good documentation, low surface area |
| 3 | Moderate integration complexity; some custom work required but well-understood patterns |
| 2 | High integration complexity; vendor APIs are poor, documentation is sparse, significant custom glue code required |
| 1 | Integration complexity is severe; vendor requires proprietary data formats, deep coupling, or migration from current systems is extremely high-risk |

**Note:** Score the buy/partner integration complexity on this dimension. The build option implicitly scores 5 on integration complexity since you own both sides — but factor in the design and API definition cost in the TCO dimension.

### Dimension 4: Team Velocity Impact

How does this decision affect the engineering team's ability to deliver over the next 12–18 months?

| Score | Description |
|-------|-------------|
| 5 | Building this capability accelerates the team — engineers will learn from it and the internal API enables downstream features faster than any buy option |
| 4 | Build has moderate velocity benefit; the team builds genuine capability in this domain |
| 3 | Velocity impact is neutral; build and buy options result in similar delivery speed |
| 2 | Building this absorbs significant capacity; team velocity on other work drops noticeably during the build period |
| 1 | Building this is a significant multi-quarter distraction; the org will be meaningfully slower on other work for 6–18 months |

**The time dimension matters here.** A build decision that scores 5 in the long run may score 1 for the first 12 months while the team is building. If the org is in a competitive sprint or has committed to delivery milestones, the short-term velocity cost may override the long-term capability argument.

### Dimension 5: Vendor Risk and Lock-In

For buy and partner options: what is the risk profile of the vendor relationship, and how difficult would migration be if the relationship ends?

| Score | Description |
|-------|-------------|
| 5 | Vendor is financially stable, market leader; data is portable; migration to alternative is straightforward |
| 4 | Vendor is established; some lock-in but manageable; competitive alternatives exist |
| 3 | Moderate lock-in; migration would require significant engineering effort but is not catastrophic |
| 2 | High lock-in; vendor-proprietary data formats or APIs; migration would be a major project |
| 1 | Severe lock-in; vendor is a single point of failure; migration would threaten business continuity; vendor financial stability is uncertain |

**For build option:** Score this dimension based on the risk of depending on a key internal contributor — if one engineer owns the system, the lock-in risk is high. Score 5 if the system is well-documented, tested, and operationally understood by multiple engineers.

For a worked example of an alternatives-considered analysis structured around exactly these dimensions — vendor lock-in tolerance, traffic-shape fit, and 5-year TCO — see the observability-stack ADR in the demonstration sandbox.[^bvb-sandbox-1]

---

## Scoring Matrix

**For each dimension, score 1–5. Apply weights based on org type.**

| Dimension | Raw Score (1–5) | Weight: Startup | Weight: Growth-Stage | Weight: Enterprise |
|-----------|----------------|-----------------|---------------------|--------------------|
| Strategic differentiation | | 30% | 25% | 20% |
| Total cost of ownership (5-year) | | 15% | 20% | 25% |
| Integration complexity | | 15% | 15% | 20% |
| Team velocity impact | | 25% | 20% | 15% |
| Vendor risk / lock-in | | 15% | 20% | 20% |
| **Weighted total** | | /5 | /5 | /5 |

**Score interpretation:**

- **4.0–5.0 → Strong build signal**
- **3.0–3.9 → Build or partner; deeper analysis required on the deciding dimensions**
- **2.0–2.9 → Buy or partner; deeper analysis required**
- **1.0–1.9 → Strong buy signal**

The score is an input to the decision, not the decision. Any dimension scoring 1 or 5 should be examined independently of the weighted total — an extreme on a single dimension can override a moderate composite score. If vendor lock-in scores 1 (catastrophic risk) but the composite is 3.0, the lock-in risk warrants an explicit conversation before proceeding.

---

## Build Signals vs. Buy Signals vs. Partner Signals

### Build Signals

- The capability is a direct input to the product's value proposition (a recommendation engine for a recommendations company; a search relevance algorithm for a search company)
- Existing off-the-shelf options have fundamental constraints that cannot be worked around without building a wrapper that is itself a significant engineering project
- The team needs to develop institutional expertise in this domain, and building is the learning vehicle
- The org is at enterprise scale with the engineering capacity to build and operate without the build being a significant capacity drain

### Buy Signals

- The category is mature, commoditized, and not a source of competitive differentiation (authentication, payment processing, email delivery)
- Time-to-market pressure makes the build timeline untenable
- The buy option's feature set exceeds what the engineering org would build in the next 2 years
- The org is at early stage and the founding team's time is the scarcest resource — building infrastructure competes directly with building product

### Partner Signals

- The capability is infrastructure-adjacent: databases, message brokers, observability agents, identity providers
- A well-maintained open-source project exists that covers 80–90% of the use case; the remaining 10–20% can be covered by configuration or thin wrappers
- The commercial support tier of the open-source project provides the operational safety net the org needs without the full SaaS lock-in
- The team wants to maintain technical fluency in the domain without fully owning the software lifecycle

---

## Common Cognitive Biases in Build-vs-Buy Decisions

**Not-Invented-Here (NIH) Syndrome**

Engineers overvalue building because building is more interesting than integrating, and because there is a cultural tendency to distrust external solutions. The tell: "we could build something better" is offered without a concrete analysis of what "better" means or what it costs.

*Counter:* Require TCO analysis before any build proposal is considered. Make the engineer proposing the build estimate the 5-year cost including their own ongoing maintenance time.

**Sunk Cost Bias on Existing Vendors**

An org that has paid $400K over three years to a vendor that is no longer the right solution will often renew because "we have already invested so much in the integration." The sunk cost is gone regardless of the renewal decision. The decision is: is this vendor the right choice for the next 3 years, starting from the current state?

*Counter:* Evaluate all vendor renewals on the same five dimensions as a new decision. The integration already existing should factor into the TCO analysis as a credit (no one-time integration cost) but should not override a score that points to switching.

**Underestimating Integration Cost**

Engineering teams consistently underestimate the cost of integrating a buy option. "We'll have it running in 2 weeks" routinely becomes 3 months when authentication, data model mapping, error handling, monitoring, and edge cases are fully accounted for.

*Counter:* Require a written integration plan before any buy decision is finalized. The plan should identify: authentication mechanism, data model differences, error states, monitoring approach, rollback plan. If the team cannot produce this in a few hours, they do not understand the integration well enough to estimate it.

**Optimizing for Demo Quality**

Vendors are expert at making their products look good in a 45-minute demo. Decisions made on demo quality are decisions made on the scenario the vendor controls. The demo shows features; it does not show operational reality, scaling behavior, or the support queue backlog.

*Counter:* Run a proof-of-concept against your actual data and actual integration requirements before any buy decision over $50K/year. Require a reference call with an org at similar scale.

**Anchoring on Initial Cost**

The buy option costs $30K/year and the build option looks like it costs nothing (we will use existing engineers). This comparison ignores the 5-year TCO of both options and the opportunity cost of the engineering time.

*Counter:* The first line of the TCO analysis for the build option is: what is the cost of the engineers doing this work, in terms of what they are not working on?

---

## Decision Log Template

Document the decision before it is made. The decision log forces the team to make the inputs explicit and creates an audit trail for the 12-month post-decision review.

```
## Build-vs-Buy Decision Log

**Decision:** [Describe the capability or tool being evaluated]
**Decision date:** [Date]
**Decision owner:** [Name, title]
**Participants:** [Names of engineers and stakeholders who contributed]

---

### Options Evaluated

1. Build: [brief description]
2. Buy: [vendor name and product]
3. Partner: [open-source project or hybrid approach]

---

### Scoring

| Dimension | Build | Buy | Partner |
|-----------|-------|-----|---------|
| Strategic differentiation (raw) | | | |
| TCO 5-year (raw) | | | |
| Integration complexity (raw) | | | |
| Team velocity impact (raw) | | | |
| Vendor risk / lock-in (raw) | | | |
| **Weighted composite** | | | |

Org type applied: [Startup / Growth-stage / Enterprise]
Weights applied: [list weights used]

---

### TCO Summary (5-year)

| Cost component | Build | Buy | Partner |
|----------------|-------|-----|---------|
| Initial build / integration | | | |
| Ongoing maintenance (annual × 5) | | | |
| License / vendor fees (annual × 5) | | | |
| Infrastructure | | | |
| Estimated migration cost (probability-weighted) | N/A | | |
| **Total 5-year** | | | |

---

### Decision

**Chosen option:** [Build / Buy / Partner]
**Rationale:** [3–5 sentences. Name the deciding dimensions. Acknowledge the trade-offs.
Do not restate the scores — explain why those scores led to this decision given the
org's current situation.]

**Assumptions that must hold for this decision to remain correct:**
- [Assumption 1: e.g., "We remain at <100 engineers; at 200+, the build option becomes worth revisiting"]
- [Assumption 2: e.g., "The vendor maintains its current pricing tier"]
- [Assumption 3]

**Conditions that would trigger re-evaluation:**
- [Condition 1: e.g., "Vendor raises price by >50%"]
- [Condition 2: e.g., "Team velocity on feature delivery drops below X due to maintenance burden"]
- [Condition 3]

**12-month review date:** [Date]
```

---

## Post-Decision Review Checklist

At 12 months after the decision, review the decision log against what happened. The exercise is for calibration, not blame; it improves future decisions.

- [ ] TCO: How do actual costs compare to the 5-year projection at month 12 (annualized)?
- [ ] Integration complexity: Did the integration take as long and cost as much as projected?
- [ ] Team velocity: What was the actual velocity impact over the first 12 months?
- [ ] Vendor risk: Have any of the vendor risk assumptions changed?
- [ ] Assumptions: Are the assumptions in the decision log still valid?
- [ ] Conditions: Have any re-evaluation triggers been hit?
- [ ] Regret score: If the team could make this decision again with 12 months of information, would they make the same choice?

If the regret score is high: document what data would have changed the decision and update the framework's decision criteria. Build institutional learning from individual decisions.

---

## Worked Examples

### Example 1: Feature Flag System

**Context:** A 60-engineer growth-stage SaaS company. Current state: feature flags are managed through environment variables in Terraform. The process is manual, requires a deploy, and is not accessible to non-engineers. Product managers want self-service flag management.

**Options evaluated:**
1. Build: custom feature flag service, flag evaluation SDK, admin UI
2. Buy: LaunchDarkly at $30K/year for 60 seats
3. Partner: Unleash (open-source) with self-hosted deployment

**Scoring (growth-stage weights):**

| Dimension | Build | Buy (LaunchDarkly) | Partner (Unleash) |
|-----------|-------|--------------------|-------------------|
| Strategic differentiation (25%) | 2 | 1 | 1 |
| TCO 5-year (20%) | 3 | 3 | 4 |
| Integration complexity (15%) | 5 | 4 | 3 |
| Team velocity impact (20%) | 1 | 4 | 3 |
| Vendor risk / lock-in (20%) | 5 | 3 | 4 |
| **Weighted composite** | **2.9** | **3.0** | **3.0** |

**TCO analysis:**
- Build: 4 engineer-months initial ($133K) + 0.1 FTE maintenance ($25K/year × 5 = $125K) = $258K
- Buy: $30K/year × 5 = $150K + 2 weeks integration ($16K) = $166K
- Partner: 3 weeks setup ($25K) + hosting ($5K/year × 5 = $25K) + 0.05 FTE maintenance ($12.5K/year × 5 = $62.5K) = $112.5K

**Decision:** Buy (LaunchDarkly). The composite scores are nearly identical, but team velocity impact tips the decision — the 4-month build timeline would delay two product features the team has committed to in H1. At growth stage, delivery commitments take precedence over infrastructure optimization. The 12-month assumption: if the team exceeds 150 engineers and LaunchDarkly pricing increases proportionally, re-evaluate Partner (Unleash) at renewal.

---

### Example 2: Data Pipeline

**Context:** A 150-engineer fintech. Current state: data pipeline is a tangle of Python scripts scheduled in cron, with no lineage, no alerting, and manual recovery when jobs fail. Data engineering team is 6 people. Analytics team is 8 people and blocked on data quality.

**Options evaluated:**
1. Build: internal orchestration layer on top of Airflow
2. Buy: dbt Cloud + Airbyte Cloud at $80K/year combined
3. Partner: Airflow (Apache) self-hosted + dbt Core (open-source) + custom orchestration

**Scoring (growth-stage weights):**

| Dimension | Build | Buy (dbt Cloud + Airbyte) | Partner (Airflow + dbt Core) |
|-----------|-------|--------------------------|------------------------------|
| Strategic differentiation (25%) | 2 | 1 | 1 |
| TCO 5-year (20%) | 3 | 2 | 4 |
| Integration complexity (15%) | 3 | 4 | 3 |
| Team velocity impact (20%) | 2 | 4 | 3 |
| Vendor risk / lock-in (20%) | 4 | 2 | 4 |
| **Weighted composite** | **2.7** | **2.6** | **3.0** |

**Key factor:** Vendor risk score of 2 for the buy option reflects that both dbt Cloud and Airbyte Cloud are storing and processing sensitive financial data; the combination creates data residency risk the compliance team cannot accept. This dimension score alone (1 or 2) triggers the "examine independent of composite score" rule.

**Decision:** Partner (Airflow + dbt Core self-hosted). The compliance constraint overrides the velocity advantage of the buy option. The data engineering team takes on operational burden for Airflow, which is well-understood at this scale, and uses dbt Core for transformation with internal orchestration. 12-month review: assess whether the operational burden was correctly estimated and whether the compliance constraint has changed.

---

### Example 3: Auth Provider

**Context:** An 8-engineer startup. Current state: home-grown authentication built in week 2 of the company. Handles email/password; no MFA, no SSO, no audit logging. Enterprise prospects are asking for SAML SSO as a deal requirement.

**Options evaluated:**
1. Build: extend current auth system to add MFA and SAML SSO
2. Buy: Auth0 at $23K/year for the required tier
3. Partner: Keycloak (open-source) self-hosted

**Scoring (startup weights):**

| Dimension | Build | Buy (Auth0) | Partner (Keycloak) |
|-----------|-------|-------------|--------------------|
| Strategic differentiation (30%) | 1 | 1 | 1 |
| TCO 5-year (15%) | 4 | 3 | 4 |
| Integration complexity (15%) | 3 | 4 | 2 |
| Team velocity impact (25%) | 1 | 4 | 2 |
| Vendor risk / lock-in (15%) | 3 | 3 | 4 |
| **Weighted composite** | **2.0** | **3.0** | **2.5** |

**Key factor:** Authentication is security-critical, and an 8-engineer team that builds and maintains auth is carrying operational risk that Auth0 has already solved at scale. The team velocity impact score of 1 for the build option reflects that adding MFA, SAML, and audit logging to a home-grown auth system is a 2–3 month project that the founding team cannot afford.

**Decision:** Buy (Auth0). The startup weight on strategic differentiation (auth is not differentiated) and team velocity (3 months on auth is a startup-killer at this stage) makes this clear. The 12-month assumption: at $23K/year, Auth0 pricing is manageable; if the company grows to the tier requiring $150K+/year, evaluate Keycloak migration at that point.

---

## Further Reading

- Christensen, Clayton M. *The Innovator's Dilemma.* Harvard Business Review Press, 1997. The original framework for distinguishing core, differentiating capability from commodity capability — foundational to the strategic differentiation dimension.
- Fowler, Martin. "Make or Buy?" *martinfowler.com*, 2018. https://martinfowler.com/bliki/MakeOrBuy.html. A concise primary-source treatment of the decision from an architecture perspective; covers the false binary and introduces the concept of integration as its own cost category.
- CNCF. *Cloud Native Computing Foundation Landscape.* https://landscape.cncf.io/. The authoritative map of the open-source ecosystem for infrastructure and platform categories — essential reference when evaluating the partner option for DevOps and data platform decisions.

[^bvb-sandbox-1]: [ADR-018: Observability Stack at `413f8a6`](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/docs/adr/ADR-018-observability-stack.md). The "Alternatives considered" section compares Honeycomb, Datadog, Grafana Cloud, and self-hosting on vendor lock-in tolerance, traffic-shape fit, and 5-year TCO — the same dimensions Sections "Total Cost of Ownership," "Integration Complexity," and "Vendor Risk and Lock-In" of this framework name. Useful as a concrete example of what the framework's analysis looks like when written out for a real decision, even though the decision context (a personal-project sandbox) differs from the org-scale decisions this framework is designed for.
