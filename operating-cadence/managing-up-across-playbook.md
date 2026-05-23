# Managing Up and Across Playbook

## Leadership Context

Engineering leaders who run technically strong and operationally disciplined often stall at the senior manager or director level for one reason: they treat the relationships above and beside them as administrative overhead, when the relationships themselves are the actual work. Managing up describes how you secure the resources, air cover, and strategic alignment your team needs to operate; the politics framing misses the function. Managing across describes how you keep your org from becoming a local optimization machine creating drag for every team depending on you; coordination understates the stakes.

Both relationships function as skills. They carry mechanics, failure modes, and a practice regimen. This playbook covers both.

## Background and Motivation

This framework draws on the cross-functional program work at ActBlue Technical Services (2022–2025), where I owned a multi-year PCI environment deprecation program requiring sustained alignment across engineering, legal, finance, product, and account operations, alongside management of a platform directorate of approximately twenty engineers across six teams. The challenges of managing up (maintaining executive visibility and trust through a multi-year program with shifting compliance deadlines) and across (coordinating four to five of those teams on the PCI program directly, along with external audit timelines and non-technical stakeholders carrying different incentives) ran parallel and interdependent. Mismanaging either created cascading problems in the other.

---

## Part 1: Managing Up

### The Trust Account Model

Your relationship with your skip-level (the person above your manager) and with your manager functions as a trust account. You make deposits through: delivering on commitments, surfacing problems early instead of late, communicating clearly without over-engineering every message, and making their job easier. You make withdrawals every time they get surprised by something they should have known, every time they have to re-explain your team's work to someone else because they never fully understood it, and every time a decision belonging at your level lands on their desk.

The goal does no work on a high balance; it builds a steady deposit habit. Most withdrawals in a healthy relationship run small and expected. A single large surprise withdrawal can take months of deposits to recover from.

### Building the Exec Relationship

Do not wait for formal check-ins to build the relationship with your skip. The formal check-in operates as where you report on things; the relationship gets built in the informal moments: a brief Slack message when something important lands, a 5-minute walk between meetings, a direct note after a difficult decision.

Three practices that build exec relationships reliably:

1. **Lead with the conclusion.** Executives do not have time to reconstruct your reasoning. State the conclusion first, then offer the reasoning. "We should delay the migration by one quarter. Here's why: [two sentences]." Not: "So I've been looking at the migration timeline and there are a few factors to consider..."

2. **Name trade-offs over problems.** A problem requires someone else to solve it. A trade-off requires a decision. "The payment processor migration runs on track, but accelerating it to meet Q2 creates risk to the fraud detection refactor in the same sprint. I can handle either at the original timeline; both becomes impossible. Which do you want me to protect?" This frames you as the person managing the tension; surfacing it sets the floor, and managing it sets the ceiling.

   In practice: during the PCI deprecation at ActBlue, when an external compliance deadline shifted mid-program, the framing to the VP of Engineering avoided "the timeline changed." It ran instead: we can hit the new compliance date, but the payment processor integration slips a quarter; the compliance date is non-negotiable from a regulatory standpoint, the integration date is a business decision, which risk do you want me to protect? That framing put the decision in the right hands and produced a resolution in the same conversation.

3. **Give them something to say.** Your skip will talk about your team's work in contexts you do not attend. Give them the two or three talking points making your team's work legible to that audience. If they have to reconstruct your team's value proposition from memory, the version they communicate will diverge from the version you would choose.

### Skip-Level Relationships

Your skip-level acts as neither ally nor authority; they act as a stakeholder. Manage the relationship accordingly:

- **Request time when you have something to bring**; check-ins without content fall short. Skips who routinely hold 30-minute 1:1s with every sub-org leader will see those compressed or canceled. Come with a question, a decision needing them, or an update genuinely useful to them.
- **Do not go around your manager.** If you carry a concern about your manager, address it with your manager first. Going directly to the skip without exhausting the direct conversation withdraws from both accounts simultaneously.
- **Know their priorities.** What does the skip-level worry about right now? What does the board ask about? What metric do they face pressure on? Your team's work reads as more legible when framed in terms of what already sits on their mind.

### Absorb vs. Escalate

