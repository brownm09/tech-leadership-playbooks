# Technical Strategy Template

## Leadership Context

A technical strategy document is the primary artifact through which an engineering leader translates business objectives into engineering investment. Without it, headcount decisions are negotiated on intuition, architectural investments compete with feature work on a quarter-by-quarter basis, and the engineering organization operates reactively. With it, there is a shared frame for what the engineering organization is trying to become and what it will and will not invest in to get there.

The primary audience for a technical strategy is the executive team, the board, and the finance function that controls the budget. It must be legible to a CFO who wants to know why replatforming requires 6 headcount for 18 months, and to a CEO who wants to know how the technical investment connects to market position. Engineers read it too, and it must be credible to them; optimizing the document for engineers at the expense of executive legibility is a frequent failure mode.

## Background and Motivation

This template was developed from two successive technical strategy engagements:

1. **ActBlue Technical Services (2024–2025):** I developed the multi-year platform directorate technical vision, set direction for a 6-team directorate, and presented roadmaps and program status to the VP of Engineering and Head of Product.

2. **Community Tech Alliance (2025–2026):** I designed and wrote a 12-month technical investment roadmap entirely, prioritizing DR posture, dependency management, and release confidence for a lean GCP-based engineering org.

---

## When to Use This

| Trigger | What the document needs to do |
|---------|-------------------------------|
| New Director/Head of Engineering in first 90 days | Establish a baseline, demonstrate command of the technical situation, and signal where investment will go |
| Annual planning cycle | Anchor engineering's budget request in a multi-year technical direction |
| Major pivot or strategy change | Translate the new business direction into engineering implications and trade-offs |
| Replatforming or major architectural investment | Frame the investment case — current state, target state, cost, risk, return |
| Post-acquisition integration | Define which systems survive, which are sunset, and on what timeline |

Do not write a technical strategy in response to a single incident or a short-term pressure. A strategy written under duress is a tactical plan mislabeled. A technical strategy should be the answer to a 12–36 month question, not a 60-day question.

---

## The Five Sections Every Technical Strategy Needs

A technical strategy document that is missing any of these five sections is incomplete. Each section answers a different executive question.

| Section | Executive question it answers |
|---------|-------------------------------|
| Current State Assessment | What is the situation we are starting from? |
| Target State Vision | What does success look like in 1–3 years? |
| Gap Analysis | What is standing between current and target? |
| Investment Roadmap | What do we need to do, in what order, and at what cost? |
| Success Metrics | How will we know if the strategy is working? |

The sections are sequential. A strategy that jumps to Investment Roadmap without a rigorous Current State Assessment is a wishlist. The diagnosis validates the prescription.

---

## Section 1: Current State Assessment

### What it needs to cover

The current state assessment is a structured diagnosis of the engineering organization at the moment the strategy begins. It should be uncomfortable to write if the situation is not good. It should name the specific systems, teams, and practices that are constraining delivery — not describe them in generalities.

**Technical debt inventory**

A technical debt inventory classifies debt by its impact on delivery, reliability, and the team's ability to change the system; it is not a list of "things engineers wish they could refactor."

Classify debt into three categories:

| Category | Definition | Example |
|----------|-----------|---------|
| Delivery-blocking | Debt that directly slows new feature development or requires heroic effort to work around | A monolith with no seam points, requiring all 12 engineers to coordinate on every deployment |
| Reliability-degrading | Debt that is causing or is likely to cause production incidents | An authentication service that was never designed for the current traffic volume and fails at load spikes |
| Velocity-taxing | Debt that adds friction but is not acute | Test suite that takes 45 minutes to run, slowing the PR review cycle |

Only category 1 and 2 debt belong in a technical strategy. Category 3 debt should be addressed in normal engineering operations without requiring executive attention.

**DORA baseline**

The current state assessment should include a DORA baseline so that the strategy has measurable starting points. Without this, progress claims are unverifiable.

| Metric | Current | Tier |
|--------|---------|------|
| Deployment frequency | ? | ? |
| Lead time for change | ? | ? |
| Change failure rate | ? | ? |
| MTTR | ? | ? |

If you cannot fill in this table, instrumentation is part of the strategy.

**Team topology audit**

Map current team ownership against the target architecture to identify:
- Services with no clear owner
- Teams that own too many systems to maintain them well
- Boundaries that require constant cross-team coordination

