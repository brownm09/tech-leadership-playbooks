# Program Planning Playbook

## Leadership Context

Multi-team technical programs are where engineering leadership makes or breaks delivery credibility with the business. When a program slips, the root cause is rarely technical; it is a planning problem: scope that was never agreed to, dependencies that were invisible until they were blocking, and milestone dates that were set to satisfy a deadline without reflecting real sequencing. This playbook documents how to scope, launch, and run a program in a way that gives executives accurate visibility and gives engineers clear ownership. The output is not a Gantt chart — it is a shared understanding of what "done" means and what it will take to get there.

## Background and Motivation

This playbook was developed from the PCI environment deprecation program at ActBlue Technical Services (2022–2025) — a multi-year initiative I owned from vision through full completion. I owned the vision, the business case, the executive alignment, and the cross-team coordination across 4–5 engineering teams with stakeholders in product, compliance, legal, finance, and account operations. Outcome: full deprecation; 2,600+ accounts migrated to Stripe; 30% codebase reduction.

## When to Use This

Apply program-level treatment when:

- Three or more teams have work that must coordinate to deliver a shared outcome
- The effort spans six or more months, or crosses a fiscal boundary
- No single team owns the end-to-end result — the deliverable lives in the integration between teams
- An executive or external stakeholder is tracking the outcome and needs status without attending every team meeting

Single-team efforts with a committed roadmap slot are projects, not programs. Epics within a single team are backlog items, not programs. The distinction matters because programs require dedicated coordination overhead that is wasteful for smaller efforts.

---

## Program vs. Project vs. Epic

The wrong label creates the wrong structure. Use this framework to pick the right one before committing to planning overhead:

| Dimension | Epic | Project | Program |
|-----------|------|---------|---------|
| Teams involved | 1 | 1–2 | 3+ |
| Duration | Weeks | 1–3 months | 3+ months |
| Exec visibility needed | No | Sometimes | Yes |
| Shared milestone dates | No | Sometimes | Yes |
| Dedicated TPM or coordinator | No | No | Yes |
| Cross-team dependencies | Rare | Occasional | Defining characteristic |

**When to escalate from project to program:** The moment you draw a dependency arrow between two teams on a whiteboard and hear "we don't know when they can get to that," you have a program. Add coordination structure immediately — retrofitting program management onto a stalled effort is harder than starting with it.

---

## Program Brief

The program brief is a four-page document that must exist before the kickoff. It is not a project plan — it is the shared frame that all planning is built on. If the brief is wrong, every decision downstream is misaligned.

**Required sections:**

### Problem Statement

What problem is this program solving, and for whom? State the business outcome, not the technical deliverable.

Bad: "Migrate the payment service to the new infrastructure stack."
Better: "Reduce payment processing latency p99 from 850ms to under 200ms, eliminate the $1.2M/year Heroku contract, and meet the SOC 2 Type II audit requirement for infrastructure standardization — all before the Q4 audit window."

The problem statement must name the consequence of not doing the program. If there is no consequence, the program should not exist.

### Success Criteria

Measurable outcomes that define "done." Not activity milestones ("migrate completed") — state outcomes ("p99 latency under 200ms in production for 30 consecutive days, zero payment failures attributed to infrastructure, SOC 2 audit finding resolved").

Limit to 3–5 criteria. More than five means the scope is not bounded.

### Scope Boundary

Explicitly list what is in scope and what is out of scope. The out-of-scope list is as important as the in-scope list — it is the document you point to when someone tries to add work mid-program.

**In scope:**
- Payment service, checkout service, fraud detection service
- AWS/Kubernetes migration for these three services
- Runbook authoring and DR drill for migrated services

**Out of scope:**
- Admin tooling (separate initiative, Q1 next year)
- Notification service migration (dependency not yet resolved)
- Performance optimization beyond latency target (follow-on work after migration)

### Key Risks

List the top 5 risks. For each: what is the risk, what is the likelihood, what is the impact, what is the current mitigation. This is seeded from the RAID log (see the RAID Dependency Tracking playbook) but distilled for the brief.

Format:

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Fraud detection service has undocumented stateful behavior that breaks in Kubernetes | Medium | High — could block go-live | Spike planned in Week 2 to map state dependencies |
| Q4 hiring freeze reduces platform team capacity below plan | Low | Medium — extends timeline by 4–6 weeks | Identified two contractor options; decision gate at Week 4 |

### Team Assignments

Name the teams, their lead, and their scope of accountability within the program. Ambiguity in team assignments is the fastest way to produce work that falls between the cracks.

| Team | Lead | Scope | Milestone Commitment |
|------|------|-------|---------------------|
| Platform | Sarah Chen | AWS/Kubernetes configuration, migration tooling | Environments ready by March 1 |
| Payments | Marcus Webb | Payment service migration, integration testing | Service migrated by April 15 |
| Security | Priya Agarwal | Threat model review, pen test scope, SOC 2 evidence | Audit-ready posture by May 1 |

### Milestone Dates

A milestone is a defined, verifiable state — not "in progress" or "mostly done." The milestone has passed or it has not. State the milestone, the date, and who signs off.

