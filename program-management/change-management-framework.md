# Change Management Framework

## Leadership Context

When technical and organizational changes fail, the root cause is rarely technical; people do not know why the change is happening, do not trust the process, or cannot see what the change means for them. A migration that is technically complete but operationally unused has not succeeded. A reorg that was announced but never reinforced reverts within a quarter. This framework applies to the transitions that engineering leaders must manage at the org level: infrastructure migrations, process overhauls, tooling consolidations, and organizational restructuring. The business outcomes at stake are adoption rate (does the change stick?), rollback risk (do we have to undo it?), and people trust during uncertainty (do teams remain productive through the transition?). All three are leadership problems before they are technical ones.

## Background and Motivation

This framework was developed from two change management deployments:

1. **Raytheon / NASA EED/2 (2018–2019):** I led a Waterfall→Kanban→SAFe transition for a team of 20+ personnel. I conducted working groups, adapted ceremonies for the team's context, and built the JIRA migration tooling to replace the legacy Waterfall tooling.

2. **ActBlue Technical Services (2022–2024):** I applied the same change management posture to the LaunchDarkly rollout — driving cross-team adoption from mandate to first trivial flag in production within 4 months and the first non-trivial customer-facing flag at 7 months — and to the on-call restructuring, which required redesigning roles and handover processes across multiple teams simultaneously.

## When to Use This

Apply this framework when:

- A reorg or role change affects more than one team
- A major technology migration requires teams to change how they work (new CI/CD platform, new data stack, new observability tooling)
- A process change is being mandated, not voluntarily adopted (new incident process, new code review standard, new release gate)
- A tooling consolidation eliminates or replaces a tool that teams rely on
- A compliance-driven change requires behavioral change from engineers (new secret management process, new data handling policy)

For incremental improvements adopted at a team's own pace, the overhead of a structured change process is not warranted. This framework applies when the change is imposed, time-bounded, or affects teams that did not choose to make it.

---

## Change Types and Their Distinct Needs

Not all changes are managed the same way. The type of change determines which levers carry the load.

### Technical Changes: Migrations and Deprecations

**Examples:** Cloud provider migration, CI/CD platform replacement, observability tooling consolidation, database engine upgrade.

**Primary risks:** Technical continuity (does the new thing work as well as the old thing?), adoption (do teams switch, or do they maintain parallel workflows indefinitely?), and knowledge gaps (do teams know how to operate the new system?).

**What carries the load:** A clear cutover date with enforcement, hands-on enablement (documentation alone is insufficient for tooling changes), and a deprecation timeline that is announced well in advance and honored.

**What does not work:** Soft deadlines ("we'd like to be on the new platform by Q3"), migrations where the old system remains fully available indefinitely, and training that consists of a one-time Zoom session with no follow-up.

### Process Changes: New Workflows and Standards

**Examples:** New incident management process, code review standards, architectural review gate, launch readiness checklist.

**Primary risks:** Compliance theater (teams perform the process without internalizing it), inconsistent adoption across teams, and rollback by attrition (the process is followed for 6 weeks, then quietly abandoned).

**What carries the load:** A compelling "why" that connects the process change to outcomes engineers care about (the compliance rationale alone does not land with the people doing the work), visible enforcement without micromanagement, and feedback loops that allow the process to evolve based on experience.

**What does not work:** Process changes announced as mandates with no rationale, processes that add work without visible benefit to the people doing the work, and changes that are never reinforced after the initial announcement.

### Organizational Changes: Reorgs and Role Changes

**Examples:** Team restructuring, engineering function split or merge, reporting line changes, role redefinition.

**Primary risks:** Productivity loss during transition, talent attrition from people who feel their role was diminished, and loss of informal coordination that was built around the previous structure.

**What carries the load:** Individual conversations with directly affected people before the announcement, honest answers to the real questions people have (career impact, team relationships, reporting structure), and a post-announcement stabilization period with explicit leadership presence.

**What does not work:** "Change by announcement" — communicating a reorg in a large group meeting or email with no prior individual outreach to affected people. This approach prioritizes operational efficiency over the people it affects and is consistently the primary source of attrition during reorgs.

---

## The 5-Phase Change Model

### Phase 1: Diagnose

**Goal:** Understand the current state, the case for change, and the stakeholder landscape before designing the change.

**Activities:**
- Document the current state: what exists today, who depends on it, what pain or risk it creates
- Build the case for change: what business outcome does this change produce? What is the cost of not changing? (If the cost of not changing is low, the change should not be a mandate.)
- Map the stakeholder landscape: who is affected? Who will resist? Who are the informal influencers whose buy-in will move others?
- Identify the non-negotiables: what aspects of the change cannot flex? A migration to a new cloud provider for compliance reasons cannot be optional. A preferred coding style can be.