See the Team Topology Framework playbook for the cognitive load audit methodology.

**Dependency map**

Identify critical external and internal dependencies:
- Vendors or platforms with risk concentration (single vendor for infrastructure, payments, etc.)
- Internal service dependencies that create coupling across team boundaries
- Third-party integrations with poor reliability or vendor stability risk

### Template: Current State Section

```
## Current State

### What Is Working
[2–3 specific strengths: capabilities the team has demonstrated, systems that are running well,
practices that are ahead of where they would be expected to be. Be specific — name the systems.]

### What Is Constraining Us
[Categorized technical debt inventory. Use the delivery-blocking / reliability-degrading /
velocity-taxing classification. For each item: name the system or practice, describe the
constraint it creates, and estimate the cost in engineering capacity or business risk.]

### DORA Baseline
[Table: metric, current value, tier. If instrumentation does not exist yet, state that
and include instrumentation as a strategy item.]

### Team Topology
[Brief description of current team structure. Identify ownership gaps and coupling problems.
Reference the team topology audit results.]

### Key Risks
[3–5 specific risks the current state creates: reliability risks, delivery risks, competitive
risks, team sustainability risks. Each risk should be falsifiable — it either materializes
or it does not.]
```

---

## Section 2: Target State Vision

### What it needs to cover

The target state vision describes what the engineering organization looks like in 1–3 years if the strategy succeeds. It should be specific enough to be falsifiable (you should be able to tell when you have arrived) without over-specifying implementation detail.

**What to specify:**

- The organizational capabilities the engineering team will have that it does not have today (e.g., "teams deploy independently to production without coordinating across service boundaries")
- The reliability posture (e.g., "99.9% availability on all customer-facing APIs; MTTR under 60 minutes for P0 incidents")
- The delivery throughput (e.g., "deployment frequency of 5+ per team per week; lead time under 2 days")
- The architecture pattern (e.g., "domain-oriented services with defined API contracts; no shared databases between domains")
- The platform capability (e.g., "self-service deployment pipeline that allows any team to provision a new service in under 4 hours without platform team involvement")

**What not to specify:**

- Specific technologies or vendors (unless choosing a technology is itself a strategic decision — e.g., "we are consolidating on AWS")
- Implementation detail of individual services
- Sprint-level delivery plans

The target state vision should be stable across leadership transitions. If a new CTO would immediately rewrite your target state, the vision is too tactical.

### Template: Target State Section

```
## Target State Vision

By [DATE — 12–36 months from strategy start], the engineering organization will:

**Delivery capability:**
[Specific, measurable delivery capability: deployment frequency, lead time, independence
between teams. Example: "Teams deploy to production independently, without cross-team
coordination, at a frequency of 5+ times per week per team."]

**Reliability posture:**
[Specific reliability targets: SLOs, MTTR, incident frequency. Example: "All customer-facing
services maintain 99.9% availability with an MTTR under 60 minutes for P0 incidents."]

**Architecture pattern:**
[High-level architecture direction: what the service ownership model looks like, what the
platform abstraction provides, what the inter-team API contract model is.]

**Team structure:**
[What the team topology looks like — not individual team names, but the model: how many
stream-aligned teams, whether a platform team exists, what the enabling capability looks like.]

**What we will NOT do:**
[Explicit exclusions. A strategy that tries to achieve everything achieves nothing. Name
what is out of scope: technologies you are not adopting, architectural patterns you are
explicitly rejecting, capability investments you are deferring.]
```

---

## Section 3: Gap Analysis

### What it needs to cover

The gap analysis is the bridge between current state and target state. It names the specific gaps — technical, organizational, and capability gaps — that must be closed to achieve the target. Each gap becomes a candidate for the investment roadmap.

**Categorize gaps by type:**

| Gap type | Example |
|----------|---------|
| Technical | Monolithic architecture prevents independent team deployment |
| Organizational | No platform team exists; each stream-aligned team is provisioning its own infrastructure |
| Capability | Engineering org has no observability expertise; no one can interpret distributed traces |
| Process | No SLO definition or monitoring; reliability posture is undefined |
| Tooling | No deployment pipeline; engineers are manually deploying from local machines |

**For each gap, estimate:**
- Impact on the target state if the gap is not closed (high / medium / low)
- Effort to close (engineering months; order of magnitude)
- Dependencies: does closing this gap depend on closing another gap first?

### Template: Gap Analysis Section

