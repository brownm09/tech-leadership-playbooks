# Escalation Framework

## Leadership Context

Escalation failure operates at high cost and low visibility in engineering organizations. When escalation works, problems surface early enough to be managed: a delivery risk gets caught in week 3 of a 12-week project (no later than week 10); a personnel situation gets addressed before it affects the team; a reputational risk reaches the CTO before it reaches the press. When escalation fails, the organization pays the compound interest on every day the problem stayed visible below the decision-making level and invisible above it. This playbook covers when to escalate, to whom, in what format, how to do it without blame or alarm, and how to distinguish the situations requiring escalation from the ones handled at the team level without surfacing upward.

## Background and Motivation

The escalation patterns in this framework grew out of multi-team programs at ActBlue Technical Services (2022–2025): particularly the PCI environment deprecation and Heroku→Kubernetes migration, both running against compliance deadlines and requiring sustained executive visibility over multiple years. The trigger categories and escalation levels here reflect what kept those programs on track; a generic model would miss the texture.

## When to Use This

- A new IC or engineering manager is onboarding and needs to understand what "good escalation behavior" looks like in this org
- Post-incident review of a significant delivery failure, customer escalation, or personnel situation reveals that escalation happened too late, too early, or not at all
- A new manager is being onboarded and needs to understand the escalation norms before they inherit a team
- An engineering Chief of Staff is designing or auditing the escalation operating model

**What this document covers:** Leadership-layer escalation — delivery risk, people risk, and reputational risk. This is distinct from on-call escalation, which covers production incidents and follows its own protocol (see [On-Call Restructuring Framework](../incident-management/on-call-restructuring-framework.md)).

---

## Escalation Triggers

Not every problem warrants escalation. The threshold question: does this situation require a decision, resource, or intervention unavailable at the level where the problem currently lives? If yes, escalate. If no, handle it at the current level and document what was done.

The following three categories cover the large majority of escalation-worthy situations.

### Category 1: Delivery Risk

Delivery risk escalation fits when a commitment sits in jeopardy and either (a) the jeopardy requires a decision above the team level, or (b) a cross-functional partner or executive needs to know before the miss lands instead of after.

**Escalation triggers in this category:**
- A project is 30% or more behind schedule and the root cause cannot be resolved at the team level within the next 5 business days
- A dependency (cross-team, vendor, or cross-functional) is blocking progress and the blocker owner is not responding to the team or manager
- A scope or timeline commitment was made to a customer, partner, or executive, and engineering now believes that commitment will not be met
- An unforeseen technical risk (security vulnerability, architectural issue, third-party integration failure) has emerged that will add more than two weeks to a project timeline
- The team has lost a key person (departure, extended leave) and the gap cannot be absorbed without affecting a committed delivery

**Threshold for escalation in this category:** A delivery risk unresolved at the current level within 5 business days of identification should be escalated. Surfacing in passing falls short; the escalation needs a memo or structured update.

**Common mistake:** Waiting until the risk materializes into a miss before escalating. A delivery risk surfacing in week 4 of a 10-week project with an escalation memo and a mitigation plan stays manageable. The same risk surfaced in week 9 with "we're going to miss" becomes a crisis. The escalation feeling premature in week 4 almost always reads as the right call.

---

### Category 2: People Risk

People risk escalation fits when a situation involving a specific person on the team requires action, awareness, or resources beyond what the manager can provide, or when the situation creates organizational risk leadership needs to know about.

**Escalation triggers in this category:**
- A team member's performance has been formally documented (verbal or written) and has not improved within the agreed improvement period
- A team member has raised a complaint (against a peer, manager, or cross-functional partner) involving potential harassment, discrimination, or a code of conduct violation; this escalates immediately to HR and the relevant management chain
- A team member appears to face significant personal risk (mental health crisis, expressed statements of self-harm); this escalates immediately to HR and, if needed, to emergency services
- A key team member is at flight risk (has shared job search activity, has received an outside offer, or has expressed significant dissatisfaction) and their departure would create a material delivery or institutional knowledge gap
- A manager is struggling with a direct report situation and needs director-level coaching or intervention to resolve it
- A team conflict (between two engineers, or between an engineer and a manager) is escalating to the point where it is affecting team function

**Threshold for escalation in this category:** People situations stay context-dependent, but the default rule runs: when a manager has had more than one direct conversation about a performance or behavior issue and the situation has not changed, escalation to the director fits. This serves no paper trail; it gives the director accurate information for making resourcing and organizational decisions.