Most decisions that could go up should not. The escalation test: does this decision require authority, relationships, or information I genuinely do not have? If yes, escalate. If it requires time, discomfort, or a hard trade-off I am trying to avoid, absorb it; escalation does not apply.

Common decisions that should be absorbed:
- Whether to delay a feature to address technical debt (you have the information to make this call)
- Whether to extend a sprint due to unexpected complexity (same)
- Personnel performance issues that are in early stages (handle them at your level until they require formal process)
- Cross-team prioritization conflicts that you can resolve directly with the other manager

Common decisions that warrant escalation:
- A commitment that will be missed, when the miss affects a stakeholder relationship your manager owns
- A personnel situation where HR process is required
- A strategic trade-off where two or more senior leaders' priorities are in direct conflict and only someone above them can adjudicate
- Any decision that, if wrong, would be irreversible and whose consequences extend beyond your scope

When you escalate, escalate with a recommendation. "I don't know what to do here" reads as a request for your manager to do your job. "Here's what I recommend, here's the alternative, here's the risk of each, and here's my ask" reads as an escalation respecting their time and positioning you correctly.

### Protecting Your Team's Capacity

One clear signal of a leader who manages up well: their team does not feel the chaos from above. Cross-functional requests, executive whims, and urgent-but-low-priority asks arrive constantly. Your job runs to filter.

Practical mechanics:
- **Run a weekly intake process.** New requests go into a queue; the team does not receive them directly. Review the queue weekly against the roadmap and capacity. Most urgent requests will have resolved themselves by the time you review them.
- **Name the trade-off explicitly.** When a new request genuinely must be prioritized, do not absorb it silently; communicate what moves. "We can take this on, but the fraud detection refactor moves to Q3. Is that the trade you want?" This keeps leadership accountable for the trade-offs they implicitly make when they add scope.
- **Do not let your team serve as the cushion.** Sprint extensions, weekend work, and heroics become necessary on occasion. They serve as no substitute for adequate scope management at the leadership level.

---

## Part 2: Managing Across

### The Peer Manager Relationship

Your peer engineering managers do not function as competitors, nor do they function as automatic allies. Peer relationships rest on reciprocity: you help them when they need it, you stay honest with them when their work creates problems for you, and you do not go over or around them in a way making them look bad to their team.

Common failure modes in peer manager relationships:
- **Scope creep without negotiation.** Your team starts owning work bleeding into another team's domain without an explicit conversation.
- **Public disagreement.** Raising concerns about a peer's team or decisions in a forum where their team appears, instead of directly.
- **Asymmetric information.** Sitting in a planning conversation affecting a peer's team without including them, then presenting the decision as fait accompli.

Practices maintaining healthy peer relationships:
- **Direct channels first.** If a peer's team creates a problem for yours, message the manager directly before raising it in a shared forum. This runs almost always faster and preserves the relationship.
- **Shared language for cross-team issues.** When two teams hold a dependency or a conflict, agree on a shared framing before it goes to leadership. "Team A needs X from Team B by Y date for Z reason" runs more useful than two competing stories about the same situation.
- **Credit generously.** When cross-team work goes well, make the other manager's contribution visible to their manager and your shared skip. This costs you nothing and builds significant trust.

### Cross-Functional Partnership: Product

The engineering/product relationship carries higher consequence than any other cross-functional relationship in most tech orgs, and gets damaged more often than any other.

The structural tension: product manages outcomes (what gets built and when), engineering manages execution (how and at what cost). In a healthy relationship, these two functions inform each other continuously. In an unhealthy one, product delivers requirements and engineering delivers dates, and both parties get surprised when neither lands.

Mechanics that work:
- **Joint roadmap ownership.** Engineering should hold a seat in roadmap planning, beyond roadmap review. The cost of a feature extends past the sprint points: opportunity cost, technical debt incurred, on-call implications. Engineering stands as the only party who can accurately represent those costs.
- **Early technical input on specs.** When product writes a PRD, engineering input during the spec phase (post-spec falls short) prevents the "we built the wrong thing" problem. A two-hour technical consultation at spec time saves ten sprint days of rework.
- **No surprises on timeline.** When a commitment runs at risk, tell the product manager before it slips. The earlier the warning, the more options they hold to adjust scope, timeline, or priority.