```
## Gap Analysis

| Gap | Type | Impact if unaddressed | Estimated effort | Dependencies |
|-----|------|-----------------------|-----------------|--------------|
| [Gap 1] | [Technical / Org / Capability / Process / Tooling] | [High / Medium / Low] | [X engineer-months] | [None / Gap N] |
| [Gap 2] | ... | ... | ... | ... |

### Gap 1: [Name]
**Current state:** [What exists today]
**Target state:** [What needs to exist]
**Why this gap matters:** [Business impact if unaddressed — connect to revenue, reliability, or
team sustainability]
**Closing the gap:** [High-level approach — not implementation detail, but architectural or
organizational direction]
```

---

## Section 4: Investment Roadmap

### What it needs to cover

The investment roadmap organizes the work required to close the gaps into a time-phased plan. Use a horizon model: 0–6 months (foundations), 6–18 months (core investment), 18–36 months (target state enablement). This framing is more durable than quarterly plans because it survives the inevitable schedule changes in years 2 and 3.

**For each horizon:**
- Name the specific investments (not epics — investment categories)
- Quantify the resource requirement (headcount, quarters of capacity)
- State the explicit trade-off: what does the org stop doing or delay to make this investment?
- Define the outcome expected at the end of the horizon

**The trade-off section is mandatory.** A strategy without explicit trade-offs is an aspiration. If you are asking for 3 engineer-years to build a platform team's self-service infrastructure, you need to say what that 3 engineer-years is not going towards — and that conversation must happen with the business, not be deferred.

### Template: Investment Roadmap Section

```
## Investment Roadmap

### Horizon 1: Foundations (0–6 months)

**Goal:** [What must be true at the end of this horizon for Horizon 2 to succeed]

**Investments:**

| Investment | What it involves | Resource requirement | Trade-off |
|------------|-----------------|---------------------|-----------|
| [Investment 1] | [1–2 sentences: what we are building or changing] | [N engineer-months] | [What we are not doing to fund this] |
| [Investment 2] | ... | ... | ... |

**Exit criteria:** [Specific, measurable conditions that define when Horizon 1 is complete.
Example: "DORA baseline instrumented; deployment pipeline gating on all required controls;
platform team chartered and staffed."]

---

### Horizon 2: Core Investment (6–18 months)

**Goal:** [What must be true at the end of this horizon for Horizon 3 to succeed]

**Investments:**

| Investment | What it involves | Resource requirement | Trade-off |
|------------|-----------------|---------------------|-----------|
| [Investment 1] | ... | ... | ... |

**Exit criteria:** [Specific, measurable conditions]

---

### Horizon 3: Target State Enablement (18–36 months)

**Goal:** [What the org looks like when the strategy is complete]

**Investments:**

| Investment | What it involves | Resource requirement | Trade-off |
|------------|-----------------|---------------------|-----------|
| [Investment 1] | ... | ... | ... |

**Exit criteria:** [Matches the target state conditions from Section 2]

---

### Resource Summary

| Horizon | Engineering headcount | Duration | Key outcomes |
|---------|----------------------|----------|--------------|
| Foundations | [N] | 0–6 months | [Exit criteria summary] |
| Core Investment | [N] | 6–18 months | [Exit criteria summary] |
| Target State Enablement | [N] | 18–36 months | [Target state conditions] |
```

---

## Section 5: Success Metrics

### What it needs to cover

Success metrics close the loop between the strategy and the health scorecard. They define how leadership will know whether the strategy is working. They should be observable, not aspirational — you should be able to pull a report and see whether you are on track.

**One metric per strategic goal.** If you have five strategic goals, you have five primary success metrics. Adding more than one metric per goal creates measurement overhead without additional clarity.

**Include leading indicators.** Lagging indicators (deployment frequency, MTTR) tell you what happened. Leading indicators tell you whether you are on track before the outcome is final. For a platform investment, the leading indicator might be: percentage of teams that have adopted the new deployment interface.

### Template: Success Metrics Section

```
## Success Metrics

### Primary Metrics (by horizon)

**Horizon 1 success if:**
- [Metric 1]: [Target] by [date]
- [Metric 2]: [Target] by [date]

**Horizon 2 success if:**
- [Metric 1]: [Target] by [date]
- [Metric 2]: [Target] by [date]

**Horizon 3 (strategy complete) success if:**
- Deployment frequency: [target]
- Lead time: [target]
- SLO attainment: [target]
- On-call load: [target]
- [Org capability metric]: [target]

### Leading Indicators (to track monthly)
- [Leading indicator 1]: [what it measures, why it predicts the lagging outcome]
- [Leading indicator 2]: ...
```

