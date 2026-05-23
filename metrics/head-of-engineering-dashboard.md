# Head of Engineering Operational Dashboard

## Leadership Context

The [Engineering Health Scorecard](engineering-health-scorecard.md) functions as a communication artifact — it answers the questions a non-technical executive will ask about engineering health. The operational dashboard described here serves a different function: the weekly instrument the Head of Engineering uses to know whether anything is silently on fire before it becomes someone else's problem.

The distinction matters because the two artifacts optimize for different things. A scorecard targets credibility with an audience holding no context; it must be curated, narrativized, and defensible. A dashboard targets speed for the one person holding full context; it must surface anomalies immediately, require no manual curation, and answer the Monday morning question: *is anything wrong I do not already know about?*

If everything is green, the dashboard should take 90 seconds to review. The value shows up in the weeks when something is amber or red and you already know where to look.

## Background and Motivation

> **Author's note:** I developed this framework during the Jellyfish rollout and engineering health reporting work at ActBlue Technical Services (2024–2025), across a six-team platform directorate. The distinction between "what I show the CTO" and "what I watch every week" became clear after the first QBR cycle: the scorecard worked for quarterly storytelling but ran too lagging and too curated to catch in-week drift. The operational dashboard fills the gap.

---

## When to Use This

| Trigger | What the dashboard provides |
|---------|-----------------------------|
| Weekly leadership ritual | Answers "is anything off?" before the weekly team sync |
| First 90 days in a new HoE role | Fastest way to build a baseline and spot which signals are already in the red |
| After a major incident or miss | Ongoing monitoring to confirm recovery is real and sustained, beyond what the post-incident review declared |
| Scaling the org | People and capacity signals degrade before delivery signals do — catch it early |
| Pre-planning / pre-QBR | Raw material for identifying the two or three things worth discussing with leadership |

The dashboard does not function as a management tool for engineers. Do not share it directly with ICs or use it in performance conversations; it lacks the context to be fair at that resolution. The dashboard belongs to the person who holds the org's risk.

---

## The Five Domains

### Domain 1: Delivery Pulse

**Question it answers:** Are we shipping, and is the pipeline healthy?

Delivery carries heavy instrumentation and well-trodden patterns. The risk: aggregate numbers mask team-level outliers.

**Deployment frequency (per team, trailing 7 days)**

The org-level aggregate hides the team going three weeks without shipping. Track per-team; per-org rolls up too much to catch the outlier. One team at zero deployments for two consecutive weeks warrants a conversation; the signal rarely shows up in org-level DORA reporting.

- Watch for: a team that was shipping twice a week suddenly shipping zero
- Common cause: unresolved merge conflicts, a release freeze nobody communicated, a team absorbed into an incident

**Lead time for change (trailing 14 days)**

Track the P90 alongside the median. A P90 two or three times the median means a class of work has gotten stuck: large PRs, dependency-blocked tickets, or tickets waiting on a specific reviewer.

- Watch for: P90 climbing while median stays flat
- Common cause: a small number of long-running branches, a bottleneck reviewer, or tickets classified as "in progress" that have been idle for a week

**PR cycle time (open to merge)**

Distinct from lead time. This metric captures the code review and merge window. A PR sitting three days before its first review signals team health as much as delivery; the team is usually heads-down on incidents, on-call, or a high-priority fire, and code review has been deprioritized.

- Target: <24 hours for P90 on standard PRs (not draft/WIP)
- Watch for: specific engineers whose PRs consistently sit longer; the pattern points to a review bottleneck or an interpersonal dynamic

**Change failure rate (trailing 30 days, per team)**

Same as the scorecard metric but tracked at team resolution. An org-level CFR of 6% can hide one team at 25% and four teams at 2%. The team at 25% has a testing or deployment process problem the org will not solve by waiting.

---

### Domain 2: Quality and Reliability

**Question it answers:** Is what we shipped holding up?

**SLO burn rate by service (rolling 30 days)**

More useful than point-in-time SLO attainment because burn rate tells you whether you are on a trajectory to breach; current attainment alone reports only what has already broken. A service burning 3× its error budget in week 1 of a 30-day window will breach even if its current attainment looks fine.

- Instrument: Datadog SLO burn rate alerts, or equivalent in New Relic / Honeycomb
- Watch for: any service burning >2× budget; more than one service in degraded state simultaneously

**P0/P1 incident count and MTTR (trailing 30 days, week-over-week trend)**

Track the count, then read the trend. Two P1s per month reads fine after four last month and two the month before. Two P1s per month reads alarming after zero for the prior two quarters.

- Watch for: MTTR trending up over multiple weeks (recovery slowing instead of accelerating); incident count stalling after a remediation effort (the fix landed as cosmetic, the root cause untouched)

**Escaped defect rate (monthly refresh)**