| Milestone | Date | Sign-off |
|-----------|------|---------|
| Program brief approved | Jan 10 | VP Eng + CTO |
| Environments ready for migration | Mar 1 | Platform lead |
| Payment service migrated (staging) | Apr 1 | Payments lead |
| Payment service migrated (production) | Apr 15 | VP Eng |
| Audit-ready posture confirmed | May 1 | Security lead + CISO |
| Program closed | May 31 | TPM |

---

## Kickoff Structure

The kickoff is not a presentation. It is the single meeting where alignment becomes real or does not happen at all. Three things must be true when the kickoff ends:

1. **Every team knows what they own.** If someone can leave the kickoff unsure of what they are responsible for, the kickoff failed.
2. **Every team knows the first dependency they have on another team.** Not all dependencies — the first one. This surfaces blocking relationships early enough to resolve them.
3. **Everyone has seen and can challenge the success criteria.** If success criteria are defined by leadership and shown to the room for the first time in the kickoff, the room will nod and then go build something different. The kickoff is where criteria get challenged and confirmed.

### Pre-Read (Required)

Send at least 48 hours before. Must include: program brief (all sections), milestone schedule, and the dependency matrix (draft is fine). If participants have not read the brief, do not run the kickoff — you will spend the meeting explaining context instead of building alignment.

### Kickoff Agenda (90 minutes)

- **0–10 min:** Problem statement and success criteria. Read them aloud. Ask: does anyone disagree? Do this before anything else.
- **10–30 min:** Team-by-team scope walkthrough. Each team lead summarizes their scope in 3 minutes. The room confirms: is this what we expected? Are there gaps?
- **30–50 min:** Dependency review. Walk the dependency matrix. For each dependency: who produces the deliverable, who consumes it, what is the handoff date, what is the format/interface. Surface unknowns immediately.
- **50–70 min:** Risk review. Walk the top 5 risks from the brief. Ask each team lead: do you have risks to add? Who owns each mitigation?
- **70–85 min:** Working agreement. How will the program sync work? Who makes decisions vs. who is informed? What is the escalation path when a dependency is late?
- **85–90 min:** Next steps. Confirm first milestone date, first sync date, and any pre-work that must happen before Week 1 of execution.

### Anti-Patterns

**The kickoff that is really a status update.** If the teams have been working for weeks before the kickoff, you are running a retroactive alignment meeting under the kickoff label. Kickoff happens before execution begins.

**Presenting a plan to the room instead of building one.** Handing teams a completed Gantt chart and asking for questions signals that the planning is done and they are here to receive it. Alignment built this way does not last past the first dependency conflict.

**Skipping the dependency walkthrough.** Teams consistently underestimate how coupled their work is until they are forced to name their dependencies out loud. Run the dependency walkthrough; it surfaces more coordination risk per minute than any other kickoff segment.

**The 3-hour kickoff.** Attention collapses after 90 minutes. If there is more content than fits in 90 minutes, cut the content — not the time limit.

---

## Milestone Tracking

### Leading vs. Lagging Indicators

A lagging indicator tells you a milestone was missed. A leading indicator tells you 2–3 weeks in advance that a milestone is at risk.

**Lagging indicators:** milestone date passed, deliverable not complete.

**Leading indicators:**
- Key dependency has not been confirmed by the date it was supposed to be confirmed
- A team's sprint velocity is below what the milestone requires (e.g., they need to ship 12 story points per sprint to hit the milestone and are averaging 7)
- Scope is being added within a workstream without a corresponding milestone date extension
- A team has stopped surfacing blockers (often means they have stopped looking for them)

Track leading indicators weekly. When a leading indicator turns yellow, do not wait for the milestone date to surface the risk.

### What "On Track" Actually Means

"On track" has a specific definition. A workstream is on track when:
- The current sprint contains the work that the milestone plan says it should
- All upstream dependencies for the next milestone have confirmed delivery dates
- There are no open blockers that the team cannot resolve without external help
- Scope since the last sync has not increased

"On track" does not mean: work is happening, no one has raised an issue, or the team thinks they will be fine.

**Green/Yellow/Red definitions for program reporting:**

| Status | Definition |
|--------|-----------|
| Green | On track per above criteria. No changes to milestone dates. |
| Yellow | At risk. One or more leading indicators are negative, or a dependency is unresolved. Mitigation plan exists. Dates not yet changed. |
| Red | Off track. A milestone date will slip, or a dependency failure is blocking progress. Escalation required. |

Red is not a failure state — it is information. Programs that have no red workstreams for months are not managed programs; they are programs where problems are being hidden.

---

## Weekly Program Sync

### Format (15–20 minutes standing agenda)

The weekly sync is a decision meeting, not a status broadcast. Status is in the written update sent before the meeting. If teams need to read their status aloud, the sync is too long.

**Required pre-read (sent 24 hours before):**
- RAG status per workstream
- Blockers requiring cross-team resolution
- Decisions needed this week

**Standing agenda:**