---

## Communication Formats by Audience

The strategy document is the source of record. It is not what you present to every audience. Adapt the format to the audience without changing the substance.

**For engineers (full document, 10–20 pages):**
- All five sections in full
- Technical detail on current state: system names, specific debt items, architecture diagrams
- Honest diagnosis of what is broken and why
- Explicit trade-offs and sequencing rationale

**For executive team (5-slide summary):**
1. Current state: 3 bullets — the technical constraints binding the business today
2. Target state: 3 bullets — what the engineering org will be capable of in 18–36 months
3. Investment roadmap: one-page table by horizon — investment, headcount, outcome
4. Success metrics: primary metrics with targets by horizon
5. Risks: 2–3 execution risks and mitigation approaches

**For board (verbal narrative, 5 minutes):**
Focus on three things: (1) where the engineering org is relative to the scale demands of the business, (2) the one or two material risks to business continuity, (3) the headline investment and expected return. Do not show the roadmap table; describe the direction. Have the detailed document ready for follow-up questions.

**For finance / budget review:**
Lead with the resource summary table. Connect each investment to a business outcome: "the platform investment reduces new feature time-to-production by 40%, which accelerates revenue growth by [N] months." If you cannot articulate the ROI of an engineering investment in business terms, you will not win the budget conversation.

---

## Common Failure Modes

**Too much aspiration, too little diagnosis**

A strategy that describes a beautiful target state without a credible current state assessment is a vision statement. Executives who have seen vision statements before (which is all of them) will not fund it. The diagnosis is what makes the investment case credible.

*Fix:* Write the current state section first. If you cannot fill it in specifically, you do not know the current state well enough to write a strategy yet.

**Strategy without resourcing**

A strategy that identifies 12 major initiatives and does not specify how many engineers are required, or where they come from, is not actionable. "We will address technical debt in parallel with feature delivery" is not a resourcing plan.

*Fix:* Every investment in the roadmap requires a headcount number and a trade-off statement. If you cannot name the trade-off, you are asking for unlimited resources, which is not a strategy.

**Specificity trap: over-specifying implementation**

A strategy that specifies "we will migrate from PostgreSQL to Aurora Serverless, move from Docker Compose to EKS, replace Kafka with Kinesis, and adopt GraphQL federation" is making decisions that belong to engineers, not to a strategy document. Technology choices at this level belong in Architecture Decision Records.

*Fix:* The strategy specifies the capability (self-service deployment platform) and the constraint (teams must be able to deploy independently without platform team involvement). The team chooses the implementation.

**Horizon 1 is too long**

A Horizon 1 that lasts 12+ months is not foundations — it is the strategy itself. Horizon 1 should deliver observable outcomes within 6 months, or executive confidence will erode before any results appear.

*Fix:* Horizon 1 exit criteria should be achievable in 3–6 months with the resources committed. If Horizon 1 requires 12 months, either the scope is too large or the resources are insufficient — and both of those are things the strategy should say explicitly.

**Metrics that are not owned**

Success metrics without a named owner do not get tracked. "Deployment frequency will reach 5/week" is not actionable unless someone is accountable for measuring it and reporting on it.

*Fix:* Every metric has an owner. The owner is responsible for the instrumentation, the monthly report, and the escalation if the metric is trending in the wrong direction.

---

## Further Reading

- Humble, Jez, Joanne Molesky, and Barry O'Reilly. *Lean Enterprise: How High Performance Organizations Innovate at Scale.* O'Reilly Media, 2015. Chapter 4 covers investment portfolio management for technology; directly applicable to the horizon model.
- Forsgren, Nicole, Jez Humble, and Gene Kim. *Accelerate: The Science of Lean Software and DevOps.* IT Revolution Press, 2018. The empirical foundation for DORA metrics and their predictive relationship to organizational outcomes.
- Larman, Craig, and Bas Vodde. *Scaling Lean & Agile Development: Thinking and Organizational Tools for Large-Scale Scrum.* Addison-Wesley, 2009. Chapter 2 on feature teams and the organizational dynamics that either enable or constrain technical strategy.