**Common mistake:** Managers over-protecting their team by failing to surface people risks upward. A director discovering a performance situation 60 days after the manager first identified it, with no escalation in between, has lost the window to intervene constructively. The manager's instinct to handle it independently makes sense; the operational consequence: leadership cannot allocate resources (HR support, coaching, temporary capacity help) to the situation.

---

### Category 3: Reputational and External Risk

Reputational risk escalation fits when a situation involves external parties (customers, regulators, press, investors), when a decision carries legal or compliance implications, or when the situation could reach executive or board awareness from a channel other than engineering.

**Escalation triggers in this category:**
- A customer has escalated a technical issue to their executive sponsor, and engineering leadership is not already on the email thread
- A security vulnerability has been discovered that may have already resulted in data exposure; this escalates immediately regardless of severity assessment
- A public incident, outage, or data exposure is being discussed externally (on social media, in press, in customer community forums) before a formal communication has been issued
- A regulatory inquiry, audit request, or legal hold has been received
- A vendor or partner has communicated a material change (pricing increase, service discontinuation, breach notification) that affects engineering's ability to meet its commitments
- A team member has communicated to someone outside the org (customer, press, social media) in a way that creates a commitment or creates reputational risk for the company

**Threshold for escalation in this category:** Immediate, or within the same business day. Reputational and external risks do not wait for the weekly staff meeting.

**Common mistake:** Treating a customer complaint escalated to executive level as a "support issue" instead of a leadership issue. By the time a customer's VP of Engineering emails the CTO, the window for managing the relationship through normal channels has closed. The escalation should have happened when the customer first expressed dissatisfaction; waiting until they escalated past the engineering team came too late.

---

## Escalation Levels

### Team → Engineering Manager

**What belongs here:** Day-to-day delivery friction, team-internal interpersonal issues, sprint-level scope questions, individual IC performance feedback, dependency gaps within a team.

**What the manager is expected to do:** Resolve it, or escalate within 5 business days if it cannot be resolved at this level.

