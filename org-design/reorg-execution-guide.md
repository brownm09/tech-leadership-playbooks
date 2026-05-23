# Reorg Execution Guide

## Leadership Context

The [Team Topology Framework](team-topology-framework.md) covers the principles for designing a well-structured engineering organization: stream-aligned teams, platform teams, enabling teams, and the cognitive load thresholds signaling when a team topology has run past its usefulness. That framework addresses *what* to change and *why*. This guide picks up at the next question: how to execute the reorganization itself.

Execution determines whether a reorg achieves its intended effect. The structural change usually lands correctly: reporting lines updated, charters redrawn, headcount reallocated. The human elements get underestimated or deferred — communication sequencing, the handling of the announcement, re-norming in reconfigured teams. Engineers experiencing a reorg as something done to them, with no path into the rationale, perform worse in the new structure even when the structure itself is correct.

The decision about whether to reorg belongs in the topology framework. This guide covers how to execute one in a way preserving trust, minimizing the performance dip every reorg produces, and re-establishing effective team norms quickly.

## Background and Motivation

This framework draws on the platform directorate formation at ActBlue Technical Services (2022), which involved significant structural change: chartering new teams (DevEx, Payments Architecture, Database Platform), realigning existing team scope, and shifting reporting structures across approximately twenty engineers in six teams. The framework also reflects observation of subsequent reorgs at ActBlue where I sat as an organizational stakeholder without owning execution; that vantage point offered a contrasting view of the process from the receiving end.

