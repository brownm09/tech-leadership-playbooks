# Engineering Health Scorecard

## Leadership Context

A Director or Head of Engineering without a way to quantify org health depends on narratives no one can validate and anyone can challenge. In a QBR, a board meeting, or an M&A diligence session, "the team ships fast and quality stays high" without supporting data lands as an assertion. The engineering health scorecard converts the assertion into a structured, defensible position: four domains measured, targets set, current values, trend direction, and a named owner per number.

Engineers need granular telemetry; this artifact serves a different audience. The health scorecard functions as a leadership communication artifact — 8–12 metrics organized by domain, answering the questions a non-technical executive will ask about engineering capability, reliability, and sustainability.

## Background and Motivation

I developed this scorecard format during the Jellyfish rollout and org-wide engineering health reporting work at ActBlue Technical Services (2024–2025). I led the rollout, owned engineering health reporting for the platform directorate, defined the metrics framework, and trained managers and ICs across six teams. The scorecard translates raw delivery data into the format a VP of Engineering uses in a QBR or board presentation.

---

## When to Use This

| Trigger | What the scorecard provides |
|---------|-----------------------------|
| New Director/Head of Engineering taking over | Establishes a baseline and surfaces the highest-leverage problems in the first 90 days |
| QBR or board update on engineering | Structured narrative that connects engineering activity to business outcomes |
| Headcount justification | Quantified evidence that the team is at capacity or that a specific domain is under-resourced |
| M&A technical diligence | Engineering maturity signal for acquirers or investors; shows the org has instrumented itself |
| Engineering leadership performance review | Gives the business a shared language for evaluating the engineering function |

Do not build a scorecard reactively, after a major incident, a missed launch, or a performance conversation with the CTO. Built reactively, the scorecard looks like a cover story. Built proactively and maintained consistently, it becomes a credibility asset.

---

## The Four Domains

### Domain 1: Delivery (DORA Metrics)

Delivery metrics measure how fast and reliably the team ships. They come from the DORA research program (Accelerate State of DevOps Report) and have been validated across thousands of engineering organizations as predictors of organizational performance.

**Deployment Frequency**

How often the team deploys to production. Not "how often the team commits code" or "how often a release branch is cut." Production deploys, per team, per week.

- Elite: Multiple per day
- High: Once per day to once per week
- Medium: Once per week to once per month
- Low: Less than once per month

*How to instrument:* Tag production deployment events in your CI/CD platform. GitHub Actions: filter workflow runs by environment = production. CircleCI: filter by context. Datadog CI Visibility can aggregate this across pipelines. For Jira-tracked releases: create a release event type and query by closed date.

**Lead Time for Change**

Time from first commit on a branch to that commit running in production. Measures the efficiency of the full delivery pipeline.

- Elite: Less than 1 hour
- High: 1 day to 1 week
- Medium: 1 week to 1 month
- Low: More than 1 month

*How to instrument:* Git commit timestamp → production deploy timestamp. LinearB, Jellyfish, and Swarmia automate this by connecting your Git provider to your deployment events. Manual proxy: Jira issue creation date to production deploy date (less precise but works without additional tooling).

**Change Failure Rate**

Percentage of production deployments requiring a hotfix, rollback, or immediate remediation. The quality gate on delivery speed: high deployment frequency with high change failure rate does not qualify as elite performance.

- Elite: 0–5%
- High: 5–10%
- Medium: 10–15%
- Low: More than 15%

*How to instrument:* Tag incidents or hotfixes causally linked to a deployment event. PagerDuty and Jeli both support incident tagging; the tag needs to connect to a deploy event. Among the four DORA metrics, this one breaks first on classification discipline. Incident tagging must consistently connect to the causative deploy event.

**Time to Restore Service (MTTR)**

How long it takes to restore normal service after a production failure. Measures incident response effectiveness.

- Elite: Less than 1 hour
- High: Less than 1 day
- Medium: 1 day to 1 week
- Low: More than 1 week

*How to instrument:* Jeli tracks incident open time and resolution time. PagerDuty incident duration works as a proxy but conflates acknowledgment with resolution. For a clean MTTR, define resolution as service restored to SLO; acknowledgment alone does not close the window.

---

### Domain 2: Quality

Quality metrics measure the rate at which defects escape into production and the org's ability to maintain the reliability contracts it has made with users.

**SLO Attainment**