**What the manager should document:** A brief written note to themselves (or in the team's task tracker) with: the issue, what was tried, and the outcome. This documentation exists for the manager's benefit; if the situation escalates, a record exists.

---

### Engineering Manager → Engineering Director

**What belongs here:** Cross-team dependency blockers, delivery risks that affect a committed initiative, team-level people situations that have not resolved after two direct conversations, vendor or tooling issues that require budget authority above the team level.

**What the director is expected to do:** Unblock, decide, or escalate within 2 business days.

**How to escalate:** The escalation memo format (see below) or a brief async written update to the director with the same structure. The format matters because it forces the manager to crystallize what they need from the director.

---

### Engineering Director → VP Engineering / CTO

**What belongs here:** Delivery risks affecting a company-level commitment or a customer-facing commitment, people situations involving a manager or above, decisions that require budget authority above the director level, any situation with reputational or external risk (see Category 3), any people situation involving potential legal or HR implications.

**What the VP/CTO is expected to do:** Engage within 24 hours on Category 3 issues, within 48 hours on Category 1 and 2 issues.

**How to escalate:** The escalation memo format, delivered to the VP/CTO directly; a chain of intermediaries dilutes the signal. The director should confirm receipt and response timeline before assuming the escalation landed.

---

### VP Engineering / CTO → CEO / Board

**What belongs here:** Delivery failures with material revenue impact, security incidents with data exposure, regulatory notifications, personnel situations involving executive-level individuals, strategic risks to the engineering organization's ability to support company goals.

**How to escalate:** Written escalation memo with an executive-ready format. For Category 3 issues (external risk), phone call first, then memo. The memo documents the conversation; it does not replace it.

**The rule about boards:** Nothing should reach the board without prior discussion between the CEO and CTO. If a board member learns about a material engineering risk from a source other than the CTO or CEO, something in the escalation chain failed.

---

## The Escalation Memo Format

A well-formed escalation memo takes 15–20 minutes to write and runs 5–8 sentences. It does no project-update work and no status reporting; it enables a decision.

**Five sentences:**

```
1. SITUATION
[What is happening, when it started, and what the current state is. One to two sentences. 
No attribution, no blame, no editorializing; the facts alone.]

2. IMPACT
[What the consequence is if this is not resolved, and by when it matters. Be specific: 
revenue, customer relationship, compliance deadline, delivery commitment, personnel stability.]

3. OPTIONS
[Two or three options for responding to this situation, stated neutrally. Include a 
"do nothing" option if it is realistic, and name its cost.]

4. RECOMMENDATION
[What you believe the right option is and why. This is where your judgment goes. 
If you do not have a recommendation, say so explicitly: "I don't have a recommendation; 
I need your input on option 2 or 3."]

5. BY WHEN
[When a decision or action is needed to stay ahead of the issue. Be specific about 
the date and what happens after that date if the decision is not made.]
```

**Example escalation memo:**

> **Situation:** The data pipeline dependency for the fraud detection model (Q4 OKR) was expected to be complete by November 15. The data engineering team has encountered a data quality issue in the upstream events table that will delay delivery to November 29 — two weeks behind plan.
>
> **Impact:** The fraud detection model requires 4 weeks of clean data to train. If the pipeline is not complete by November 29, the earliest the model can be trained and validated is December 27 — after the Q4 cutoff. The Q4 OKR for fraud loss reduction ($600K/year estimated impact) will be missed, and the work will carry into Q1.
>
> **Options:** (1) Accept the Q4 miss, carry the work into Q1, and update the Q4 OKR score accordingly. (2) Reprioritize the data engineering team to resolve the data quality issue by November 22, requiring them to deprioritize their current sprint commitment (the analytics dashboard release). (3) Scope down the fraud model to use a smaller, already-validated dataset, accepting reduced model accuracy — estimated impact is 60% fraud reduction instead of 80%.
>
> **Recommendation:** Option 3 — reduced scope now, full model in Q2 with the complete dataset. The $600K/year estimate was based on the full 80% reduction; even 60% recovers ~$450K/year and delivers in Q4 as committed. This avoids disrupting the data team's sprint commitment.
>
> **By when:** Need a decision by November 14 to allow time for the reduced-scope implementation plan.

---

## Non-Escalation Anti-Patterns

### Hero Culture: Fixing It and Not Telling Anyone

The prevailing escalation failure in engineering organizations involves no failure to escalate a crisis; it shows up as a manager or IC resolving a significant risk through heroic personal effort and never surfacing it to leadership.

**Why this poses a problem:** Leadership cannot learn from risks they never knew occurred. The headcount decision preventing the crunch does not get made. The architectural investment preventing the recurrence stays off the roadmap. The manager absorbing an unsustainable load for six weeks without speaking up gets praised for execution and burns out in Q2.

**The calibration question:** "If I had told my manager what was happening, what would have happened?" If the answer reads "they would have helped" or "they would have made a better decision," escalate next time. If the answer reads "nothing, I handled it faster," this one case earns the hero move. Most people using this test honestly find they should have escalated earlier more often than they did.

### False Alarm Fatigue: Escalating Too Early

The opposite failure: escalating every uncertainty to the director level, with the effect that the director spends much of their time triaging issues solvable at the team level, and begins to discount escalations because most do not require their intervention.

**The calibration question:** "Have I tried two specific things to resolve this at my level, and neither worked?" If no, handle it at the current level first. If yes, escalate.

**The intent behind this threshold:** The point makes no effort to render escalation onerous; the point ensures that when something reaches the director, the director knows it genuinely requires their level of authority or perspective.

---

## Escalation Response SLA

Escalation runs as a two-way process. When someone escalates to you, you owe them a response within a defined timeframe. The absence of a response does not equal "working on it"; it reads as "did not receive this" or "chose not to act."

| Escalation type | Expected response time | What the response must include |
|---|---|---|
| Category 3 (external/reputational risk) | Same business day | Acknowledgment, whether you are engaging, and what the next step is |
| Category 2 (people risk, potential legal/HR) | Within 24 hours | Acknowledgment and whether you are looping in HR or escalating further |
| Category 1 (delivery risk) | Within 48 hours | Acknowledgment of the escalation, your read on the options, and whether you are deciding or escalating further |
| General escalation memo | Within 48 hours | A response to each of the five memo sections — situation understood, impact assessed, options considered, decision made or escalated, timeline |

**Response does no resolution work.** A 24-hour acknowledgment does not mean the problem gets solved in 24 hours. It means the escalator knows their message landed and someone is acting.

If you cannot respond within the SLA, delegate the acknowledgment to someone who can: "I received this. I am in back-to-back until Thursday. [Name] will reach out in the next 4 hours." The absence of any response within SLA defines the failure mode.

---

## How to Escalate Without Blame

Escalation language assigning blame before the facts get established creates defensiveness, distorts the information leaders receive, and makes the person being blamed less likely to stay forthcoming in future escalations.

**Language patterns that introduce blame:**
- "The data team dropped the ball on..."
- "[Name] failed to deliver..."
- "This wouldn't have happened if product had..."
- "We warned them this would happen, but..."

**Language patterns that describe the situation without blame:**
- "The data pipeline dependency was expected by November 15; it is now projected for November 29."
- "The current state is X. The contributing factors we have identified are A, B, and C."
- "The design requirement was received on [date], which left [N] days for implementation. That was not enough time."

Blame language in an escalation memo signals the author has already concluded who is at fault, bringing a conclusion to leadership instead of a problem needing analysis. Leadership receiving blame-laden escalations has to spend time evaluating whether the framing reads accurately before they can address the actual issue.

Blame does not mean accountability conversations disappear. It means those conversations happen after the facts get established and the immediate situation resolves; the escalation memo holds no place for them.

---

## Distinguishing Escalation From Venting

Not every difficult conversation with a manager qualifies as an escalation. The internal checklist below helps distinguish situations warranting a formal escalation memo from situations warranting a different kind of conversation.

**Before drafting an escalation memo, answer these questions:**

1. Does a decision exist beyond my authority to make? (If yes, escalate.)
2. Do I need a resource unavailable to me at my level? (If yes, escalate.)
3. Does a level above me need information, regardless of whether any action gets required right now? (If yes, escalate; frame it as an FYI, never as a request for action.)
4. Am I seeking permission to do something I already know reads as the right thing to do? (If yes, make the decision and inform; do not escalate.)
5. Am I frustrated about a situation and wanting someone above me to validate that frustration? (If yes, this calls for a conversation; an escalation memo would misframe it. Have the conversation; do not dress it as an escalation memo.)

Most escalation-vs.-venting confusion comes from question 5. Frustration runs legitimate and worth expressing; expressing it in the format of a formal escalation memo creates the expectation that the director will act when the person may only want to feel heard. Be clear about which you are asking for.

---

## On-Call vs. Leadership Escalation

These describe two different systems with two different purposes.

**On-call escalation** fires on a production incident. The on-call runbooks define the path: page the IC, page the responder, escalate within the incident when the IC cannot be reached or the incident exceeds defined severity thresholds. The on-call system exists to restore service. See [On-Call Restructuring Framework](../incident-management/on-call-restructuring-framework.md).

**Leadership escalation** (this document) fires on a risk or decision requiring leadership-layer attention. It carries no time-constraint comparable to an incident; it has urgency calibrated to the type of risk. Its purpose: decision-making, resource allocation, and risk management; service restoration sits elsewhere.

**The interaction between the two systems:** A significant production incident will often trigger both. The on-call system restores service; the leadership escalation ensures the right executive stays aware, the customer communication gets managed, and the post-incident investment decisions get made with full information. The incident postmortem marks the handoff point: once service is restored, the on-call system's job ends and the leadership-layer review begins.

One failure mode in practice: an incident gets resolved by the on-call team, correctly, but no one escalates the severity or the customer impact to leadership. The CTO hears about a significant P0 incident three days later in the QBR. That gap counts as a leadership escalation failure; the on-call system worked.

---

## A Note on Escalation Culture

The load-bearing thing an engineering director or VP can do to make escalation work: model the behavior they want to see and reward it when they see it.

If an engineering manager escalates a delivery risk early and the director's response runs frustration or criticism, that manager will not escalate early again. They will wait until the risk lands as a confirmed miss; exactly the wrong time.

The response building escalation culture: "Thank you for surfacing this early. Here is what I think we should do. Let me know if the mitigation plan isn't working by [date]."

The response killing escalation culture: "Why is this the first I'm hearing about this? This should have been caught earlier."

Both responses may read as accurate. Only one builds the behavior you need.

## Further Reading

| Topic | Resource |
|---|---|
| Difficult conversations and escalation communication | Patterson et al., *Crucial Conversations* (McGraw-Hill, 2021) |
| Management escalation structures | Grove, *High Output Management* (Vintage, 1995) |