For engineers stepping into the role of a newly reorganized team, the [Inheriting a Team You Didn't Hire](../transitions/inheriting-a-team.md) framework provides the complementary playbook.

## When to Use This

| Trigger | What This Framework Provides |
|---|---|
| Chartering a new team from scratch | Pre-announcement checklist, communication sequencing |
| Merging two teams into one | Announcement guidance, re-norming process for the combined team |
| Splitting a team into two | Structural preparation, manager briefing sequence |
| Realigning reporting lines without changing team charters | Simplified communication sequence (skip team charter work) |
| Post-acquisition integration of an engineering team | Extended version of the full execution sequence with HR and legal coordination |

---

## Part 1: Pre-Announcement Checklist

Nothing should be communicated to the broader team before every item on this list is complete. Sequencing matters: the people most affected should know before the people least affected. Announcing broadly before affected managers are prepared inverts this order; the inversion accounts for a large share of reorg execution failures.

### Structural Decisions (complete before any communication)

- [ ] Reporting lines are finalized and approved by the appropriate level of leadership
- [ ] New team charters are drafted (scope, ownership boundaries, team name if changing)
- [ ] Headcount assignments are confirmed — every engineer has a clear landing in the new structure
- [ ] Any role changes (level adjustments, new manager assignments) are confirmed with HR
- [ ] Compensation adjustments if applicable are confirmed and ready to communicate

### HR and Legal Alignment

- [ ] HR has reviewed the plan for any performance-related personnel decisions made in connection with the reorg
- [ ] If any roles are being eliminated: HR, legal, and the manager of record are aligned on timing and communication
- [ ] If any roles are being created: job specs and level definitions are drafted and approved

### Manager Briefing Sequence

Before any broader communication:

1. **Brief the manager's manager first** — they need to be able to answer questions without appearing surprised
2. **Brief each affected manager in 1:1 conversations** — do not deliver reorg news to managers in a group setting
3. **Allow 24 hours for managers to process** before expecting them to deliver the news to their teams
4. **Confirm manager messaging** — give each manager the core points they need to communicate and the questions they should be prepared to answer

The gap between the manager briefings and the team announcement should run as short as possible (24–72 hours). A reorg plan widely known at the manager layer but not yet communicated to ICs produces anxiety from information asymmetry.

---

## Part 2: The Announcement

### What to Say

The announcement should answer five questions in the following order. When the announcement skips one of these, the team spends the post-announcement period generating their own answers, and the generated answers come out worse than anything you would have said.

1. **What is changing?** The specific structural change: who moves where, which teams change, what the new structure looks like. Specific over vague. "The platform and infrastructure teams are merging" beats "we are optimizing our org structure for scale."

2. **Why now?** The business or technical rationale. Skip the comprehensive history; aim for one to two sentences giving engineers a real reason. Platitudes lose. "Our current team boundaries have created a dependency bottleneck slowing every team's delivery" works as a real reason. "To position the org for future growth" does not.

3. **What does this mean for you specifically?** The team members want to know: does my job change? does my manager change? does my project change? Generic announcements skipping the individual question produce individual anxiety, and the anxiety spreads into collective distraction.

4. **What is not changing?** Naming what stays the same carries as much weight as naming what changes. Engineers will assume everything is uncertain until you tell them otherwise. When the tech stack, the current project commitments, and the on-call rotation hold steady, say so explicitly.

5. **What happens next, and by when?** A concrete timeline for the transition. "Team charters will be finalized by [date]. New sprint planning begins on [date]. Individual 1:1s with your new manager will be scheduled by [date]." Uncertainty about next steps produces more anxiety than the reorg itself.

### Announcement Formats

**All-hands (synchronous, preferred):** Lets questions get answered in real time. Schedule no more than 24 hours after manager briefings. Prepare for questions in three categories: logistics (the specific change and when), motivation (why this decision got made), and personal impact (what this means for me). Prepare answers in each category.

**Written announcement (async, as supplement):** Always follow a verbal announcement with a written summary engineers can reference later. The written version carries the same five-question structure as the verbal announcement. Sending the written announcement before the verbal one fails: questions arrive with no opportunity for clarification.

**Do not announce via org chart change.** Org charts updated in systems before the announcement let engineers discover the change without context. Check whether your HR system's org chart is visible to employees, and if so, delay the system update until after the announcement.

---

## Part 3: The 72 Hours After Announcement

The 72 hours following a reorg announcement concentrate the period of risk to the changes the reorg intended to produce. Engineers are processing, and the signal they look for from leadership runs simpler than more information; it is consistency between what was said in the announcement and what shows up in the building.

### What to Watch For

**The Informal Information Network.** Engineers talk to each other, and the informal information circulating after an announcement often carries more weight than the official communication. Watch the signal moving through the team's informal channels: which questions surface in Slack, which uncertainties surface in retros, which concerns first-line managers hear in 1:1s. The gap between what was communicated and what is circulating informally marks where the announcement failed.

**The High Performer Who Goes Quiet.** After a reorg, strong performers uncertain about the new structure may go quiet as a processing strategy; disengagement is not the right read. A high performer visibly less engaged in the week after an announcement should get a direct check-in conversation: "How are you experiencing the change? Is there anything I can answer or clarify?" Do not wait for the next scheduled 1:1.

**The Manager Who Is Not Ready.** Some managers absorb the reorg news intellectually but lack the emotional readiness to deliver it clearly to their teams. When you observe a manager communicating uncertainty, frustration, or ambivalence to their direct reports, address it in a 1:1 with the manager; the all-hands is the wrong venue.

### One-on-One Cadence

In the week following the announcement, every manager should hold a 1:1 with every direct report. These run as check-in conversations; status reporting belongs elsewhere:

- How are you doing with the change?
- Where does the uncertainty concentrate for you right now?
- Is there anything about your role or project needing clarity?
- What can I do to make this transition easier?

Do not skip these conversations because the team "seemed fine" in the all-hands. The all-hands creates a social context where expressing concern feels uncomfortable. The 1:1 surfaces the real concerns.

---

## Part 4: Re-Establishing Norms

Re-norming consistently gets underestimated as a reorg phase. Even when the individual engineers stay the same and the work stays the same, a change in team membership or structure disrupts the implicit agreements governing how work gets done.

### What Norms Need Re-Establishing

**Decision-making authority.** In a newly formed team, nobody yet knows whose job covers which decisions. Re-run the decision authority conversation early and explicitly: who holds authority in which domains, how disagreements get resolved, what gets escalated.

**Communication cadence.** How the team communicates across sync (standups, planning, retro) and async (Slack, docs, code review) channels should be explicitly agreed, never assumed to transfer from the prior structure. Different teams hold different norms, and a merged team brings multiple prior norms into contact.

**Quality standards.** Code review standards, test coverage expectations, and deployment conventions need explicit re-agreement when a team's membership changes. "We've always done it this way" stops working as a complete statement when half the team is new to this team's "always."

### The Re-Norming Retro

Within the first two sprints of the new team structure, run a structured retro focused specifically on norms — not on sprint output. Format:

```
1. What do we want to keep from how each of our prior teams operated? (10 min)
2. What do we want to leave behind? (10 min)
3. What do we need to create that neither team had? (10 min)
4. What is the one thing we should decide today to make the next sprint go better? (10 min)
```

The retro output runs as a brief working agreement: a short list of explicit agreements the team can hold each other accountable to. A comprehensive policy document is the wrong artifact here.

### The New Team Charter

For newly formed or significantly restructured teams, a written team charter shortens the period of norm uncertainty. The charter should cover:

- Team mission and primary ownership domain
- Team name (if new)
- Scope: what the team is responsible for, and what is explicitly out of scope
- Dependencies: which other teams this team depends on, and which teams depend on this team
- Decision authority: how the team makes decisions within its scope
- Operating cadence: recurring ceremonies and their purpose

The team charter functions as a working tool; the bureaucratic-document framing produces a charter no one consults. Keep it short enough to read in practice (one page is ideal) and revisit it at the six-month mark or after any significant scope change.

---

## Templates

### Announcement Email / Slack Message

```
Subject: [Team name] — organizational change

I want to share a change we're making to our team structure, effective [date].

**What's changing:**
[Specific structural change: who moves where, what team names change, what scope changes]

**Why:**
[1–2 sentences of real rationale — a specific problem this change addresses]

**What this means for you:**
[The individual impact — role changes if any, manager changes if any, project continuity]

**What's not changing:**
[Explicit list of things that stay the same]

**What happens next:**
- [Date]: [Specific next step — new manager 1:1s, charter finalization, etc.]
- [Date]: [Specific next step]
- [Date]: [Specific next step]

I'll be holding time in [all-hands / office hours / channel] on [date] for questions.

[Signature]
```

### Manager Briefing Agenda (pre-announcement)

```
1. What is changing (5 min)
   - Walk through the specific structural change
   - Show the before and after reporting structure

2. Why it's happening (5 min)
   - The rationale, at the level of detail you are authorized to share
   - What problems this is intended to solve

3. What each manager needs to communicate to their team (10 min)
   - The core message: what, why, personal impact, what's not changing, next steps
   - What questions to expect and how to answer them
   - What to say when you don't know the answer ("I'll find out and get back to you by [date]")

4. Manager questions (10 min)
   - Take questions. Document anything you cannot answer in the room.
   - Follow up on undisclosed items before the announcement.
```

### Team Charter Kickoff Agenda

```
45-minute working session, first week of new team structure

1. Mission statement draft (15 min)
   - Why does this team exist? What would be missing if it didn't?
   - What is the team responsible for? What is explicitly out of scope?

2. Dependencies and interfaces (10 min)
   - Which teams do we depend on?
   - Which teams depend on us?
   - Where are the potential conflict points?

3. Decision authority (10 min)
   - What decisions does the team make independently?
   - What requires escalation?
   - How do we handle disagreements within the team?

4. Operating cadence (10 min)
   - What recurring ceremonies do we want? At what frequency?
   - How do we communicate across sync/async?
   - Who owns the team calendar / documentation home?
```

### Re-Norming Retro Format

```
Pre-work: every team member writes 2–3 sticky notes per prompt before the session

Prompt 1: What do we want to KEEP from how our prior teams operated?
Prompt 2: What do we want to LEAVE BEHIND?
Prompt 3: What do we need to CREATE that neither team had?

Session structure:
- Share and cluster stickies (15 min)
- Dot vote on most important items in each category (5 min)
- Discuss top 3 per category (20 min)
- Agree on one working agreement to start with (10 min)
- Schedule follow-up retro in 2 sprints (5 min)
```

---

## Anti-Patterns

**The Surprise Announcement.**
An all-hands where the engineering team first learns about a reorg. This stays almost always avoidable; the leak risk of manager-first briefings runs lower than the trust damage of a surprise announcement. Engineers learning about a reorg from a system update or a hallway conversation before the formal announcement feel managed; the respect signal is absent.

**The Org Chart Without Context.**
A reorg communicated only as a reporting-line change, with no explanation of the rationale or intended outcome. Engineers experiencing this supply their own rationale, and the supplied rationale frequently runs worse than the actual one ("they must be preparing to reduce headcount," "they must have lost confidence in [manager]").

**The False Certainty.**
An announcement presenting the new structure as definitely correct and final. Engineers know org structures change. Presenting a reorg as permanent ("this is the right structure for where we're going") and revising it six months later produces more trust damage than an announcement framing the structure as the working answer to the current problem.

**The Skipped Re-Norming.**
Assuming team norms transfer automatically because the work is the same and some of the people are the same. They do not. Every team membership change resets some portion of the implicit operating agreement, and the reset period runs longer when unaddressed.

**The Delayed Charter.**
Running a newly structured team without a written charter for the first quarter because "we'll figure it out as we go." The figuring-out period produces real costs: scope disputes with adjacent teams, decision-making ambiguity, and onboarding friction for any new engineer joining before the charter exists.
