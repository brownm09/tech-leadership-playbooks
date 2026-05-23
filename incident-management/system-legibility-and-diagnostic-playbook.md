# System Legibility and Diagnostic Judgment Playbook

## Purpose

This playbook addresses a specific failure mode in engineering organizations: systems that work
but resist diagnosis — by new engineers, by responders under pressure, or by anyone absent
when the system was built. It covers what system legibility means, how it decays,
and the concrete practices preserving it.

The primary audience: engineering leaders and senior individual contributors responsible for
on-call programs, service ownership standards, and onboarding design. It complements the
[On-Call Restructuring Framework](on-call-restructuring-framework.md), which covers rotation
structure and incident coordination; this playbook covers the underlying diagnostic
infrastructure making a rotation sustainable.

> **Demonstration sandbox:** [lifting-logbook](https://github.com/brownm09/lifting-logbook)
> is a personal-project monorepo, not a production system at scale. The artifact linked
> here illustrates the technique; production-scale application of the same technique is
> documented in [ORIGINS.md](../ORIGINS.md) where applicable.

---

## The Core Problem: Two Kinds of Legibility

System legibility in software operates at two levels simultaneously, and they decay
independently.

**Code legibility** names the property Fred Brooks called *conceptual integrity* in *The Mythical
Man-Month* (Addison-Wesley, 1975): a system behaves as if designed by a single coherent mind.
Where conceptual integrity is absent, engineers cannot predict a system's behavior from
inspection. They test by poking and observing — slow and unreliable under pressure.

**Runtime legibility** asks whether an engineer can understand what the system is doing *right
now*. Traditional monitoring — dashboards of predefined metrics — operates as legibility theater:
it makes a system look understandable while hiding the state space producing incidents.
High-cardinality, high-dimensionality telemetry remains the only way to make a running system legible
against novel failure modes.[^1] A system legible only against anticipated failures is illegible
against the failures that actually matter.

Both kinds of legibility require active maintenance. Neither is a one-time documentation
exercise.

---

## How Judgment Atrophies in Software Systems

Organizations build systems to scale coordination and reduce variance. Every system absorbing
a decision also erodes the judgment capacity capable of making it. The longer a system runs
without challenge, the more its internal logic becomes invisible — to operators, to managers,
and eventually to its original designers.

### The Automation Trap

**ORMs and query planners** trade SQL legibility for development velocity. Generally
worth it — until a query plan degrades under production load and the engineer debugging it has
never written a JOIN. The system worked; the human judgment needed to fix it when it stopped had
quietly atrophied. Michael Feathers describes the downstream result in *Working Effectively with
Legacy Code* (Prentice Hall, 2004): code that works but resists safe change, because
the understanding required to change it was never built or has since been lost.

**CI/CD pipelines** create a structurally similar problem at the deployment level. Green tests
serve as a legibility proxy: they assert correct behavior on the dimensions the test suite measures.
Engineers trusting the proxy lose the habit of reasoning about what the pipeline doesn't cover —
infrastructure state, downstream dependencies, traffic patterns. Automation bias migrates
judgment to the binary green/red output of a pipeline whose coverage goes uninspected.

**AI-assisted coding** marks the current frontier of this dynamic. The output is plausible and
often correct — exactly the condition under which judgment atrophies fastest. The risk:
engineers shipping AI-generated code without building a mental model of it have no basis for
diagnosing it when it fails in a novel way.

### Normalization of Deviance

Diane Vaughan's analysis of the Challenger disaster in *The Challenger Launch Decision*
(University of Chicago Press, 1996) describes the institutional version of this failure: small
deviations from expected behavior get observed, then rationalized as acceptable. The decision
process looks correct by its own internal measures, while those measures quietly drift from what
they were designed to track. The same mechanism operates in software systems when alert
thresholds get tuned to eliminate noise instead of reflecting actual risk — and when no one
reviews the thresholds afterward.

---

## Interval Validation Mechanisms

Several engineering practices function as legibility-preservation mechanisms wearing other labels.
The key insight: legibility must be validated at the same cadence as the system changes, never
as a one-time setup activity.

### Architecture Decision Records (Per Decision)

ADRs — documented in Michael Nygard's 2011 post[^2] — force the decision-maker to articulate
why a system has the shape it does, in terms a future engineer can evaluate. Without them,
architectural decisions accumulate as tacit knowledge in the heads of the engineers who made
them, then evaporate when those engineers leave. ADRs serve as the primary tool for keeping the
*why* of a system legible over time.

### Fitness Functions (Per Commit)

Fitness functions, from Neal Ford, Rebecca Parsons, and Patrick Kua's *Building Evolutionary
Architectures* (O'Reilly, 2017), encode automated assertions over architectural properties:
dependency direction, module coupling, latency budgets, security posture. Legibility of
architectural intent needs mechanical enforcement at the same cadence as code changes or
it drifts. A fitness function operates as a legibility check running in CI.

### Chaos Engineering and Game Days (Quarterly to Annually)

Chaos engineering and game days (developed formally at Netflix and described in Gregor Hohpe's
*The Software Architect Elevator*, O'Reilly, 2020) provide explicit interval validation of
operational judgment. The point: verify engineers can still reason about and respond to failure
modes, independent of whether the system is currently failing — bug discovery comes second.
Every question a responder cannot answer during a game day exposes a gap in the runbook or
in the system's observability.

### Cross-Service On-Call Rotation (Ongoing)

Werner Vogels' 2006 articulation of "you build it, you run it" at Amazon[^3] originated as a
quality incentive; its legibility effect carries equal weight. Engineers operating their
systems under pressure build mental models design-time work does not produce. Rotation
across *unfamiliar* services constitutes the stronger version — it surfaces the gap between
documentation and actual operational knowledge, exactly the gap mattering in an incident.

### Runbook Criteria Review (Annually)

Do not ask only "are we hitting our targets?" Ask "do our targets still track what we care
about?" Any alert threshold or runbook trigger condition adjusted to reduce alert
frequency without a corresponding investigation of the underlying signal counts as a legibility
regression. Criteria review should constitute an explicit annual step, separate from the incident
retrospective.

---

## Making Systems Diagnosable for Onboarding Engineers

The hardest version of the legibility problem eliminates the one thing most systems rely on —
implicit knowledge in the team. The right frame: what artifacts does the system produce
enabling correct inference by someone with no priors?

### Layer 1: Service Catalog

An engineer who has never touched a service needs to answer four questions before diagnosing
anything:

1. What does this service do and what does it own?
2. What does it depend on, and what depends on it?
3. What does healthy look like?
4. Who wrote it and who operates it?

A service catalog — with those four fields, kept current, and linked from the alerting system —
delivers the single highest-leverage investment for onboarding legibility. Spotify's Backstage[^4]
provides the reference implementation, but the format matters less than the discipline: every service has
an entry, entries are owner-maintained, and alerts link to them.

A catalog is not a wiki. It functions as a machine-readable registry with a human-readable summary.
Wikis rot; catalogs with ownership fields have someone accountable for keeping them current.

### Layer 2: Distributed Tracing with Meaningful Context

Tracing remains the only observability primitive preserving causality across service boundaries.
For an onboarding engineer, this draws the line between "the checkout service is slow" and
"the checkout service is slow because a downstream call to the inventory service is timing out,
and here is the specific request path." The first requires system knowledge to diagnose; the
second is followable by anyone who can read a waterfall.

As Cindy Sridharan establishes in *Distributed Systems Observability* (O'Reilly, 2018), metrics
surface the existence of a problem. Traces surface its location and cause.[^5] The practical requirements:

- Trace IDs must propagate through every service boundary, including async queues and caches
- Spans must carry enough context to identify the operation — beyond a function name, the
  relevant parameters (order ID, user ID, SKU) making a trace instance interpretable
- Errors must attach structured context, beyond a message string

### Layer 3: Runbooks Structured for Diagnosis

Most runbooks describe what to do after diagnosis. The wrong ordering for an engineer
new to the system. A useful runbook starts with symptoms and walks toward cause:

1. **Alert context.** What does this metric measure and what does it mean when it's elevated?
2. **First look.** The specific dashboard or query that shows current system state in one view.
   What healthy looks like on that view.
3. **Decision tree.** If you see A, proceed to step 4. If you see B, escalate to service owner.
4. **Known failure modes.** The three to five historical causes of this alert, with distinguishing
   symptoms.
5. **Remediation.** The action, its blast radius, and its reversibility.
6. **Escalation.** Role, not individual — who to call and what to tell them.

Michael Nygard's *Release It!* (Pragmatic Bookshelf, 2nd ed., 2018) establishes the
architectural prerequisite: systems must be designed with operational affordances — health
endpoints, structured error output, graceful degradation signals — making the diagnostic path
navigable. A runbook performs only as well as the system it describes; if the system does not expose
its own state cleanly, no runbook compensates.

### Layer 4: Error Messages Designed as Diagnostics

An error message gets read by an engineer who does not know what caused it. Its job: narrow
the search space. Good error messages answer: what was attempted, what failed, and what state
the system was in when it failed.

"Connection refused" does not function as a diagnostic. "Connection refused to
postgres-primary.internal:5432 after 3 retries; last attempted at 14:23:07; upstream service
returned HTTP 503 at 14:22:59 — possible upstream availability issue" gives a new engineer a
hypothesis and a timeline.

The Google SRE book (Beyer, Jones, Petoff, Murphy, O'Reilly, 2016) frames this as part of SLO
design: errors at service boundaries should distinguish user-visible impact from internal
degradation.[^6] That structuring requirement naturally improves error message quality, because
it forces engineers to think about what information a responder needs.

### Layer 5: The Onboarding Engineer as Legibility Probe

The most effective validation of onboarding legibility: deliberately give a new engineer a
staged incident and observe where they get stuck.

Run a structured shadow on-call exercise where the new engineer responds to a simulated incident
with an experienced engineer watching silently. Treat every question the new engineer asks as a
gap report — a missing runbook step, an untraced service boundary, a metric without a
description, an error message without context.

Matthew Skelton and Manuel Pais's *Team Topologies* (IT Revolution, 2019) frames this as a
cognitive load problem: if a new engineer cannot build a sufficient mental model of a service to
diagnose it within a bounded time window, the service's cognitive load exceeds what a single
person can hold, and the team structure needs to change to match.[^7] The shadow on-call
exercise surfaces this before the production incident does.

---

## What Doesn't Work

**Architecture diagrams maintained manually.** They go stale within a month. Auto-generated
dependency maps from traces or service mesh telemetry stay current; hand-drawn diagrams do not.

**"Ask the team" as an escalation path.** It works until the team turns over — exactly the
moment of greatest need. Escalation paths should name roles, never individuals.

**Onboarding buddy programs without legibility artifacts.** They transfer tacit knowledge — a
valuable transfer, but one leaving the underlying gap intact. When the buddy leaves, the
knowledge leaves with them.

**Long wikis.** They encode what the author thinks a newcomer needs, filtered through what the
author has forgotten they know. By the time a responder reaches them, the content has aged past
its useful window and the responder has already exhausted the channels worth trusting.

---

## Summary

Legibility for new engineers operates as an architectural property requiring maintenance at the
same cadence as the system itself. The governing question for any system: *what artifacts does
this produce enabling correct inference by someone with no priors, under pressure?* Answer
that question concretely — with a service catalog entry, a trace, a runbook starting with
symptoms, and error messages pointing toward causes — and onboarding legibility follows as a
byproduct.

---

## Further reading: demonstration artifacts

The artifacts below illustrate the techniques described in this playbook against the demonstration sandbox introduced after the Purpose section. See [LINKING.md](../LINKING.md) for the full convention. Citation links pin to commit [`413f8a6`](https://github.com/brownm09/lifting-logbook/tree/413f8a62f43f12fa200be3e3307da7ef72c7b446) per the LINKING.md SHA-pinning rule. Where an artifact is intended to evolve as the stack does, a `main` link is provided alongside.

### On runtime legibility and high-cardinality telemetry

- **Backend selection rationale** — [ADR-018: Observability Stack](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/docs/adr/ADR-018-observability-stack.md). Records the OpenTelemetry + Grafana Cloud (Tempo / Mimir / Loki) decision, the GKE DaemonSet collector topology, the Prisma instrumentation choice, and the head-based 100% sampling stance until production traffic exists. The alternatives-considered section is the part most worth reading: it makes explicit why Honeycomb, Datadog, and self-hosting were rejected for this project's constraints — illustrating the playbook's claim that "the right backend" is a function of traffic shape, vendor lock-in tolerance, and what the team needs to query, not a context-free best practice.
- **Local development verification path** — [Observability Runbook](https://github.com/brownm09/lifting-logbook/blob/main/docs/runbooks/observability.md) (live state). Documents the docker-compose stack (OTel Collector + Tempo + Loki + Prometheus + Grafana) that mirrors the production topology, plus the trace-by-`trace_id` lookup flow and log↔trace correlation queries. Demonstrates the playbook's argument that runtime legibility must be verifiable on a developer's laptop, not only in production.

### On runbook structure and symptom-first authoring

- **Runbook index** — [`docs/runbooks/README.md`](https://github.com/brownm09/lifting-logbook/blob/main/docs/runbooks/README.md) (live state).
- **Worked runbooks** — [`api-5xx-surge.md`](https://github.com/brownm09/lifting-logbook/blob/main/docs/runbooks/api-5xx-surge.md), [`auth-provider-outage.md`](https://github.com/brownm09/lifting-logbook/blob/main/docs/runbooks/auth-provider-outage.md), [`database-unreachable.md`](https://github.com/brownm09/lifting-logbook/blob/main/docs/runbooks/database-unreachable.md), [`deploy-regression-rollback.md`](https://github.com/brownm09/lifting-logbook/blob/main/docs/runbooks/deploy-regression-rollback.md). Each is organized around the symptom a responder sees, not the component an architect would name. They demonstrate the playbook's claim that legibility artifacts must be authored from the responder's frame of reference, not the designer's.

### On SLOs as legibility infrastructure

- **SLO definitions** — citation: [`docs/operations/slo.md` at 413f8a6](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/docs/operations/slo.md); live state: [same path on `main`](https://github.com/brownm09/lifting-logbook/blob/main/docs/operations/slo.md). Defines availability and p95 latency SLOs for `apps/api` with explicit PromQL expressions, the 28-day rolling window, and the deliberate exclusion of 4xx responses from the bad-request count. Demonstrates that an SLO is only legible if its definition is precise enough to argue about — and that the argument is the point.
- **SLO methodology rationale** — citation: [ADR-019: SLO Methodology at `413f8a6`](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/docs/adr/ADR-019-slo-methodology.md); live state: [same path on `main`](https://github.com/brownm09/lifting-logbook/blob/main/docs/adr/ADR-019-slo-methodology.md). Records why these specific definitions, why these targets, and why burn-rate alerting (Workbook methodology) over simple threshold alerts.

### On on-call posture for the responder

- **On-call ops doc** — [`docs/operations/on-call.md`](https://github.com/brownm09/lifting-logbook/blob/main/docs/operations/on-call.md) (live state). Shows how the posture described in the [On-Call Restructuring Framework](on-call-restructuring-framework.md) collapses to a single-operator project: the diagnostic infrastructure (telemetry, runbooks, SLOs) is what makes any rotation workable, including a rotation of one.

---

These artifacts are not exhaustive. Per [LINKING.md](../LINKING.md), additional cross-references are added only where they add evaluative power — not as breadth for its own sake.

---

## References

[^1]: Majors, C., Fong-Jones, L., & Miranda, G. (2022). *Observability Engineering*. O'Reilly Media.

[^2]: Nygard, M. (2011, November 15). Documenting Architecture Decisions. *Cognitect Blog*. https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions

[^3]: Vogels, W. (2006). All Things Distributed: *ACM Queue interview on Amazon's architecture*. https://queue.acm.org/detail.cfm?id=1142065

[^4]: Spotify Engineering. (2020). *Backstage: An open platform for building developer portals*. https://backstage.io

[^5]: Sridharan, C. (2018). *Distributed Systems Observability*. O'Reilly Media.

[^6]: Beyer, B., Jones, C., Petoff, J., & Murphy, R. (Eds.). (2016). *Site Reliability Engineering: How Google Runs Production Systems*. O'Reilly Media.

[^7]: Skelton, M., & Pais, M. (2019). *Team Topologies: Organizing Business and Technology Teams for Fast Flow*. IT Revolution Press.