Percentage of rolling 30-day windows in which each published SLO holds. Customer-facing quality lands here before anywhere else.

*Target:* Depends on what you have committed to. If you have not set SLOs yet, start there before building this scorecard. A reasonable starting target for a B2B SaaS platform: 99.5% availability SLO, measured per service tier.

*How to instrument:* Datadog SLO tracking, New Relic SLAs, or Honeycomb. Define SLOs in code (Terraform for Datadog SLOs is ideal for audit purposes). Report the number of SLOs in breach plus trending direction; a binary pass/fail collapses too much signal.

**Defect Escape Rate**

Percentage of bugs found in production versus bugs found in pre-production. A high defect escape rate means the test suite and staging environment are not catching what they should.

- Target: <20% of reported defects first detected in production
- Warning threshold: >35%

*How to instrument:* Jira bug label + environment field at time of creation. Requires discipline about where bugs are logged. If your team logs all bugs as customer-reported issues after the fact, the data will run noisy; establish a classification convention first.

**P0/P1 Incident Count (Quality-Attributed)**

Count of severity 0 and severity 1 incidents whose root cause traces to a code defect (as opposed to infrastructure failure, dependency failure, or capacity). The classification separates engineering quality failures from operational failures.

*Target:* Set based on your 90-day baseline; target 20% reduction quarter-over-quarter until you reach a stable floor.

*How to instrument:* Jeli or PagerDuty incident classification + root cause tag. Requires the postmortem process to consistently classify root cause.

---

### Domain 3: Reliability

Reliability metrics measure how the system behaves under failure conditions and whether operational load is sustainable for the team carrying it.

**Incident Frequency**

Total number of incidents per time period, segmented by severity tier. Frequency trending up signals systemic debt accumulating faster than the team addresses it.

*Segment by:* P0/P1 (customer-impacting), P2 (degraded service, users affected), P3 (internal/latent).

*How to instrument:* PagerDuty incident count by severity. Requires consistent severity classification at incident open time; post-hoc reclassification breaks the trend line.

**MTTR (see also Delivery domain)**

MTTR appears in both delivery and reliability because it measures different things depending on the context. In delivery, it measures response to deployment-caused failures. In reliability, it measures response to all production failures. Track both separately.

**On-Call Incident Load Per Engineer**

Average number of pages per on-call engineer per week, by severity tier. The metric tracks on-call sustainability directly and surfaces engineer burnout and attrition risk before either appears in HR data.

- Target: <3 P2+ pages per engineer per week on average
- Warning threshold: >6 P2+ pages per engineer per week (sustained over 4+ weeks)

*How to instrument:* PagerDuty on-call report → divide total pages by number of engineers in the rotation. Report the distribution alongside the average; one engineer absorbing 80% of pages while others see none reveals a structural problem the average hides.

**Mean Time Between Failures (MTBF) for Critical Services**

How long your most critical services run without a customer-impacting incident. For services with SLAs, MTBF is an input to the reliability covenant.

*How to instrument:* Calculated from PagerDuty or Jeli incident history. For each critical service, time between consecutive P0/P1 incidents. Track the trend; absolute MTBF varies too much with service profile to compare across the portfolio.

---

### Domain 4: Team Health

Team health metrics lead the lagging indicators in the other three domains. Delivery and reliability degrade after teams have been overloaded, undersupported, or operating with unclear ownership for weeks.

**Lead-to-Cycle Time Ratio**

Cycle time measures the duration of active work on a ticket. Lead time measures end-to-end time from creation to done. A high ratio (lead time >> cycle time) indicates work spends the majority of its time waiting: waiting for review, waiting for dependencies, waiting in a queue, waiting for a decision.

- Healthy ratio: Lead time < 3× cycle time
- Warning: Lead time > 5× cycle time

*How to instrument:* Jira time-in-status reports. LinearB and Jellyfish calculate this automatically from Jira + Git data. Cycle time = time in "In Progress"; lead time = time from "To Do" to "Done."

**Sprint Capacity on Unplanned Work**

Percentage of sprint capacity consumed by work not planned at sprint start: reactive incidents, urgent requests, context switches from leadership.

- Target: <25%
- Warning threshold: >40% (sustained over two or more sprints)

*How to instrument:* Jira sprint report: tickets added after sprint start versus at sprint start. Requires consistent sprint planning discipline. If your team does not use sprints, use a weekly capacity tracking mechanism instead.