**Output:** A 1-page change brief: problem, case for change, success criteria, scope, stakeholders, and non-negotiables.

**Common mistake at this phase:** Designing the change before diagnosing the current state. The result is a change that solves the problem leadership perceives while leaving the real source of friction in place.

### Phase 2: Design

**Goal:** Define the change and the path to it, including how affected people will be supported through the transition.

**Activities:**
- Define the desired end state (what does success look like 90 days after the change is complete?)
- Map the transition path: what must happen for the org to move from the current state to the end state? What is the sequence? What are the dependencies?
- Design the support structure: training materials, documentation, office hours, champions program (designating a point person per team who receives extra support and serves as the team-level resource)
- Define the rollback criteria: at what point would the change be acknowledged as failing? What would trigger a reversal or a significant design modification?
- Set the timeline: when does the transition begin? When does enforcement begin? When does the old state become unavailable or unsupported?

**Output:** Change plan — 2–3 pages. Sections: desired end state, transition path (phased), support structure, timeline, rollback criteria.

**Common mistake at this phase:** Designing the change without input from the people most affected. This produces a change that is technically logical but functionally ignores the realities of how work is done. Include representatives from affected teams in the design phase — not to give them veto power, but to surface gaps the designers cannot see.

### Phase 3: Align

**Goal:** Build the shared understanding and commitment that the change requires before execution begins.

**Activities:**
- Communicate the "why" broadly and repeatedly before the "what" (the details of the change). People who understand the reason for a change tolerate its friction better than people who have only been told what to do.
- Hold individual conversations with highly affected people before the broad announcement. This is non-negotiable for organizational changes. For technical and process changes, do it with team leads and informal influencers.
- Collect and respond to concerns — design a feedback mechanism (see the Feedback Loop Design section) that gives people a structured way to raise concerns without producing an endless open comment period.
- Confirm alignment with each team lead explicitly: do they understand the change? Do they have what they need to lead it within their team?

**Output:** Stakeholder alignment confirmation. Each decision-maker and influencer has been briefed, their concerns have been heard and addressed (or explicitly acknowledged as unable to be addressed with reasoning), and they are prepared to support the change within their teams.

**The alignment trap:** Alignment does not mean consensus. Some changes will not have consensus. Alignment means the affected people have been heard, their concerns are understood, and the decision has been made with that input incorporated. Waiting for everyone to agree produces paralysis. The goal is informed commitment; universal approval is a different and unreachable bar.

### Phase 4: Execute

**Goal:** Implement the change in a way that manages disruption and maintains trust.

**Activities:**
- Launch with a clear, consistent message (see the Communication Plan template below)
- Implement in phases where possible: pilot with one team before rolling out broadly; use a canary approach for technical changes
- Maintain high visibility of leadership during the transition — this is not the time for engineering leaders to become less accessible
- Track adoption weekly (see Success Metrics section)
- Surface and resolve issues in real time; do not accumulate problems and address them in batches
- Honor the support commitments made during the design phase (training, office hours, champions) — cutting support at this stage signals to the org that the commitment was performative

**Common mistake at this phase:** Launching and then disappearing. A change that is announced, launched, and then left to self-execute without active reinforcement will revert. The execution phase requires active leadership presence through the entire transition window.

### Phase 5: Reinforce

**Goal:** Embed the change into normal operations so it persists without ongoing active management.

**Activities:**
- Update team norms, documentation, and onboarding materials to reflect the new state as the default (not "the new process" — "the process")
- Remove the old state: if the old tool is still available, some people will continue using it. Deprecate and remove on the timeline committed in Phase 2.
- Recognize and celebrate adoption publicly — name teams and individuals who have successfully adopted the change; this signals that the change is permanent
- Close the feedback loop: what concerns were raised during execution? What was addressed? What was not addressed and why? Publish a brief summary.
- Conduct a 90-day retrospective: is the desired end state being achieved? What metrics are improving? What is not working?

**The most common reinforcement failure:** The change succeeds in Phase 4 and leadership moves on. Three months later, the old practices have crept back because they were never formally closed. The reinforce phase is what separates a change that lasts from a change that requires re-running the same process 18 months later.

---

## Stakeholder Resistance Model

Resistance to change comes from three root causes. Diagnosing the actual cause determines the right response.

### Root Cause 1: They Do Not Understand Why

**Symptom:** Questions like "why are we doing this?" and "who decided this?" appearing weeks after the announcement.

**Underlying cause:** The "why" was communicated once, in a broad announcement, and did not connect to things the person cares about. Engineers who are skeptical of management decisions filter announcements for subtext. If the "why" does not ring true, they discount it.

