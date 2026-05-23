# RAID Log and Dependency Tracking

## Leadership Context

The RAID log is the artifact that separates programs that surface problems early from programs that discover them when they are already blocking. Every multi-team program accumulates risks, makes assumptions, generates issues, and creates dependencies — the question is whether they are tracked in a structured format that enables proactive management, or scattered across Slack threads and meeting notes where they become invisible until they cause damage. A maintained RAID log is also the primary input for board-level or executive program reporting: when a VP asks "what are our top risks heading into Q4?", the RAID log should produce the answer in minutes, without requiring a frantic synthesis session the night before.

## Background and Motivation

This framework was developed from the dependency tracking work on the PCI environment deprecation and payment processor migration programs at ActBlue Technical Services (2022–2025). I owned cross-team coordination across 4–5 engineering teams running against both external audit deadlines and internal milestones. The RAID format here is the one that surfaced dependencies early enough to manage them — not after they were blocking.

## When to Use This

Apply RAID tracking from the day the program brief is signed through program closure when:

- The program has external dependencies (another team, a vendor, a regulatory body)
- Multiple workstreams must coordinate to deliver a shared milestone
- A deliverable in one workstream is required as input to another team's work
- An executive or board-level stakeholder receives regular program status

Do not wait for a problem to materialize before creating the RAID log. The log is most valuable when it is started during program planning, before execution begins.

---

## RAID Defined

RAID stands for Risks, Assumptions, Issues, Dependencies. Each category has a distinct meaning; putting items in the wrong category is a recurring RAID log failure because it obscures what action is needed.

### Risks

A risk is a condition that may occur in the future and would negatively affect the program if it does. It has not happened yet. The appropriate response is mitigation — reducing the likelihood or impact before the risk materializes.

**Examples:**
- The fraud detection service may have undocumented stateful behavior that does not transfer cleanly to Kubernetes. (Not confirmed — needs investigation.)
- A hiring freeze may reduce platform team capacity below what the milestone schedule requires.
- The third-party compliance vendor may not be available to complete their review before the audit window.

**Not risks:** The fraud detection service has a bug that breaks the Kubernetes integration (this happened — it is an issue). The platform team has already confirmed they are understaffed (this is a known constraint, not a future possibility — it is an issue).

### Assumptions

An assumption is something the program plan takes as true that has not been explicitly confirmed. Every program plan is built on assumptions. The RAID log makes them explicit so they can be validated on a schedule, not discovered after they turn out to be wrong.

**Examples:**
- We are assuming the compliance vendor can complete their review in 3 weeks based on past engagements.
- We are assuming the fraud detection service migration can reuse the payment service migration tooling.
- We are assuming the SOC 2 audit scope does not expand to cover the admin tooling, which is out of program scope.

**Why assumptions matter:** When an assumption fails, the program impact can be as large as a high-severity risk. The difference is that an assumption failure is often discovered later — because no one was actively checking whether the assumption was still true. The RAID log assigns an owner and a validation date to every assumption. If the validation date passes without confirmation, the assumption escalates to a risk.

### Issues

An issue is a problem that has already occurred and is affecting the program now. It requires immediate action and active management, not monitoring.

**Examples:**
- The fraud detection service has a bug that breaks Kubernetes networking. Engineering lead is investigating. Blocker for the April 1 staging milestone.
- The compliance vendor review is delayed by 2 weeks due to capacity constraints on their end. May push the audit-readiness milestone.
- The platform team environment configuration has a permission error that is preventing the payments team from running integration tests.

**The difference between an issue and a risk that materialized:** When a risk materializes, close the risk entry and open an issue entry. Do not leave a closed risk as the only record of an active problem — the issue tracker is where active problems live.

### Dependencies

A dependency is a deliverable that one workstream needs from another team, vendor, or external party in order to proceed. Dependencies are different from risks because they are definite — Team A will need output from Team B on a specific date. The RAID log tracks whether that handoff is on schedule.

**Examples:**
- Payments team needs Kubernetes environment configuration from Platform team by March 1 to begin integration testing.
- Security team needs the completed threat model from each migrated service before they can scope the pen test.
- Compliance certification requires legal sign-off on data processing agreements by April 15.

---

## Risk Register Format

One row per risk. Maintain in a shared document (Confluence, Notion, Google Sheets) with edit access for all program leads.

| ID | Description | Likelihood | Impact | Score | Owner | Mitigation | Review Date | Status |
|----|-------------|-----------|--------|-------|-------|------------|-------------|--------|
| R-001 | Fraud detection service has undocumented stateful behavior that may not migrate cleanly to Kubernetes | M | H | 6 | Marcus Webb | Spike planned Week 2 to map all stateful dependencies; go/no-go gate before migration begins | Feb 15 | Open |
| R-002 | Hiring freeze reduces platform team capacity below plan | L | M | 3 | Sarah Chen | Two contractor options pre-identified; decision gate at Week 4 if freeze is confirmed | Feb 28 | Open |
| R-003 | Compliance vendor unavailable before audit window | L | H | 4 | Priya Agarwal | Contract with alternative vendor signed; Priya to confirm scope compatibility by Feb 1 | Feb 1 | Open |

