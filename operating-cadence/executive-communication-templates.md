# Executive Communication Templates

## Leadership Context

An engineering leader's ability to communicate upward acts as a direct multiplier on the team's impact. If the CTO, CEO, or board cannot quickly understand what engineering has delivered, what it needs, and what risks it manages, they will make resourcing and priority decisions with incomplete information; engineering will consistently lose those decisions to functions communicating better. This playbook covers the written artifacts engineering leaders produce for executive and board audiences: the formats, the principles, and the failure modes to avoid. Getting these right does no polish work; it builds the executive trust translating into headcount, runway, and the ability to make consequential technical bets.

## Background and Motivation

These templates draw on repeated upward communication at ActBlue Technical Services (2022–2025): presenting roadmaps and program status to the VP of Engineering and Head of Product, presenting the PCI deprecation business case to Legal, Finance, and Product leadership, and making headcount and investment requests through multiple planning cycles. The formats here produced alignment in practice; theoretical approximations of good communication do not appear.

## When to Use This

- Preparing for a QBR, board presentation, or investor due diligence session where engineering will have a dedicated slot
- After a significant incident and leadership needs an executive summary before the all-hands debrief
- When requesting headcount, tooling budget, or a major cross-functional commitment
- When a new engineering leader is learning what "writing upward" means in practice

---

## Principles for Writing Upward

Before any template: the principles that make executive communication work.

### Lead With the Ask

Executive audiences read under time pressure. The load-bearing sentence in any executive communication is the first one. Burying the ask in the third paragraph after two paragraphs of context means the ask will be missed.

**Wrong:**
> "In Q3, the payments team made significant progress on the checkout reliability initiative. We resolved 14 of 18 known issues in the payment retry logic and improved error detection by adding distributed tracing to the payments service. As a result of this work, and in light of the upcoming Q4 holiday traffic projections, we believe we need to add two engineers to the payments team."

**Right:**
> "We need to add two engineers to the payments team before Q4 to meet holiday traffic reliability targets. Here is the context."

The context is still there. The ask is not buried.

### Quantify Impact

Every claim in an executive communication should be quantifiable or should say explicitly why it cannot be quantified. Executives reading "significant improvement" or "faster response times" without numbers cannot make resource decisions on those statements.

**Wrong:** "We significantly reduced checkout errors this quarter."
**Right:** "Checkout error rate dropped from 2.3% to 0.8% this quarter, which is estimated to recover approximately $180K in lost revenue per quarter at current conversion rates."

If the business impact remains unknown, say so and give the engineering metric: "We reduced checkout error rate from 2.3% to 0.8%. We have not yet quantified the revenue impact."

### Distinguish Diagnosis From Recommendation

Executive audiences need to know the difference between "here is what is happening" and "here is what we recommend doing about it." When these get conflated, executives cannot tell whether they face an approval request, an FYI, or a judgment call.

Format:
1. What is happening (diagnosis)
2. What it means for the business (impact)
3. What we recommend (recommendation)
4. What we need from you (ask)

### Manage Risk Explicitly

Engineering leaders surfacing risks only after they have materialized lose executive trust quickly. Executive communication should include an honest risk register; the purpose runs to demonstrating leadership is thinking ahead and managing proactively, never to alarm.

Risk language that signals credibility: "We believe X is unlikely but are monitoring Y as an early indicator. If Y triggers, we will do Z."

Risk language that signals loss of control: "We are working through some challenges in the infrastructure layer."

---

## Engineering QBR Structure

**Audience:** Executive team (CTO, CPO, CEO, CFO depending on org)
**Format:** 60–90 minute meeting with written pre-read distributed 48 hours in advance
**Length:** Pre-read: 4–6 pages. Meeting slides: 15–20 slides maximum.

### Section 1: Health Metrics (One Page)

A dashboard view of the 4–6 metrics representing engineering health. Each metric should include: current value, prior quarter value, trend direction, and a one-sentence interpretation.

**Template:**

| Metric | Q3 Actual | Q2 Actual | Trend | Signal |
|---|---|---|---|---|
| Deployment frequency | 12/week | 8/week | ↑ | On track; target is 15/week by Q4 |
| Change failure rate | 4.2% | 6.8% | ↓ | Improving; industry benchmark is <5% |
| Mean time to restore (MTTR) | 38 min | 2.1 hours | ↓ | On track vs. Q3 target of <45 min |
| P0 incidents | 2 | 4 | ↓ | Both Q3 incidents postmortem'd; fixes deployed |
| Open critical bugs | 7 | 12 | ↓ | 3 require product decision before close |
| On-call hours/engineer/week | 2.1 | 3.4 | ↓ | Acceptable; watch if any team exceeds 4 |