**Response:**
- Communicate the case for change at the team level; an all-hands alone does not land it. The team lead walking through the "why" carries more credibility than a VP email.
- Connect to outcomes that matter to the affected engineers: if the migration reduces deploy time from 45 minutes to 8 minutes, that is the "why" for engineers, not the compliance rationale.
- Repeat the "why" in multiple formats (written, verbal, in 1:1s for skeptics) for the first 4 weeks.

### Root Cause 2: They Do Not Trust the Process

**Symptom:** "We've been through this before and it didn't work" or "the timeline is unrealistic" or "they didn't ask us before deciding."

**Underlying cause:** Past change processes have failed, been reversed, or been designed without input. The org has accumulated change debt — resistance that builds up when change is repeatedly done poorly.

**Response:**
- Acknowledge the history explicitly. If past changes failed, say so and describe what is different about this approach. Pretending the past did not happen produces cynicism, not alignment.
- Include affected teams in the design phase. The change does not need to be co-designed, but the affected people need to have had a real opportunity to shape it.
- Keep every commitment made during design and execution. Process trust is rebuilt one kept commitment at a time; one broken commitment undoes weeks of progress.

### Root Cause 3: They Do Not See What Is in It for Them

**Symptom:** Questions about career impact, workload, and whether the new process makes their job harder or easier.

**Underlying cause:** The change was framed from the org's perspective (compliance requirement, cost reduction, standardization) without a credible account of what it means for the individual.

**Response:**
- Answer the "what does this mean for me?" question directly and specifically for each affected role. Do not make people infer the personal implication from an organizational rationale.
- Be honest when the change is net-negative for some people (more work, loss of familiar tools, role scope reduction). Acknowledging it builds more trust than papering over it.
- For organizational changes, address career impact questions in individual conversations before the broad announcement.

---

## Communication Plan Template

| Audience | Key Message | Channel | Timing | Owner |
|---------|------------|---------|--------|-------|
| Engineering team leads | Why the change is happening, what success looks like, what their team is expected to do, support available | 1:1 conversation + written summary | 2 weeks before broad announcement | VP Engineering |
| All engineers | What is changing, when, why, and where to get help | All-hands (verbal) + written Confluence post | Announcement day | VP Engineering + TPM |
| Affected team daily standup | Team-specific impact, questions answered in real time | Team standup (verbal) | Week 1 and Week 2 of execution | Team lead |
| Executive stakeholders | Status, adoption metrics, risks | Weekly status update | Weekly during execution | TPM |
| Support / customer-facing teams | If the change is visible externally — what customers may notice and how to respond | Email + briefing session | 1 week before launch | Program lead |

**Timing principle:** Communicate the "why" before the "what." Communicate the "what" before the "when." People who learn the "when" before the "why" react to the deadline, not the rationale.

---

## Feedback Loop Design

Unstructured feedback ("if you have concerns, reach out to the team") produces one of two outcomes: silence (people do not bother because they do not believe it will change anything) or a flood of unstructured input that is impossible to action. Design the feedback loop explicitly.

**Structured collection options:**

1. **Office hours:** 30-minute open sessions twice per week during the first 4 weeks of execution. Any engineer can attend. The TPM or engineering lead takes questions. Decisions from office hours are published in a shared FAQ document within 24 hours.

2. **Async Q&A doc:** A shared document where anyone can add questions anonymously. The program lead answers weekly. Visible to all. This is the most efficient format for high-volume changes — questions cluster, and answering one question answers many people simultaneously.

3. **Team lead feedback loop:** Team leads collect concerns from their teams and bring them to the weekly sync. This works well for concerns that require discussion; a direct written answer would not resolve them.

**What to do with feedback:**

- Publish a log of questions raised and answers given. This is visible evidence that concerns are being heard.
- Distinguish between concerns that will change the approach and concerns that have been heard but will not change the approach. Be explicit about which is which.
- Close the feedback loop at 30 and 60 days: publish a summary of what was raised, what changed as a result, and what did not change with reasoning.

**The endpoint:** Feedback collection cannot be open-ended. After 60 days, move from active feedback collection to normal channels (team syncs, manager 1:1s). Signal this explicitly: "We are closing the structured feedback period. The change is now part of normal operations. Concerns go through your manager or to [channel]."

---

## Rollback Criteria

Both technical and process changes can fail. Define the rollback criteria before the change launches — not in the moment when you are under pressure and the sunk cost feels too high to acknowledge.

**Criteria for acknowledging a technical change is failing:**

