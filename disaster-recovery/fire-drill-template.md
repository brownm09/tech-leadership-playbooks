# Disaster Recovery Fire Drill Template

## Leadership Context

Boards and enterprise customers increasingly ask for evidence of DR validation; a tabletop exercise log answers the question. Fire drills also surface assumptions baked into recovery plans before an actual incident reveals them at cost: undocumented steps, single-person knowledge, and unvalidated RTO commitments.

## Purpose

This template formalizes the tabletop exercise format used to stress-test disaster recovery posture and incident management protocols before a real event forces the issue. Tabletop runs surface gaps in failover readiness, standardization debt, and coordination assumptions while discovery cost stays low.

Tabletop exercises prioritize catastrophic failure scenarios first (full failover, data loss, regional outage) and work down to lesser issues in subsequent runs. Starting with the worst case validates critical-function recovery before teams spend time on lower-severity paths.

## Background and Motivation

At ActBlue Technical Services (2022–2024), no formal DR drill process existed. I designed the fire drill structure and ran the drills. The exercise validated payment continuity during a disaster scenario — the single most critical function of the system. The format was modeled after the TREX (Threat and Risk Exercise) approach used at Capital One, adapted for a payments-focused engineering org. Outcome: improved incident preparedness; DR posture validated before a real event.

The first exercise exposed a standardization gap: user-facing components were hosted on Heroku, outside the standard AWS/Kubernetes stack. During the tabletop, engineers working through failover procedures immediately hit friction because Heroku recovery paths were unfamiliar and underdocumented compared to the rest of the platform. This led to a migration of those components to AWS/Kubernetes, eliminating $64K/year in infrastructure costs and removing the operational outlier from DR scope.

## Participants

Core participants for a payments or high-volume platform:

- Platform architect
- Security team lead (and one senior security engineer)
- Platform engineers with ownership of critical paths (2-3)
- Engineering managers for security and platform

Target size: 6-10 people. Larger groups dilute focus; smaller groups miss cross-team dependencies.

Exclude: stakeholders without technical decision-making authority. Tabletops move faster when participants can commit to remediation on the spot.

## Scenario Sequencing

Run scenarios in this order across multiple exercises:

1. **Catastrophic / full failover:** complete loss of primary region, failover to DR environment, verify critical functions (payment receipt, data integrity)
2. **Partial outage:** single-service failure with blast radius analysis, identify cascading dependencies
3. **Data corruption or loss:** backup restoration procedures, RPO/RTO validation
4. **Security incident:** credentials compromised, unauthorized access to production systems
5. **Third-party dependency failure:** payment processor outage, CDN failure, DNS provider incident

Run scenario 1 first. If the platform cannot survive it on paper, the remaining scenarios are secondary.

## Exercise Format

**Duration:** 2-3 hours per session

**Format:** Tabletop only (no live production changes). Facilitator presents the scenario; participants walk through response procedures step by step.

**Facilitator role:** Drive the scenario, ask probing questions, surface assumptions ("you said you'd page the on-call engineer; what if they're unavailable?"), and document gaps in real time.

**Documentation during the exercise:**
- Record each procedural step as participants describe it
- Flag steps relying on tribal knowledge or undocumented runbooks
- Note any single points of failure or previously invisible dependencies

## Pre-Exercise Checklist

- [ ] Runbooks for critical systems are current and accessible in shared documentation
- [ ] Recovery Time Objective (RTO) and Recovery Point Objective (RPO) targets are defined and agreed upon
- [ ] On-call escalation paths are documented
- [ ] Participants have reviewed architecture diagrams for systems in scope
- [ ] Facilitator has prepared 3-5 probing questions per scenario to stress assumptions

## Post-Exercise Output

The exercise produces a gap register. For each gap identified:

| Gap | Impact | Owner | Remediation | Target Date |
|-----|--------|-------|-------------|-------------|
| Heroku components outside AWS/K8s standard | Engineers unfamiliar with recovery path; DR scope harder to reason about | Platform lead | Migrate to AWS/K8s | Q[X] |

Gaps are prioritized by impact on critical functions (payment receipt, data integrity) first. Standardization gaps increasing cognitive load are tracked as risk items in the register, with remediation prioritized accordingly.

## Rerun Cadence

- Run the catastrophic failover scenario annually at minimum
- Re-run any scenario where a gap was found after remediation is complete
- Run an unannounced partial-scope exercise 6 months after the initial full exercise to test whether runbooks are being maintained

## What to Watch For

**Heroku/non-standard hosting:** Components outside the standard deployment stack add DR risk. Engineers default to patterns they know under pressure, and outliers slow response time and increase the chance of error.

**Undocumented recovery steps:** If a participant says "I would just..." and cannot point to a runbook, document the gap.

**Assumed availability:** Scenarios depending on a specific person being reachable measure individual availability; the procedure itself stays untested. Verify procedures work without named individuals.

**Unvalidated RPO/RTO:** A stated RPO of 1 hour with no restore test stays aspirational. Run the restore before treating the number as a commitment.