1. **Blockers** (5 min): What is blocked and what does it need? No blocker should leave the meeting unowned.
2. **Dependency status** (5 min): Any dependency that is at risk of missing its date. Walk the dependency matrix. If a handoff is on time and unambiguous, do not discuss it.
3. **Decisions needed** (5–10 min): Decisions that require cross-team alignment or escalation. Assign an owner and a date for each. If the decision cannot be made in this meeting, name who makes it and when.

**What does not belong in the sync:**
- Team-level sprint updates (not a cross-team concern)
- Technical design decisions (take offline with the relevant engineers)
- Celebrations or recognition (valuable, but not in the 15-minute sync)

### Working Session (Optional, Bi-Weekly)

For programs with complex dependencies, add a 60-minute bi-weekly working session separate from the sync. This is where teams resolve technical interface questions, unblock design decisions, and do the coordination work that cannot happen asynchronously. The weekly sync reports on what the working session produced.

---

## Closure Criteria

A program is done when it is integrated, stable, and handed off — not when it has shipped.

**Shipped** means code is in production. **Done** means:

- All success criteria from the program brief have been met and verified
- Ownership of all outputs has transferred to the teams that will maintain them (runbooks updated, alerts configured, on-call rotation updated)
- Any operational anomalies observed in the first 30 days post-launch have been addressed or explicitly accepted as known issues with a plan
- The program's shared planning artifacts (dependency matrix, RAID log, milestone tracker) have been archived or closed
- Stakeholders have received a written closure summary

**Common premature closure signals:**
- "We shipped it, so we're done" — integration testing and operational stability are post-ship work
- The team that built it still owns the operational support — handoff did not complete
- Success criteria were never formally verified — someone assumed the numbers were good

### Closure Checklist

- [ ] All success criteria confirmed with measurement evidence
- [ ] Production stability confirmed for defined period (typically 2–4 weeks post-launch)
- [ ] Operational handoff complete: runbooks, monitoring, alerting, on-call
- [ ] Program artifacts archived (RAID log closed, milestone tracker frozen)
- [ ] Written closure summary sent to all stakeholders
- [ ] Retrospective completed (see below)
- [ ] Any open items from the program either resolved or transferred to a named owner with a follow-up plan

---

## Program Retrospective

Run the retro within two weeks of program closure. Delay kills memory; waiting a month produces a retro of vague impressions, not specific events.

### Attendees

Program leads from each team, the TPM, and the executive sponsor (for 30 minutes at the end, not the whole retro).

### Format (90 minutes)

**Pre-work:** Each participant submits 3–5 items per category to a shared doc before the meeting.

Categories:
- What enabled this program to succeed?
- What slowed us down?
- What would we change in how we planned?
- What would we change in how we coordinated?
- What did we assume that turned out to be wrong?

**Meeting:**
- 30 min: Review and cluster submitted items. Identify themes.
- 30 min: Discuss the top 3–5 themes in depth. Name the root cause, not the symptom.
- 20 min: Produce 3–5 concrete changes to make to the next program. Each change has an owner.
- 10 min: Executive sponsor join. Share top 3 themes and changes. Get feedback.

**Output:** A 1-page retro summary with themes, root causes, and changes committed. Distribute to all participants and archive with program artifacts.

---

## Common Failure Modes

**Scope creep without scope decisions.** New work enters the program because no one said no. The fix is a documented scope boundary in the brief, and a process for approving scope changes. Any scope addition must name what it displaces (another deliverable, a date, or team capacity). Scope additions that displace nothing are not real additions — they are magical thinking.

**Phantom dependencies.** Dependencies that are listed but not actively managed. The dependency matrix says Team A needs output from Team B by March 15, but no one has spoken to Team B about it since the kickoff. Phantom dependencies surface late — when Team A goes to use the output and it does not exist. Fix: weekly dependency review with explicit confirmation from the producing team.

**The coordinator tax.** When the TPM becomes the information bottleneck — all updates flow through them, all decisions wait for them, all coordination is synchronous with them in the loop. The right model is a program infrastructure that enables asynchronous coordination: a shared status document that teams update directly, a RAID log that any lead can update, and a sync format that surfaces only what requires the group's attention. A TPM who is needed for every communication is a single point of failure, not a coordination asset.

**Milestone date anchoring.** Dates set in the program brief based on a desired outcome, not on a bottoms-up estimate from the teams doing the work. The earliest symptom is that no team can explain how the milestone date was derived. The fix is to build the milestone schedule from team-level estimates, then have the conversation about the gap between what teams can realistically deliver and what leadership wants — not paper over it.

**The program that is really three projects.** Three separate efforts packaged under a shared banner for executive visibility, but without genuine interdependencies or shared outcomes. These do not benefit from program-level coordination — they benefit from three separate project plans and a shared reporting roll-up. Running them as a program adds overhead without adding value. Dissolve these when you identify them.

## Further Reading

| Topic | Resource |
|---|---|
| Program planning methodology | PMI, *A Guide to the Project Management Body of Knowledge (PMBOK Guide)*, 7th ed. (PMI, 2021) |