**On-Call Load** (see Reliability domain; the same metric appears here as a team health signal)

**Voluntary Attrition (Engineering)**

Rolling 12-month voluntary attrition rate for engineering roles. Engineering attrition runs expensive: recruiting, onboarding, and knowledge transfer costs typically reach 0.5–1.5× annual salary per departure. High attrition correlates with on-call overload, poor manager effectiveness, and unclear career growth.

- Target: <12% annually
- Warning: >20% annually

*How to instrument:* HRIS data. Distinguish voluntary from involuntary; include contractors if they are a significant share of engineering capacity.

---

## Sample Scorecard Table Format

Use this format for QBR slides, board decks, and leadership reports. One table per domain. Apply color-coding (green/yellow/red) to the Trend column; the Current column stays uncolored. Improvement trajectory carries the narrative weight; absolute current value does not.

**Delivery**

| Metric | Target | Current | Trend | Owner |
|--------|--------|---------|-------|-------|
| Deployment frequency (prod) | ≥1/day | 3.2/week | ↑ | VP Eng |
| Lead time for change | <3 days | 4.1 days | ↓ | Eng Mgr, Platform |
| Change failure rate | <8% | 6.2% | ↔ | Eng Mgr, QA |
| MTTR (all incidents) | <2 hours | 1.8 hours | ↑ | SRE Lead |

**Quality**

| Metric | Target | Current | Trend | Owner |
|--------|--------|---------|-------|-------|
| SLO attainment (avg, all services) | ≥99.5% | 99.3% | ↓ | SRE Lead |
| Defect escape rate | <20% | 28% | ↔ | Eng Mgr, QA |
| P0/P1 incidents (code-attributed) | ≤2/month | 3/month | ↑ | VP Eng |

**Reliability**

| Metric | Target | Current | Trend | Owner |
|--------|--------|---------|-------|-------|
| P0/P1 incident count | ≤2/month | 3/month | ↑ | SRE Lead |
| On-call pages/engineer/week (P2+) | <3 | 4.8 | ↓ | Eng Mgr, on-call rotation |
| MTBF (critical services, days) | ≥30 days | 18 days | ↓ | SRE Lead |

**Team Health**

| Metric | Target | Current | Trend | Owner |
|--------|--------|---------|-------|-------|
| Lead-to-cycle time ratio | <3× | 4.1× | ↔ | Eng Mgr |
| Sprint capacity on unplanned work | <25% | 38% | ↓ | Eng Mgr |
| Engineering attrition (12-month) | <12% | 9% | ↑ | Director Eng |

---

## How to Set Targets

A common mistake in scorecard setup: adopting DORA industry benchmarks as targets before establishing a baseline. "Elite performer" thresholds carry aspirational weight; using them as Q1 targets for an org with no measurement history sets up the scorecard as a failure narrative from day one.

**The right sequence:**

1. **Instrument first.** Get 90 days of data before you set targets. You cannot set a credible target for deployment frequency if you do not know what your current deployment frequency is.

2. **Set 90-day improvement targets; industry benchmarks come later.** "We will improve deployment frequency from 3/week to 5/week" reads as a credible target. "We will be an elite performer by Q3" reads as a slogan.

3. **Anchor on the trajectory; absolute value follows.** A metric below benchmark but improving every quarter carries a stronger leadership story than one with a single benchmark hit and a flat curve since.

4. **Re-baseline annually.** As the org matures, benchmarks should tighten. Do not leave targets unchanged for more than 12 months — they become wallpaper.

5. **Define the instrumentation for each metric before the scorecard goes to leadership.** A metric with an undefined data source becomes a liability; the first time someone asks "how did you calculate that" and "we estimated" comes back, the scorecard loses credibility.

---

## Leadership Dashboard Narrative

A metric without context lands as a number on a slide; the narrative does the load-bearing work. At a QBR or board meeting, each metric needs two sentences of explanation: what it measures and what it means for the business.

**Deployment Frequency:**
"We ship to production an average of 3.2 times per week, up from 1.8 times per week six months ago. This means we can respond to user feedback and fix production issues faster — it also means our deployment process has become reliable enough to run more frequently without increasing risk."

**SLO Attainment:**
"Our average SLO attainment across services is 99.3%, slightly below our 99.5% target. The underperformance is concentrated in two services — our search API and our notification pipeline — and we have a roadmap item in H2 to address the root cause for each."