- Adoption rate below 30% after 60 days of active enforcement
- Measurable degradation in a key metric attributable to the change (deploy frequency, incident rate, on-call load) that has not improved after 30 days of operation
- Three or more teams independently escalating that the new system is causing productivity losses

**Criteria for acknowledging a process change is failing:**

- Compliance theater: teams are performing the process in name only, and the outcomes the process was supposed to produce are not materializing
- Significant increase in the metric the process was supposed to improve (e.g., incident rate goes up after implementing a new incident process)
- Team leads reporting that the process is generating more overhead than value

**What rollback means for technical changes:** It does not always mean reverting to the old system. Rollback may mean: pausing the migration and running parallel systems while the issues are resolved, reducing the scope (migrating 3 of 6 services instead of all 6), or replacing the target system with a different option.

**What rollback means for process changes:** Process rollback is usually a redesign, not a return to the previous state. If the new incident process is failing, the response is typically: identify specifically what is not working, redesign those elements, and re-launch — not abandon the process and return to the previous ad hoc approach.

**The hardest call:** When to acknowledge a reorg is failing. Signs include: key people leaving, cross-team coordination breaking down in ways that are attributable to the structure change, and the informal org reassembling the old structure through workarounds. Acknowledging this early and making adjustments is less expensive than maintaining a failing structure for a full year. The signal is usually in attrition and escalation patterns — watch both closely in the first 90 days.

---

## Success Metrics

Track these three signals throughout execution.

### Adoption Rate

The percentage of affected teams or users operating in the new state. Defined specifically per change type:

- For tooling migrations: percentage of teams whose production deployments are running through the new system (not the old one)
- For process changes: percentage of completed work that followed the new process (e.g., percentage of incidents that used the new incident process)
- For organizational changes: percentage of team leads reporting active collaboration through the new structure (survey-based)

**Target:** 80% adoption by end of Phase 4 (execution). 100% by end of Phase 5 (reinforce), or explicit documentation of exceptions.

### Ticket Volume

Operational friction from a change shows up as support tickets, Slack questions to the program team, and escalations. Track the volume of change-related support requests week over week.

**Expected pattern:** Volume peaks in week 2–3 of execution, then declines as enablement materials mature and the org builds familiarity. A volume that does not decline after week 4 signals that the support structure is insufficient or the change is generating unanticipated friction.

**Threshold:** If change-related support volume is still high at week 6, run a root cause analysis: is the enablement material inadequate, or is the change itself creating issues that need to be addressed?

### Sentiment Signal: One-Question Pulse Survey

At week 4 and week 8 of execution, send one question to all affected engineers:

> "On a scale of 1–5, how confident are you that [the change] is making your work easier or better? (1 = it is making things significantly harder; 5 = it is clearly an improvement)"

Average score target: 3.5 or above by week 8. Below 3.0 is a signal for active diagnosis — what is driving the low scores? The survey is anonymous. Results are published to the full affected group.

This single-question survey takes 10 seconds to answer, produces actionable signal, and signals to the org that leadership cares whether the change is working for the people it affects.

---

## Common Failure Modes

**Change by announcement.** The change is communicated in a company-wide email or all-hands, and the assumption is that awareness equals adoption. It does not. Announcement is Phase 3; execution and reinforcement are where change happens. Orgs that confuse announcement with execution consistently see changes fail to stick.

**Skipping the "why."** Mandating a change without a credible rationale creates compliance without commitment. Engineers who do not understand the "why" will find workarounds, perform the process in letter while abandoning the spirit, or leave when a compelling alternative arises. The "why" is a precondition for adoption, not a courtesy.

**Launching too many changes simultaneously.** Every change competes for organizational attention. An org in the middle of a reorg, a major platform migration, and a new incident process simultaneously is an org where all three changes will be slower, messier, and more expensive than any one would be alone. Sequence changes deliberately. The rule of thumb: no more than one high-friction change per team at a time.

**The support that does not get delivered.** The change plan committed to office hours, champions, and training. The office hours had low attendance after week 2, so they were cancelled. The champions program was never resourced. The training was a one-time session that is no longer discoverable. Support commitments that are not delivered signal that the commitment was performative — and erode trust in the next change process.

**Measuring the wrong thing.** Tracking completion ("the migration is 80% done") instead of adoption ("80% of teams are operating in the new state"). A migration that is technically 80% complete but has 30% adoption is failing in the metric that matters. Adoption is the outcome; completion is a leading indicator.

## Further Reading

| Topic | Resource |
|---|---|
| Change management methodology | Kotter, *Leading Change* (Harvard Business Review Press, 1996) |
| Individual change adoption (ADKAR) | Prosci ADKAR Model (Prosci, 2006) |
