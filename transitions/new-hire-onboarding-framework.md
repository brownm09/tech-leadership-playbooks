# New Hire Onboarding and Ramp-Up Framework

## Leadership Context

The 30-60-90 framework for a new engineering leader runs from the leader's perspective: how to listen, diagnose, and commit. The new hire onboarding framework runs the other direction: the document handed to an incoming engineer telling them what the first thirty days will look like, what success means, and where ownership sits. The two frameworks complement; they do not overlap.

The structural problem with most engineering onboarding: design serves the organization's convenience instead of the engineer's ramp. A list of systems to read, a Slack workspace to join, a calendar of recurring meetings to attend — these deliver information; they do not constitute onboarding. A new hire who has read all the wikis and joined all the channels but has not shipped anything and has not built a working relationship with their team has not onboarded. They have accumulated content.

The thirty-day ramp matters along three observable outcomes: understanding the system well enough to contribute, shipping something small enough to carry low risk yet real enough to carry value, and establishing a working relationship with at least one teammate beyond the buddy assignment. If an engineer reaches day thirty without all three, the onboarding has failed regardless of how many Confluence pages they have read.

## Background and Motivation

This framework was developed from onboarding multiple engineers into platform and payments teams at ActBlue Technical Services (2022–2025) and refined through the Community Tech Alliance context (2025–2026), where onboarding happened into a smaller team with less standardized infrastructure. At ActBlue, the challenge: scaling onboarding across six platform teams without sacrificing the 1:1 calibration making the first weeks effective. At CTA, the challenge: creating structure where very little existed without overengineering a process the team would not sustain.

The framework reflects a recurring observation: the engineers who struggled in their first quarter almost always had onboarding heavy on orientation (reading, attending, watching) and light on contribution (shipping, deciding, asking questions changing the plan).

## When to Use This

| Trigger | What This Framework Provides |
|---|---|
| New IC joining the team at any level | A structured 30-day ramp with clear ownership and observable milestones |
| Returning engineer after extended leave | A compressed version for re-establishing context without re-doing the full ramp |
| Bootcamp or internship conversion hire | A bridge from educational context to production ownership |
| Post-reorg team integration (engineer joins an existing team as part of a structural change) | A ramp that accounts for existing team context the engineer does not yet share |

---

## The Three Roles

Onboarding requires three distinct roles. Conflating them, or leaving any of them unfilled, produces the common failure modes.

**The Manager** is responsible for the success of the onboarding. This means setting the ramp plan, checking in weekly on milestones, clearing blockers the engineer and buddy cannot resolve, and conducting the 30-day retrospective. The manager owns the outcome; execution sits elsewhere.

**The Buddy** is responsible for daily context and access. Assign a peer engineer, not a senior leader. The buddy fields the first call when something feels confusing, when tooling breaks, or when the engineer does not know which channel fits the question. The buddy should be someone who joined in the last eighteen months and still remembers what it felt like to not know things. Do not assign the team's senior IC as the onboarding buddy; they hold a different relationship to not-knowing and rarely serve as the right first stop for operational friction.

**The New Hire** is responsible for communicating blockers, asking questions, and not waiting until a 1:1 to surface a problem. Learned helplessness, waiting for explicit permission to do the next thing, surfaces as a common new hire anti-pattern. The manager and buddy need to create an environment where asking questions registers as engagement, not as gap.

---

## The 30-Day Ramp

### Phase 1: Orientation (Days 1–7)

Phase one delivers access and context: getting the engineer to the point where they can read the codebase and understand the team's current work. Shipping does not belong here. Shipping before understanding produces the wrong kind of confidence.

**Days 1–3: Infrastructure access and environment setup.**
Every new engineer should have a working local development environment and repository access by end of day 3. When tooling blocks this, treat it as a tooling problem: note it and fix it. Do not let environment setup drift into week two.

**Days 4–5: Architecture orientation.**
The buddy walks the engineer through the primary systems: service boundaries, data flows, deployment pipeline, on-call structure, and monitoring stack. A live conversation with a working local environment open. The aim: a mental model.

**Days 6–7: Team context and cadence.**
The engineer attends the team's primary recurring meetings (sprint planning, standup, retrospective if scheduled). The manager conducts the first 1:1, using it to confirm environment setup, answer open questions, and identify the first task.

**Milestone at end of week 1:** The engineer can run the application locally, has read the architecture overview, and has identified their first task.