**What to include:** Metrics where the number alone tells the story.
**What to exclude:** Metrics that require engineering expertise to interpret. Save those for the architecture review.

---

### Section 2: Delivery Recap (One Page)

What engineering committed to in Q3 and what shipped. This section requires honesty. Executives receiving only good news from engineering will stop trusting the signal.

**Format:**

**Delivered as committed:**
- [Initiative name]: [One sentence on what was delivered and why it matters]. [Business metric moved, if applicable.]

**Delivered, modified scope:**
- [Initiative name]: [One sentence on what was originally committed, what changed, and why]. [Business metric moved, if applicable.]

**Not delivered:**
- [Initiative name]: [One sentence on why this did not ship]. [What the consequence is]. [When it will ship, or why it was deprioritized].

**Example (not delivered entry):**
> Fraud detection model v2: Deprioritized in week 6 when the data team identified a data quality issue that would invalidate the training data. Rescheduled for Q4 with a new data pipeline prerequisite. Current fraud loss rate is $22K/month; v2 is estimated to reduce this by 60%.

---

### Section 3: Risks and Blockers (Half Page)

**Format:** Three categories, brief entries.

**Active risks:** Problems engineering is already managing.
> Infrastructure vendor contract renewal is due January 15. Negotiation is in progress; current contract terms are $1.2M/year. Renewing at current terms is the likely outcome; renegotiation risk if we miss the 60-day window is contract lapse.

**Emerging risks:** Signals that may become problems in the next quarter.
> Attrition rate in the platform team is 25% annualized over the last two quarters. We have not lost a staff engineer yet, but the trend is a risk to Q4 delivery if it continues. Retaining the two remaining senior platform engineers is a priority.

**Blocked items requiring exec action:**
> The security audit firm engagement requires legal to execute the MSA. Currently in legal review for 3 weeks. Unblocking this before December 1 is necessary to stay on track for the Q1 SOC 2 report.

---

### Section 4: Team Updates (Half Page)

Headcount, hiring, and organizational changes. Written for a finance and operations audience; an engineering audience reads this differently.

**Format:**
- Headcount: [current] engineers vs. [plan]. [Delta and explanation if material].
- Open roles: [number open]. [Roles that are critical path if unfilled].
- Attrition: [number of departures] in Q3. [Voluntary vs. involuntary]. [Any pattern worth naming].
- Org changes: [any team reorganizations, new managers, reporting changes].

---

### Section 5: Asks (One Page)

What engineering needs from the executive team to deliver on Q4 commitments. The load-bearing section in the QBR. Be specific.

**Format for each ask:**
- **Ask:** [What you need]
- **By when:** [Date]
- **Why it matters:** [What happens if this does not happen by that date]
- **Who needs to act:** [Named person or function]

**Example:**
> **Ask:** Approve the $240K annual contract for the database observability tooling (vendor: Datadog, scope: production databases)
> **By when:** November 15
> **Why it matters:** We have three P1 incidents in Q3 directly attributable to database performance blind spots. This tooling closes that gap before Q4 holiday traffic.
> **Who needs to act:** CFO to approve the budget variance; CPO to confirm it is within the engineering tools budget envelope.

---

## Board Engineering Update Format

**Audience:** Board members (may include investors, independent directors, operators)
**Length:** 1 page (in the board deck), or a 5-minute verbal presentation with a leave-behind
**Cadence:** Quarterly or as part of a board-level product and technology update

Board members do not work as engineering practitioners. They want signal on three questions: Does the technology read as a risk or an asset? Can the team deliver on the company's growth plan? Do material issues exist that engineering leadership knows about and manages?

**One-page format:**