The dashboard refreshes this on a monthly cadence; weekly refresh outruns the Jira classification discipline the metric depends on. Flag it at the dashboard level when the monthly update shows it crossing 30%; at that threshold, testing and staging fidelity have materially broken down.

**Open critical vulnerabilities (count, aging)**

CVEs at critical/high severity with no assigned owner, or with an owner but no action in 14+ days. This metric stays invisible until it surfaces as an incident. One line on the dashboard: count and average age. A growing count or an average age above 30 days warrants direct attention.

- Instrument: Snyk, Dependabot, or your security scanning tool of choice; the dashboard pulls only the count and age, leaving the full report to the tool

---

### Domain 3: People and Org Health

**Question it answers:** Is the team sustainable?

Most HoE dashboards omit this domain, and the omission causes the expensive surprises. People signals lead delivery signals; the lag typically runs weeks to months. By the time delivery metrics degrade, the attrition or overload behind it has usually been visible in the people data for months.

**Capacity vs. plan (headcount and open requisitions)**

Actual engineers on staff versus planned headcount, broken down by team. Include contractor ratio if contractors carry production load. Also: open requisitions and their age. A req open for 90 days without a hire represents a present capacity gap; treating it as a future one understates the load already on the team.

- Watch for: teams operating at >20% below planned headcount for more than 60 days; delivery risk accumulates silently before it shows up in DORA metrics

**On-call incident load per engineer (P2+, per week, trailing 4 weeks)**

Already in the scorecard but worth tracking at weekly cadence on the operational dashboard. When this crosses 6 pages/engineer/week sustained for more than 3 weeks, attrition risk becomes measurable. The distribution matters as much as the average; one engineer absorbing 80% of pages while others see none reveals a structural problem the average hides.

**Unplanned leave (trailing 30 days)**

Beyond sick days: abrupt leave patterns correlated with burnout or disengagement. Most HRIS systems can surface this. A team where 3 of 6 engineers took unplanned leave in the same two-week window warrants a check-in conversation with the manager; the framing carries no accusation.

**1:1 completion rate by manager (rolling 4 weeks)**

Managers who stop having regular 1:1s are usually underwater: on-call, context-switching, or managing up too much. A manager's 1:1 cadence offers a low-cost leading indicator of team cohesion. Most people-management tools (Lattice, Culture Amp, even calendar analytics) can surface this.

- Target: >90% of scheduled 1:1s held over a rolling 4-week window
- Watch for: a manager whose 1:1 rate drops below 70% for two consecutive weeks

**Engineers with no PTO logged in 90+ days**

Systematically invisible until someone burns out or resigns. Run a query against HRIS at the start of each quarter. The signal belongs to health tracking; performance tracking misreads it. Flag individuals to their manager for a check-in conversation.

---

### Domain 4: Strategy Execution

**Question it answers:** Are we building the right things and making progress?

**OKR progress by team (% complete at current week of quarter vs. expected)**

The dashboard signal: each team's progress as a percentage of expected progress at this point in the quarter. A team 40% complete at week 8 of a 13-week quarter runs behind pace; a team 90% complete at week 4 either sand-bagged or stands ready to declare victory on something insufficiently ambitious.

- Watch for: two or more teams simultaneously behind pace (suggests a cross-cutting dependency or a planning assumption that was wrong); a team consistently hitting 100% before week 10 (goals need to be reset)

**Distraction ratio (% of sprint capacity on unplanned work, trailing 4 weeks)**

Unplanned work carries no inherent stigma; incident response, urgent stakeholder requests, and production support count as real work. The ratio carries the signal. When more than 40% of sprint capacity goes reactive for two or more consecutive sprints, the team has effectively lost the ability to execute on planned strategy.

- Target: <25% unplanned work as a sustained average
- Warning: >40% for two consecutive sprints; immediate attention: >60% for any single sprint

**Dependency blockers (count, owner, age)**

Cross-team dependencies blocking work. Track count, which team is blocked, which team is the blocker, and how long the block has been open. A blocker open three weeks without escalation has slipped from managed to tolerated.

- Instrument: Jira blocker links with a dependency-tracking label, or a dedicated RAID log
- Watch for: the same team showing up repeatedly as a blocker; the pattern points to a structural capacity or prioritization issue beyond the dependency at hand

**Initiative milestone health (RAG by initiative)**

For the two to four major engineering initiatives in flight at any given time: on track, at risk, or off track? Not the detailed project plan; the RAG status and a one-line reason. The dashboard entry prompts a check of the detailed tracking when something turns amber or red; the dashboard does not replace the detail.

---

### Domain 5: Cost and Capacity

**Question it answers:** Are we spending efficiently and do we have the capacity we think we have?

**Cloud spend vs. budget (trailing 30 days, by service/team)**

Not a FinOps replacement; FinOps requires dedicated tooling and expertise. The dashboard signal: monthly-budget tracking and outlier identification by team or service. A service jumping 40% in cloud spend in a single week either handles significantly more traffic (good, but check if it was expected) or carries a misconfiguration (infrastructure leak, forgotten test environment, cost regression from a code change).