### Phase 2: First Contribution (Days 8–21)

Phase two delivers a real, merged contribution. The specific scope of "first task" matters more than most managers realize.

**Criteria for the first task:**
- Small enough to merge in under a week with review cycles included
- Substantive enough to matter to someone, not filler invented to occupy the engineer
- Low enough blast radius for a mistake to avoid triggering all-hands incident response
- In a part of the codebase the engineer will work in regularly, not a one-off corner offering no transferable context

Good first tasks: fixing a known bug in a non-critical path, adding a metric or log line to an existing flow, implementing a small feature with clear acceptance criteria, improving test coverage in an area with known gaps.

Poor first tasks: refactoring a large module, implementing a new external integration, anything requiring coordination with another team, anything the engineer will never touch again.

**Days 8–14: First task in progress.**
The buddy does at least one pair programming session during this week. The session walks through how to navigate the codebase and tooling in practice; the buddy does not write the code for the engineer. Code review on the first PR should be thorough and instructional. Explain the reasoning behind the verdict.

**Days 15–21: Second task and independent navigation.**
By day 15 the engineer should be able to identify their next task from the backlog without the manager assigning it. If they cannot, they have not yet built the context for independence; surface this in the week 2 1:1 and address it explicitly.

**Milestone at end of week 3:** At least one PR merged. Engineer can identify their own next task. Engineer has participated in at least one code review as reviewer (reviewing someone else's work builds codebase context quickly).

### Phase 3: Independent Delivery (Days 22–30)

Phase three delivers sprint participation: the engineer no longer runs on a separate ramp track and contributes to the team's shared commitments.

**Days 22–28: Sprint integration.**
The engineer should carry story-level work in the sprint, beyond "onboarding" tasks. They should attend the sprint ceremonies as a participant: commenting in retrospectives, asking questions in planning, and flagging scope risks when they see them.

**Days 29–30: 30-day retrospective.**
The manager and engineer conduct a structured retrospective (see template below). The output: what worked in the ramp, what should be changed for future hires, and an explicit agreement on what the engineer's first 60-day goals look like.

**Milestone at end of day 30:** Engineer is carrying sprint commitments at partial velocity. Manager has completed 30-day retrospective. Engineer has named at least one thing they want to learn in the next thirty days.

---

## Ramp Metrics

The manager tracks these; do not share with the engineer as a performance rubric. The underlying milestones match the engineer-facing checklist; the warning signals and framing here calibrate to manager monitoring.

| Metric | Target | Warning Signal |
|---|---|---|
| Time to first merged PR | ≤ 14 days | > 21 days without a blocker explanation |
| Time to first solo incident participation | ≤ 45 days | Not on-call rotation by day 60 |
| Time to sprint participation | ≤ 30 days | Still on separate ramp track at day 30 |
| 30-day retrospective held | Day 28–32 | Not scheduled or skipped |
| Manager 1:1 frequency in month 1 | Weekly | Biweekly or skipped |

---

## Onboarding Failure Modes

**Too much reading, not enough doing.** A two-week "reading sprint" before the engineer touches code almost always misfires. The reading does not stick without context, and the context comes from doing. Send the engineer to the codebase on day four, not day fourteen.

**No defined success criteria.** "Get comfortable with the codebase" does not name a milestone. Engineers who do not know what success looks like in week one will not know whether they are on track in week three. The manager's job: make success concrete and observable.

**Buddy assignment without buddy preparation.** Assigning a buddy without explaining the buddy's responsibilities guarantees inconsistency. The buddy needs to know: what they own, what the manager owns, how to escalate a blocker, and when the buddy relationship formally ends (typically 30 days, with optional continuation).

**The firehose first week.** Cramming every system introduction, every team overview, every stakeholder meeting, and every onboarding doc into the first five days produces an engineer who has heard everything and retained little. Sequence context so each layer builds on the last. The payment system architecture overview belongs in week two, not day two.

**The invisible first month.** An engineer who gets through thirty days without being visible in any team communication (no questions in Slack, no comments in code reviews, no contributions to a planning meeting) has been allowed to hide. Hiding in the first month signals an environment failing to create safety for participation.

---

## Templates

### 30-Day Onboarding Checklist (for the engineer)

```
## Week 1: Access and Orientation
[ ] Development environment running locally
[ ] Repository and CI/CD access confirmed
[ ] Primary communication channels joined (Slack workspaces, incident tooling)
[ ] Architecture walkthrough with buddy completed
[ ] First 1:1 with manager completed
[ ] First task identified

## Week 2: First Contribution
[ ] First task in progress
[ ] One pair programming session with buddy
[ ] PR opened
[ ] At least one code review completed as reviewer

## Week 3: Independent Navigation
[ ] First PR merged
[ ] Second task self-identified from backlog
[ ] Attended sprint ceremonies as participant (not observer)

## Week 4: Sprint Integration
[ ] Carrying sprint story points or tasks in shared sprint
[ ] On-call shadowing scheduled (if applicable)
[ ] 30-day retrospective scheduled with manager

## Day 30
[ ] 30-day retrospective held
[ ] 60-day goals agreed with manager
[ ] Buddy relationship formally acknowledged and transitioned to peer
```

### Manager Onboarding Kick-Off Agenda (first 1:1, day 1–3)

```
1. Context and team overview (10 min)
   - What the team owns, what we are working on, where we are in the sprint
   - How to find information (who to ask, where to look)

2. Ramp plan walkthrough (10 min)
   - The three phases and what success looks like in each
   - The buddy role and how to use it
   - When to escalate vs. when to figure it out

3. First task (5 min)
   - Walk through the task together
   - Confirm the engineer understands scope and acceptance criteria
   - Identify the first person to talk to if they get stuck

4. Open questions (5 min)
   - What is the engineer most uncertain about right now?
   - Is there anything about the environment or team that is not clear?

5. Weekly 1:1 cadence confirmed (1 min)
   - Day and time, format, agenda ownership
```

### Buddy Guide

```
## Your Role
You are the first stop for operational friction — environment problems, tooling
confusion, "which channel do I ask this in?" questions. You are not responsible
for teaching the engineer their job. You are responsible for making sure the
environment does not get in the way of them doing it.

## What You Own
- One architecture walkthrough in days 4–5
- One pair programming session in week 2
- Availability for ad-hoc questions (same-day response; no need to be instant)
- Weekly check-in (informal, 15 min or less) for the first four weeks

## What the Manager Owns
- Assigning and scoping the first task
- Weekly 1:1 with the engineer
- Clearing blockers that are outside the team (access, process, tooling)
- The 30-day retrospective

## Escalation
If the engineer is blocked on something you cannot resolve in 30 minutes, flag it
to the manager the same day. Don't spend a day debugging an environment issue
that a manager could clear in ten minutes with one Slack message to IT.

## When It Ends
The buddy relationship is formal for the first 30 days. After that, you transition
to peer. There is no ceremony required — just acknowledge it in the week 4 check-in:
"The formal buddy period is ending; I'm here as a peer going forward."
```

### 30-Day Retrospective Agenda

```
1. What worked in the ramp? (10 min)
   - What was most useful in the first week?
   - What made the first contribution go smoothly?

2. What should be changed? (10 min)
   - What took longer than expected?
   - What was missing from the onboarding that would have helped?

3. Process feedback (5 min)
   - What should we change for the next hire?
   - Is there documentation that should exist but doesn't?

4. Looking ahead (5 min)
   - What does the engineer want to learn in the next 30 days?
   - What are the 60-day goals we are agreeing on now?
```

---

## Anti-Patterns

**The Information Dump.**
Scheduling twelve system overview meetings in the first week because "there's a lot to learn." The engineer absorbs about 20% of it, and the 80% they missed produces a false sense of completion. Prioritize the systems the engineer's work touches first; sequence everything else.

**The Invisible Buddy.**
Assigning a buddy too senior, too busy, or too unfamiliar with day-to-day operational friction to help. A strong buddy sits one to two years into their tenure: experienced enough to know the system, recent enough to remember not knowing it.

**The Test-Free First PR.**
Allowing the first merged contribution to skip tests "just to get something in." The team's test standards then read as negotiable from day one. Hold the bar.

**The Delayed Retrospective.**
Skipping or rescheduling the 30-day retrospective because the manager has scheduling pressure. The retrospective drives improvements in onboarding for the next hire. Skipping also signals to the engineer their experience held no value worth examining.

**The Sink-or-Swim Frame.**
Framing onboarding as a proving ground instead of a ramp. Engineers who feel tested from day one instead of supported ask fewer questions and hide more often when stuck. The first thirty days should feel like a structured ramp, not a trial.