```
ENGINEERING — Q3 2024

DELIVERY
[Two sentences: what shipped, what it unlocked for the business.]
Example: "The payments infrastructure migration completed in August, reducing checkout 
error rates from 2.3% to 0.8% and eliminating the manual reconciliation process that 
cost ~12 engineering hours/week."

PLATFORM HEALTH
[One sentence per domain. Use signal words: stable, improving, at risk, recovering.]
Example:
- Reliability: Stable. Two P0 incidents in Q3; both resolved and postmortem'd.  
- Security posture: Improving. SOC 2 Type II audit scheduled for Q1; pre-scan clean.
- Scalability: At risk. Current architecture supports 2x current load; 
  Q2 2025 growth projections require architectural work beginning in Q1.

TEAM
[One sentence on headcount and one sentence on any material people risk.]
Example: "Team is at 48 of 52 planned headcount; four open roles are in late-stage 
interviews. One staff engineer departure in Q3; backfill is the priority hire."

RISKS TO WATCH
[One to two risks, two sentences each. Name the risk, the probability, and what 
engineering is doing about it.]
Example: "The AWS us-east-1 dependency is a concentration risk; a regional outage would 
take the product offline. We are evaluating multi-region active-passive for a Q2 
implementation. Cost estimate is $80K/year incremental infrastructure spend."

ASK FROM THE BOARD
[Zero or one asks. Boards can provide: introductions, governance decisions, budget 
approvals above the CEO's authority.]
Example: "No asks this quarter."
```

---

## Incident Executive Summary

**Audience:** CEO, CPO, VP of Customer Success (and whoever else is tracking the incident)
**Timing:** Within 2 hours of incident resolution; before any external communication
**Length:** 5 sentences. The format prescribes this; suggestion mode does not apply.

```
INCIDENT EXECUTIVE SUMMARY
[Service / Date / Duration]

1. WHAT HAPPENED
[One sentence on the observable failure — what users experienced.]
Example: "From 2:14 PM to 4:38 PM EST on November 12, users were unable to complete 
purchases through the web checkout flow."

2. CUSTOMER IMPACT
[One sentence on scope — how many users, what percentage, what revenue impact if known.]
Example: "Approximately 14,000 users encountered the failure; estimated revenue impact 
is $340K based on average order value and the window of impact."

3. ROOT CAUSE
[One sentence on technical root cause, written for a non-technical reader.]
Example: "A database configuration change deployed at 2:10 PM introduced a connection 
pool limit that was too low for peak checkout traffic."

4. FIX
[One sentence on what was done to restore service.]
Example: "The configuration change was reverted at 4:35 PM; full checkout availability 
restored at 4:38 PM."

5. PREVENTION
[One sentence on what will be done to prevent recurrence — and the timeline.]
Example: "We are implementing automated load testing for database configuration changes 
before production deployment; this will be in place by November 26."
```

**What not to include in the executive summary:**
- Stack traces, error codes, or technical diagnostics; these belong in the incident postmortem
- Attribution or blame; this creates legal risk and reads as inappropriate before a full postmortem
- Speculation about causes still unconfirmed
- Assurances that this will "never happen again"; these read uncredible and undermine trust

---

## Major Initiative Status Memo

**Audience:** Cross-functional leadership (CPO, CTO, CEO, relevant VP)
**When:** When a major initiative is in flight and needs to be discussed at the leadership layer
**Length:** 1–2 pages, written document (not slides)

```
[INITIATIVE NAME] STATUS MEMO
Date: [Date]
Author: [Engineering leader]
Distribution: [Who this is going to]

SITUATION
[Two to three sentences. What are we building, why, and where are we in the timeline?]
Example: "The identity platform migration moves user authentication from a legacy 
vendor (Auth0) to our own infrastructure. This is required to support the enterprise 
SSO feature on the Q4 roadmap and reduces annual vendor cost by $480K. We are in 
week 6 of a 14-week implementation plan."

PROGRESS
[Two to three sentences or a brief table. What is done, what is in progress, what is 
ahead? Honest on schedule.]
Example: "Phases 1 (infrastructure setup) and 2 (internal user migration) are 
complete. Phase 3 (external user migration) is in week 2 of 4. We are currently 
3 days ahead of schedule; this buffer is intentional given the risk in phase 4."

BLOCKERS
[Each blocker gets: what it is, who owns resolution, by when.]
Example:
- Legal review of data processing addendum with the new infrastructure vendor is 
  pending. Owner: legal. Needed by: November 20. Risk if delayed: phase 4 timeline 
  slips by approximately one week.

DECISION NEEDED
[The specific decision this memo is asking for, by whom, by when.]
Example: "We need CPO approval to send migration notification emails to all external 
users beginning November 18. The communication draft is attached. Approval needed 
by November 15 to allow 3 days for localization review."

NEXT UPDATE
[When leadership should expect the next status memo or update.]
```