**On-Call Load:**
"Our engineers are averaging 4.8 pages per on-call week at P2 and above, above our 3-page target. Sustained above 6, this becomes a retention risk. We are investing in alert rationalization and service reliability work in Q3 specifically to bring this number down — it is one of the primary quality-of-life signals we track."

**Defect Escape Rate:**
"28% of bugs are first detected in production, above our 20% target. This is a test coverage and staging fidelity problem. We are adding contract testing between two high-failure service pairs and expanding staging data representativeness as a Q3 priority."

---

## What NOT to Include

A leadership scorecard with too many metrics becomes harder to act on than one with too few. Every metric on the scorecard should be actionable: if it drops, a known owner has a known response.

**Metrics to exclude:**

- **Lines of code / commit count:** Captures activity; output stays unmeasured. By this metric, the team that wrote 10,000 lines outranks the team that later refactored those lines into 1,000 cleaner ones — an inversion of the actual value created.
- **Story points completed:** Velocity serves as a sprint planning tool; performance evaluation breaks the metric. Story point inflation, scope change, and estimation variance make cross-team and cross-quarter comparisons meaningless.
- **PR count:** Same problem as commit count. Optimizes for small PRs; impactful work stays invisible.
- **Test coverage percentage (as a standalone metric):** 80% coverage with tests asserting nothing meaningful loses to 60% coverage with tests catching real regressions. Track defect escape rate instead.
- **Uptime percentage (without SLO context):** 99.9% uptime sounds reassuring but says nothing about whether commitments were met or whether the 0.1% downtime landed at peak user traffic.
- **"Team happiness" survey scores as a primary metric:** Engagement surveys feed a Director's understanding of team health; they belong in manager-level conversations. In leadership scorecards, the audience lacks the context to interpret them fairly.

---

## Cadence

Different metrics belong at different review cadences. Reviewing everything weekly creates noise; reviewing everything quarterly means you miss slow degradation.

**Weekly (engineering leadership)**

- On-call incident count and severity distribution (PagerDuty weekly digest)
- Active SLO breaches
- Deployment frequency (trailing 7 days)
- Sprint capacity on unplanned work (at sprint review)

**Monthly (Director/VP-level review)**

- Full scorecard review: all four domains
- Trend analysis: is each metric improving, stable, or declining?
- Owner check-in: every metric has an owner accountable for the trend
- Update targets if a metric has been consistently above/below target for 60+ days

**Quarterly (QBR / exec / board)**

- Scorecard summary: current vs. target vs. prior quarter
- Narrative for each domain: what happened, what we did about it, what comes next
- Investment asks that are supported by scorecard data (e.g., "on-call load is at 4.8 pages/week; to get to target we need 2 additional SRE headcount in H2")
- Rolling 12-month trend on attrition and team health

**Annual (engineering planning)**

- Re-baseline all targets
- Review instrumentation: is each metric still measuring what it should?
- Identify new metrics if the org has matured into a domain that was not previously measured
- Archive metrics that are no longer decision-relevant

---

## Tooling Reference

| Domain | Recommended Tool | Alternative | What to instrument |
|--------|-----------------|-------------|--------------------|
| Deployment frequency | LinearB, Jellyfish, Datadog CI | GitHub Actions workflow analytics | Production deploy events by team |
| Lead time | LinearB, Jellyfish | Jira time-in-status | First commit to production deploy |
| Change failure rate | PagerDuty + Jeli | Manual Jira classification | Incidents tagged to causative deploy |
| MTTR | Jeli | PagerDuty incident duration | Incident open to service restored |
| SLO attainment | Datadog SLOs | New Relic | SLO burn rate, rolling 30d |
| Defect escape rate | Jira bug fields | Linear | Bug environment at first detection |
| On-call load | PagerDuty | OpsGenie | Pages per engineer per week |
| Cycle/lead time ratio | LinearB, Jira | Swarmia | Time in status per ticket type |
| Attrition | HRIS (Workday, BambooHR) | HR reporting | Voluntary departure by role |

## Further Reading

| Topic | Resource |
|---|---|
| DORA metrics | Forsgren, Humble & Kim, *Accelerate* (IT Revolution Press, 2018) |
| SRE observability | Google SRE Book — [Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/) |