**Likelihood/Impact scale:**
- H = High (likely to occur / would significantly delay or block the program)
- M = Medium (possible / would cause material schedule or scope impact)
- L = Low (unlikely / manageable without significant impact)

**Score** = Likelihood × Impact (H=3, M=2, L=1). Use score to prioritize weekly review attention. Score 6–9: review every week. Score 3–5: review every two weeks. Score 1–2: review monthly.

**Status:** Open, Monitoring (mitigation in place, still tracking), Closed (risk passed or no longer applicable), Escalated (risk materialized — open Issue entry).

### Risk Review Cadence

- Score 6–9 risks: discussed in every weekly program sync
- Score 3–5 risks: reviewed at every other sync; owner confirms mitigation is on track
- Score 1–2 risks: reviewed monthly; owner confirms no change in likelihood or impact
- Any risk that increases in likelihood or impact is reviewed immediately, not at the next scheduled interval

---

## Assumption Log

| ID | Assumption | Owner | Validation Method | Validation Date | Status |
|----|-----------|-------|-------------------|-----------------|--------|
| A-001 | Compliance vendor can complete review in 3 weeks based on prior engagement | Priya Agarwal | Confirm scope and timeline with vendor in kickoff call | Jan 20 | Pending |
| A-002 | Fraud detection service migration can reuse payment service tooling without modification | Marcus Webb | Engineering spike in Week 2 | Feb 10 | Pending |
| A-003 | SOC 2 audit scope does not expand to admin tooling | Priya Agarwal | Written confirmation from audit firm | Jan 31 | Confirmed |

**What to do when an assumption fails:**

1. Close the assumption entry with status "Failed."
2. Open a risk entry (if the failure creates future uncertainty) or an issue entry (if the failure is already impacting the program).
3. Notify program leads and update the milestone forecast if the assumption was load-bearing for a date.
4. Do not absorb assumption failures silently into team-level work. If an assumption fails and it only becomes visible when a milestone slips, the RAID log failed its purpose.

---

## Issue Tracker

| ID | Description | Date Identified | Severity | Owner | Current Action | Target Resolution | Status |
|----|-------------|-----------------|----------|-------|----------------|-------------------|--------|
| I-001 | Fraud detection Kubernetes networking bug — integration tests failing | Feb 12 | High | Marcus Webb | Engineering debugging as of Feb 12; escalated to staff architect | Feb 19 | Open |
| I-002 | Compliance vendor 2-week delay | Feb 8 | Medium | Priya Agarwal | Re-sequencing internal review tasks to absorb partial delay; evaluating alternative vendor for remaining scope | Feb 22 | Open |

**Severity scale:**
- High: Blocking a milestone or a critical dependency handoff
- Medium: Not blocking today but will become a blocker within 2 weeks without intervention
- Low: Degrading team velocity but workarounds exist; not milestone-threatening

**Escalation rule:** Any High severity issue that has no clear resolution path within 48 hours is escalated to the program executive sponsor. Medium issues unresolved after one week are reviewed in the weekly sync with an escalation decision made at that point.

---

## Dependency Matrix

The dependency matrix is a team × team table that makes all inter-team handoffs visible in one view. Unlike the RAID log issue list, the dependency matrix is organized around the structure of the program — who needs what from whom.

### Format

**Rows:** Teams that consume deliverables (the team that is waiting).
**Columns:** Teams that produce deliverables (the team delivering).
**Cells:** Description of the dependency, date needed, and current status.

|  | **Platform Team** | **Security Team** | **Legal/Compliance** | **Vendor (Compliance)** |
|--|-------------------|-------------------|----------------------|------------------------|
| **Payments Team** | K8s environment config by Mar 1 — 🟡 at risk (env config scope expanding) | Threat model review sign-off by Mar 15 — 🟢 on track | Data processing agreement by Apr 15 — 🟢 on track | N/A |
| **Security Team** | Network policy templates by Feb 20 — 🟢 on track | N/A | Audit scope confirmation by Jan 31 — ✅ done | Pen test scheduling by Mar 1 — 🔴 vendor at capacity |
| **Platform Team** | N/A | Security review of K8s configuration by Mar 1 — 🟢 on track | N/A | N/A |

**Status codes:**
- ✅ Done — deliverable received and confirmed
- 🟢 On track — delivery date confirmed, work in progress
- 🟡 At risk — delivery is possible but a named concern exists; mitigation in place
- 🔴 Blocking — delivery will miss date without intervention; escalation required