---

## Headcount Request Memo

**Audience:** Engineering VP or CTO (and sometimes CFO or CEO)
**When:** When requesting a new headcount approval outside of the annual plan
**Length:** 1 page

The headcount request memo has to answer the question the approver asks in their head: "Why now, why this role, and what happens if I say no?" Failing to answer all three means the request will not get approved.

```
HEADCOUNT REQUEST
Role: [Title and level]
Team: [Team name and manager]
Date needed by: [When the person needs to start — work backward from business need]
Total compensation (est.): [Base + equity + benefits, as an annual cost]
Submitted by: [Requestor name and date]

BUSINESS CASE
[Two sentences. What initiative or outcome requires this hire? Why can existing 
capacity not absorb it?]
Example: "The fraud detection model migration, committed to product in Q4, requires 
ML engineering expertise the current team does not have. Pulling an existing engineer 
from the payments team to cover this work would put the checkout reliability OKR at 
risk."

INITIATIVE
[One sentence on what the hire will work on in their first 90 days.]
Example: "This hire will own the fraud detection model re-training pipeline and 
integrate it with the new feature store being built by the data team."

REQUIRED SKILLS
[3–5 bullet points. Be specific enough that recruiting can write a job description. 
Be specific enough that the approver can understand what they are approving.]
Example:
- 4+ years of production ML engineering (not research)
- Experience with real-time inference serving (sub-100ms latency constraints)
- Familiarity with payment fraud domain preferred but not required
- Python-native; experience with MLflow or equivalent experiment tracking

COST
[Annual cost all-in, including loaded benefits. Compare to the revenue or cost impact 
the hire is expected to unlock.]
Example: "Estimated fully-loaded cost: $280K/year. The fraud model is estimated to 
reduce fraud losses by $600K/year at current transaction volume."

RISK OF NOT HIRING
[One to two sentences. What is the realistic outcome if this request is not approved?]
Example: "If this hire is not approved by December 1 to allow for a Q1 start, we will 
need to deprioritize the fraud detection migration to Q2 and accept $150K in 
additional fraud losses in Q1 while we wait."
```

---

## Common Failure Modes

### Too Much Technical Detail

Executives reading about a database migration do not need to know which ORM the team used or the specifics of the migration tooling. Every sentence of technical detail failing to connect to a business outcome dilutes the communication and signals the author misreads their audience.

Test: read the document and ask, for every sentence, "does this help the executive make a decision?" If the answer reads no, cut it.

### Passive Voice on Risks

"There were some delays due to vendor dependencies" reads as passive voice on a risk. It obscures who holds responsibility, what the impact runs, and what someone is doing. Executives reading passive voice on risks conclude, correctly, that the engineering leader has lost full control of the situation.

Write risks in active voice with an owner and a timeline: "The vendor delayed the API documentation by two weeks. We escalated to their VP of Engineering on October 15; we now have access and run back on schedule."

### Missing the "So What"

The recurring failure in engineering-to-executive communication: stating facts without connecting them to consequences. Engineering leaders writing "we completed the infrastructure upgrade" and stopping there have communicated half the message. The complete message includes what changed for the business as a result.

Every delivery statement should carry a "which means..." or "as a result..." attached to it. If you cannot complete that sentence, you do not yet understand the business impact of the work; that signals a separate problem worth solving.

### Underselling Risk to Protect the Team

Engineering leaders downplaying risk to executives to protect the team from scrutiny do the opposite of protecting the team. Executives learning about a significant risk in a customer meeting or from a board member (instead of from the engineering leader) will conclude that engineering leadership is either unaware of the risk or hiding it. Both conclusions damage trust and prove hard to recover from.

Surface risks early, frame them constructively, and bring a mitigation plan. That builds trust.

### Updating After the Decision Is Made

Executives cannot act on information they receive after the window to act has closed. A headcount request arriving two weeks before the hiring freeze goes up arrives too late. An incident summary landing 12 hours after the incident resolves arrives too late for the CEO to carry context into the customer conversation they already had.

The timing of upward communication matters as much as the content. When in doubt, communicate earlier and with less polish over later and perfectly formatted.

## Further Reading

| Topic | Resource |
|---|---|
| Structured executive communication | Minto, *The Pyramid Principle* (Pearson, 2009) |