- Instrument: AWS Cost Explorer / GCP Billing / Azure Cost Management, with tag-based attribution by team or service
- Watch for: week-over-week spend increases >20% on any service; total spend trending ahead of monthly budget by week 2 of the month

**Cost per deployment (trailing 30 days)**

Unit economics for the delivery pipeline. Divide total CI/CD infrastructure cost by deployment count. The metric catches pipeline inefficiencies aggregate spend does not surface: long build times, large artifact sizes, redundant test runs. Cost per deployment rising while deployment frequency stays flat means something in the pipeline has gotten more expensive.

**Open headcount reqs and time-to-fill**

Already in Domain 3 as a capacity signal; it appears here as a cost signal because unfilled reqs typically mean the work has been absorbed by existing staff (overload risk) or deferred (strategic risk). Time-to-fill above 60 days on a critical-path hire flags the recruiting pipeline as well as the hiring manager.

---

## Dashboard Format

The dashboard belongs in a system refreshing automatically; a slide deck or a manually updated spreadsheet decays within weeks. Grafana, Datadog dashboards, or a simple internal tool all work. The format:

**Top-level view:** Five rows, one per domain. Each row shows a RAG status (green/amber/red) and a one-line summary. If all five are green, the review takes 90 seconds.

**Drill-down per domain:** Clicking into an amber or red domain shows the underlying metrics for that domain — the specific team, service, or signal that triggered the status.

**Weekly diff:** Show the delta from last week alongside the current value. A metric amber but improving opens a different conversation from one amber and declining for four weeks.

**Owner per metric:** Every metric has a named owner. The owner carries responsibility for the trend; reporting the number alone does not discharge the role.

---

## RAG Status Definitions

Consistent RAG definitions prevent the dashboard from becoming a negotiation. For metrics without a clean percentage target, substitute the team-specific thresholds defined in each domain's "Watch for" notes.

| Status | Meaning | Default response |
|--------|---------|-----------------|
| Green | At target or within 10% of target, trend flat or improving | No action required |
| Amber | >10% off target, or at target but trend declining for 2+ weeks | Owner is accountable for a plan within one week |
| Red | >25% off target, or amber for 2+ consecutive weeks without an improving trend | Escalation to HoE for direct attention; plan due in 48 hours |

A common dashboard failure mode: treating amber as "informational." Amber without an owner response becomes red; red without HoE attention becomes an incident, a miss, or an attrition event.

---

## What to Exclude

The same exclusion principles from the scorecard apply here, with one addition:

**Individual engineer metrics.** The dashboard operates at team and system resolution, never at individual contributor resolution. Monitoring individual output here lands as methodologically unsound (the metrics lack the fidelity) and as corrosive to the trust making the people signals useful. Evaluation of an individual's output belongs in a performance conversation with their manager; the dashboard does not have the resolution for that work.

**Metrics requiring manual input.** If a metric cannot be pulled from a system, it will not stay current. A manually updated dashboard decays within two weeks; the cost of keeping it fresh exceeds the value it provides. Every metric on the operational dashboard must have an automated data source. Where automated sources are unavailable for people-domain metrics, start with the signals you can instrument and add the rest as tooling matures; a partially automated dashboard outperforms a fully manual one.

**Engagement survey scores.** Useful in a quarterly HR review; too noisy and infrequent for a weekly operational signal.

---

## Relationship to the Engineering Health Scorecard

The scorecard and the dashboard share some underlying data sources but serve different purposes and audiences.

| | Operational Dashboard | Engineering Health Scorecard |
|---|---|---|
| **Audience** | Head of Engineering | VP Eng, CTO, board, M&A diligence |
| **Cadence** | Weekly (reviewed Monday) | Monthly (full review); Quarterly (narrative) |
| **Resolution** | Per-team, per-service | Org-level with domain rollups |
| **Format** | RAG + drill-down | Narrative + tables |
| **Purpose** | Early warning; catch drift before it becomes a miss | Credibility artifact; convert org performance into a defensible story |
| **Maintenance** | Automated; no curation | Requires owner narrative per metric |

The scorecard is built from the dashboard. When a domain is amber or red on the dashboard for 4+ weeks, it surfaces as a scorecard item at the monthly or quarterly cadence with a narrative attached. The dashboard catches it; the scorecard tells the story.

---

## Further Reading

| Topic | Resource |
|---|---|
| DORA metrics and delivery performance | Forsgren, Humble & Kim, *Accelerate* (IT Revolution Press, 2018) |
| SLO-based reliability | Google SRE Book — [Service Level Objectives](https://sre.google/sre-book/service-level-objectives/) |
| Engineering org health and team sustainability | Skelton & Pais, *Team Topologies* (IT Revolution Press, 2019) |