Update the matrix before every weekly sync. Stale status codes are the primary signal that the RAID process is being treated as theater.

---

## Visual Dependency Map

The matrix provides detail. The visual map provides a communication artifact for stakeholders who need the overview.

### Swimlane Approach

Use a swimlane diagram when the primary concern is sequence — understanding what must happen before what.

Each team gets a lane. Dependencies are drawn as arrows between lanes, labeled with the deliverable and due date. Critical path is highlighted. This format works well for linear programs (work flows left to right) and for board presentations.

**Tool:** Lucidchart, Miro, or draw.io. Export as PNG for embedding in program status reports.

**When to use swimlane:** The program has a clear sequential structure and the primary risk is schedule delay. The swimlane makes the critical path visible.

### Gantt-Style Dependency View

Use a Gantt-style view when teams need to see their workstreams in parallel and understand how their work lands relative to others.

Each team's milestones appear as bars on a shared timeline. Dependency arrows connect the bars. The critical path is the chain of dependencies with the least slack.

**Tool:** Notion timeline, Linear milestones, Asana Gantt, or a Google Sheet with conditional formatting.

**When to use Gantt:** The program has parallel workstreams and the primary risk is teams not understanding how their timeline affects others. The Gantt makes slack (or the absence of it) visible.

**Which to use:** Choose swimlane for exec communication; Gantt for team coordination. For a complex program, maintain both — they serve different audiences.

---

## Weekly RAID Review Ritual

Build RAID review into the weekly program sync as a standing 5-minute segment. The ritual should take no more than 5 minutes if the RAID log was updated before the meeting.

**Review order:**
1. Any new risks added since last week? Owner, mitigation, score?
2. Any assumptions due for validation this week? Confirmed or escalated?
3. Any issues that changed severity since last week? Any issues that resolved?
4. Any dependencies that changed status since last week? Any new 🔴 entries?

**What to update in the RAID log weekly (owner responsibility, not TPM):**
- Risk status and mitigation progress
- Assumption validation results
- Issue current action and target resolution
- Dependency matrix status codes

**What the TPM does:** Reviews the log before the sync, identifies items that require group discussion or escalation, and surfaces those in the meeting. The TPM is not the person updating the log — the workstream lead is. A RAID log updated only by the TPM is a RAID log that is out of date within two weeks.

---

## RAID Health Signals

A RAID log that is being actively maintained looks different from one that is being performed as process theater. Use these signals to assess health:

### Signals of a Live RAID Log

- Issue entries have been updated in the last 7 days
- At least one risk has been closed (resolved or materialized into an issue) in the last two weeks
- Assumption validation dates are being met or explicitly extended with a reason
- Dependency matrix status codes changed from the previous week (either improving or deteriorating — stasis is a warning sign)
- Program leads can explain the top 3 open risks without looking at the document

### Signals of a Theater RAID Log

- The log was last updated at program kickoff and has not changed since
- All risks are "Low" likelihood — no one is surfacing real concerns
- The issue list is empty while the program is in active execution — real programs generate issues
- Dependency matrix has all 🟢 entries for more than 3 consecutive weeks during a complex integration phase — this is statistically unlikely if the program is real
- No one can tell you what R-003 is without opening the document

**What to do when the RAID log has gone stale:** Do not reboot the log in a meeting. Call each workstream lead individually, ask them to walk you through their current status from memory, and update the log based on what you learn. The stale log is a symptom — the cause is usually that leads feel the log is a reporting artifact, not a decision-support tool. Fix the perception; updating the document alone will not hold.

---

## RAID Log Maintenance at Scale

For programs with 5+ workstreams, the RAID log becomes unwieldy if all items are maintained in a flat list. Apply these conventions:

**Archive resolved items.** Closed risks, confirmed assumptions, and resolved issues move to an "Archive" tab or section. The active log stays short — target fewer than 20 active items total. Active logs over 30 items are not being closed aggressively enough.

**Tag by program phase.** Add a "Phase" column (Planning, Execution, Integration, Launch, Post-Launch). Filter by phase to understand what is relevant now vs. what is upcoming.

**Separate vendor/external dependencies.** External dependencies have different management paths — you cannot directly resolve them. Create a dedicated section or tab for vendor and third-party dependencies and manage them through a relationship owner; the standard workstream lead path does not fit.

**Link to source systems.** Where a risk or issue has a corresponding Jira ticket, engineering incident, or legal review document, link directly from the RAID log entry. This reduces the double-maintenance burden and makes it easier for leads to update status where they are already working.

## Further Reading

| Topic | Resource |
|---|---|
| RAID management methodology | PMI, *PMBOK Guide*, 7th ed. (PMI, 2021) |
| Risk and team dynamics | DeMarco & Lister, *Peopleware* (Dorset House, 1987) |