### Cross-Functional Partnership: Legal and Compliance

Legal and compliance often get treated as obstacles. They function more accurately as early-warning systems holding organizational authority to block things and preferring to use it sparingly.

The key insight: legal and compliance teams almost always hold a workable path; they need to understand what you are trying to do and why before they can help you find it. Going to them with a completed design and asking for approval loses. Going to them at the problem statement stage and asking "what constraints should we design around?" produces a different conversation.

For compliance-adjacent programs (PCI, GDPR, SOC 2): own the relationship, beyond the deliverables. Know your compliance lead by name. Know their concerns and timeline pressures. When the audit approaches, they should be calling you; the other direction signals failure.

In practice: during the PCI deprecation at ActBlue, the conversation with the compliance team started at the problem-statement stage with a question about constraints, instead of at the approval stage with a completed scope-reduction design. That early framing surfaced compliance constraints at design time; surfacing them at the approval stage would have triggered late-stage rework.

### Cross-Functional Partnership: Finance

Finance cares about two things: accuracy and surprise avoidance. Your job in the finance relationship: give them accurate numbers and early warning when those numbers are changing.

The headcount request, the infrastructure cost estimate, and the vendor contract renewal all describe documents finance will scrutinize. The ones surviving scrutiny carry: a clear business case (what problem does this solve), a cost model (the total cost of the decision, beyond the sticker price), and a plan for what happens when the approved number falls short.

Do not go to finance with a number you made up and hope they do not ask questions. They will ask questions. Go with a range, a methodology, and an acknowledgment of the assumptions baked in.

### Dependency Navigation

When your team holds a dependency on another team (or another team holds a dependency on yours), the default failure mode runs: both teams treat the dependency as the other team's problem. The dependency sits unresolved until it blocks someone, at which point both teams react with surprise and resentment.

The practice:
- **Name the dependency in writing at the time of creation.** Who owns what, by when, and what blocks it. Not in a meeting; in a RAID log, a ticket, or a shared document both managers can see.
- **Weekly dependency check.** During whatever stand-up or sync runs across the teams, explicitly review blockers and dependencies. Treat an unresolved dependency as a blocker; nice-to-have follow-up framing understates the cost.
- **Escalate early.** If a dependency is not moving and the deadline runs real, escalate before the deadline. The earlier the escalation, the more options exist for resolution.

### When to Escalate a Cross-Team Issue vs. Solve It Yourself

Escalate when:
- You and the other manager have reached an impasse and the decision requires authority above both of you
- The timeline impact of the unresolved issue will affect a commitment leadership owns
- The issue involves a resource or priority two orgs are competing for and only one person can adjudicate

Solve it yourself when:
- You have not yet tried a direct conversation with the peer manager
- Changing scope, timeline, or priority at the team level resolves the issue without affecting shared commitments
- Escalation would burn relationship capital with shared leadership for a problem solvable locally

Most cross-team issues that get escalated did not need escalation. Most that were never escalated needed it.

---

## Anti-Patterns

**The Status Theater Update.** Sending detailed weekly updates to your manager communicating a lot of information and very little understanding. Updates should run curated: here is what matters, here is what changed, here is what I need.

**Managing Down Instead of Up.** Spending all available energy on team-facing work and treating the exec relationship as secondary. This feels comfortable and productive; it also caps how much your team can accomplish, because the resourcing, prioritization, and air cover they need flow from the relationship you are failing to invest in.

**The Peer Escalation Bypass.** Going to your shared manager about a peer's team before trying to resolve it directly. Even when your concern reads valid, this approach damages the peer relationship and signals to leadership that you cannot handle lateral problems.

**Alignment Theater.** Scheduling cross-functional syncs failing to surface the real tensions. The real tensions get resolved in the hallway (or Slack) by the people willing to name them; the sync becomes a performance of alignment. Run syncs where the agenda explicitly includes: what is blocking us, what do we disagree about, what needs a decision.

**The Invisible Trade-Off.** Absorbing a cross-functional request without naming what moved to accommodate it. When you absorb scope silently, you give leadership no information about the cost, and the same pattern repeats. Name the trade-off every time.
