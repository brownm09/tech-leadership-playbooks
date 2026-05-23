# Career Ladder and Calibration Playbook

## Leadership Context

A career ladder carries weight beyond its HR-artifact status; it is the primary mechanism by which an engineering organization makes compensation, promotion, and retention decisions consistently at scale. Without a shared level definition, managers in the same org will promote different engineers for different reasons, pay bands will drift, and high performers will leave when they conclude that advancement depends on who their manager is. This playbook covers building a ladder from scratch, running calibration sessions that hold up under scrutiny, and handling the failure modes that quietly undermine both.

The business outcome is straightforward: equitable promotion decisions reduce regrettable attrition, reduce the cost of external hiring (replacing a mid-level engineer typically runs 0.5–1x their annual salary in recruiting and ramp costs), and give managers a shared vocabulary for growth conversations that do not depend on a single person's judgment.

## Background and Motivation

This playbook was developed from the talent development work at ActBlue Technical Services (2022–2025). I drove promotions for three engineers within three years through deliberate project placement, calibration preparation, and sustained advocacy; two of the three were promoted twice each (from SE2 to SSE2), and one was promoted from SSE to SSE2. Five total promotion events across the three. I also founded and ran the Engineering Management Community of Practice, which systematized calibration coaching, talent development, and recruiting partnership across the manager group.

## When to Use This

**First career ladder.** The org has been making level decisions informally — "Senior" means whatever the hiring manager decided at offer time, and there is no shared standard for what promotion requires.

**Inherited inconsistent levels.** Common after rapid headcount growth (50+ engineers in under 18 months) or after a Director transition. You inherit a team where two engineers with the same title are doing meaningfully different scopes of work, and no one can explain why.

**Post-acquisition level normalization.** The acquired team has its own ladder — often with different level names, different anchors, and different compensation bands. Before you can manage across the combined org, you need a common frame.

**Pre-performance cycle pressure.** The org is running its first formal performance cycle and managers lack calibration anchors. Without a shared frame, every calibration session will devolve into argument about edge cases.

---

## Part 1: IC Track vs. Manager Track

### When to Fork the Ladder

Fork the IC and manager tracks at the level where pure technical execution is no longer the primary job. For most engineering organizations, this is the transition from Senior (L5 equivalent) to Staff/Lead (L6 equivalent) on the IC side, and from Senior to EM or TL/M on the manager side.

The fork matters because the leadership behaviors required to be an effective Director of Engineering are not a superset of what makes a great Staff Engineer. Both paths lead to significant organizational influence; the tracks run parallel, without one sitting above the other. Treating them as hierarchical (IC as "lesser" than management) produces exactly the wrong incentive: technical leaders feel they must enter management to advance, even when that is not where they create the most value.

The fork should be explicit in the written ladder. Show both tracks side by side at the Senior level so that the choice is visible and deliberate; engineers should not discover it after the moment to change course has passed.

### What Staff+ Looks Like in Practice

Career ladders go vague and unhelpful at Staff Engineer. Descriptions like "leads technical direction" or "drives cross-team alignment" are true but not operational. Here is what Staff-level work looks like in a mid-size engineering org (100–500 engineers):

- **Initiative scope:** Owns a technical problem that spans at least two teams and has a meaningful business dependency. At ActBlue, this looked like: driving the migration of payment processing from a legacy monolith to an event-driven architecture, coordinating across the payments team, platform team, and compliance team over 18 months.
- **Ambiguity tolerance:** Given a problem with no specified solution, produces a structured proposal with trade-offs, a recommended path, and a timeline — without requiring a manager to decompose it first.
- **Manager multiplier:** A Staff Engineer's work makes the adjacent engineering managers more effective. They surface problems early, translate technical risk into business language, and write the design docs that make review and onboarding faster.
- **Selective escalation:** Knows which decisions to make autonomously and which decisions require stakeholder buy-in. Gets this right more often than not, even under deadline pressure.

Staff+ is not a reward for tenure. It is a description of the work the person is already doing. If an engineer is not already operating at this scope, the promotion is a bet on a change that has not happened yet — and those bets are expensive when they do not pay off.

---

## Part 2: Level Definition Format

### Why Not Skill Checklists

Skill checklists ("writes idiomatic Go," "can design a REST API") are easy to game and hard to evaluate fairly. An engineer who has been at the company two years and worked on Go exclusively will pass every Go-related item. An engineer hired six months ago from a Python shop may be a more sophisticated engineer but will miss every language-specific criterion.

The definition format that holds up in calibration is structured around four axes:

1. **Impact scope** — What is the scale and consequence of the work this person owns?
2. **Decision autonomy** — What can this person decide without escalating? What does escalation look like when they do it?
3. **Execution expectations** — What does "done" look like at this level, and over what time horizon?
4. **Collaboration expectations** — Who does this person lead, influence, and coordinate with?

These axes force the discussion toward evidence — "what did this person do?" — and away from abstract capability assessments.

---

## Part 3: Example Level Definitions

### L3 — Software Engineer

**Impact scope:** Owns individual features and bug fixes within a single, well-scoped system. Work is reviewed and guided by L4+ engineers. Mistakes are caught in code review or testing before they reach production.

**Decision autonomy:** Chooses implementation approach within an established design. Does not make architectural choices. Escalates blockers to senior teammates within 24 hours instead of staying stuck.

**Execution expectations:** Completes well-specified tasks in days to a few weeks. Work is accurate and follows team conventions. Produces readable code and adequate test coverage without prompting.

**Collaboration expectations:** Participates in team rituals (standup, retro, sprint planning). Reviews code from teammates with useful feedback. Communicates status proactively when plans change.

**What this is not:** L3 is not a holding stage for engineers waiting to become "real" engineers. It is the level at which someone is learning the system and the team's way of working. Forward movement to L4 is expected within 18–24 months of hire.

---

### L4 — Senior Software Engineer I

**Impact scope:** Owns a feature domain or a subsystem. Work has visible product impact within the team's quarter. Can take a well-defined product or technical requirement from design through delivery without sustained manager guidance.

**Decision autonomy:** Makes implementation and local design decisions autonomously. Escalates decisions that carry cross-team risk or that affect the system's overall architecture. Writes design docs for non-trivial work.

**Execution expectations:** Delivers multi-week initiatives on schedule. Identifies scope risk early and surfaces trade-offs; does not let delay silently accumulate. Test coverage, error handling, and observability are part of the definition of done — not afterthoughts.

**Collaboration expectations:** Runs code review for L3 engineers on the team. Mentors new hires through onboarding. Capable of representing the team's technical work to stakeholders outside engineering without losing accuracy.

---

### L5 — Senior Software Engineer II

**Impact scope:** Leads delivery of significant features or technical investments with impact visible to the product and business at the team or product area level. Drives technical strategy within their domain and contributes meaningfully to adjacent domains.

**Decision autonomy:** Makes consequential technical decisions — service boundaries, data model choices, major dependency selections — with appropriate stakeholder review. Does not require a manager to decompose problems before beginning work. Escalates decisions with org-wide implications.

**Execution expectations:** Owns outcomes beyond outputs. Identifies when the defined solution is wrong and redirects without requiring permission. Manages external dependencies (vendor APIs, platform team deliverables, infra capacity) as part of delivery.

**Collaboration expectations:** Informal technical lead for one or more features in flight. Known across the engineering org as a reliable source of expertise for their domain. Writes documentation that enables teammates to work independently. Makes peers more effective.

---

### Staff Engineer (L6)

**Impact scope:** Technical owner of an initiative that spans multiple teams, affects a product line, or addresses a platform-level risk. Work creates leverage across the org — reduces toil, unblocks future roadmap, or closes a risk that would otherwise require executive attention.

**Decision autonomy:** Operates with a high degree of autonomy on technical architecture, tooling choices, and cross-team technical standards. Accountable for the outcomes of those decisions, including when they are wrong. Escalates decisions that have significant cost, compliance, or strategic implications — and arrives at escalation with a recommendation alongside the question.

**Execution expectations:** Drives multi-month technical programs with ambiguous initial scope. Creates the structure that allows others to execute. Writes the technical design that becomes the team's shared understanding. Holds a high quality bar without being a bottleneck.

**Collaboration expectations:** Partners with engineering managers as a peer — not to manage people, but to inform team structure and priorities. Builds relationships across engineering, product, and adjacent functions (security, data, platform). Represents engineering credibly to senior business stakeholders. Grows the technical capability of the org through documentation, review, and direct mentorship of L4–L5 engineers.

**What this is not:** Staff is not a tenure award, a consolation for engineers who do not want to manage, or a title that makes an engineer feel better about not being a Principal. The work must be happening before the title is granted.

---

## Part 4: Calibration Session Structure

### Purpose

Calibration is not performance review. It is the mechanism that ensures level decisions are consistent across managers — that "Senior" means the same thing to a manager in the payments team as it does to a manager in the platform team.

Calibration should happen at minimum once per year, prior to the compensation cycle, and before any batch promotions are finalized.

### Pre-Read Format

Each manager prepares a one-page write-up for each engineer under review. The format:

```
Name: [Engineer]
Current level: [L4]
Manager recommendation: [Promote to L5 / Sustaining at L4 / Needs support]
Time in level: [14 months]

Evidence of impact scope (2–3 bullet points of specific work, not traits):
- Led delivery of the user authentication rewrite, reducing login latency by 40% across 1.2M active accounts
- Identified and resolved a data race in the order processing service that was producing 0.3% silent data corruption; root cause required coordinating a fix across two teams

Evidence of decision autonomy:
- Proposed and delivered migration from session cookies to JWT without senior oversight; trade-offs documented in design doc X
- Escalated one architectural risk (cache invalidation strategy) in Q3, arrived with three options and a recommendation

Evidence of collaboration:
- Runs code review for two L3 engineers; feedback is specific and actionable based on review history
- Wrote onboarding guide for the payments service that reduced new hire ramp time from 3 weeks to 10 days

Context (anything that affects interpretation):
- Was primary on-call for 6 weeks during team staffing gap — may underrepresent "normal" scope
- Joined the team 8 months ago via internal transfer; prior work at L5 scope should count
```

This format forces managers to work from evidence before the session, not to improvise in the room.

### Facilitator Role

Calibration sessions need a neutral facilitator — typically the Director or VP of Engineering, or a senior HR partner who knows the org. The facilitator does not advocate for any individual. Their job is to:

- Ensure every engineer gets equivalent discussion time
- Surface proxy arguments ("she works so hard") and redirect to evidence
- Name calibration failure modes when they are happening (see Part 5)
- Drive toward a decision when discussion has exhausted new evidence

The facilitator is not the final decision-maker on every case. For clear cases (strong evidence at or above the target level, or clearly not there yet), the manager's recommendation stands. The facilitator earns their influence in contested cases.

### Promotion vs. Not-Yet Criteria

**Promote:**
- Engineer is operating at the target level consistently and for a sustained period (typically 6+ months of clear evidence)
- The promotion recognizes work that has already happened, not potential for work to happen

**Not yet:**
- Engineer is operating at the target level in some but not all dimensions
- Evidence is strong for this cycle but was concentrated in the last 60 days (recency — see failure modes)
- Manager can name specific gaps and a plausible path to close them in the next cycle

**Not yet is not no.** The calibration session output for a not-yet case should include: what specifically needs to happen, over what time horizon, and what support the manager will provide. If a manager cannot articulate this, the case needs more discussion.

### Forced Ranking — Do Not Use It

Forced ranking (e.g., stack-ranking all L4 engineers against each other and requiring a fixed distribution of outcomes) is incompatible with a level-based promotion system. Level definitions describe an absolute standard, not a relative one. Two engineers can both meet the bar for L5 and should both be promoted, even if they are on the same team. Forced ranking will correctly identify your top performers. It will also incorrectly suppress promotion of everyone else and create a zero-sum dynamic that destroys peer collaboration.

The legitimate version of forced ranking is using it as a calibration check, not as a decision mechanism: "If we had to rank these engineers, would the order surprise us? If so, why?" That is a useful question. Requiring a bottom 10% outcome is not.

---

## Part 5: Calibration Failure Modes

### Grade Inflation

**What it looks like:** Every manager promotes every report. L5 is the new L4. "Senior" describes 70% of the engineering org.

**Why it happens:** Managers use promotion as a retention tool when they cannot offer salary increases, or when they lack the confidence to deliver difficult feedback. The ladder loses predictive validity — a title no longer reliably describes the scope of work the person owns.

**How to catch it:** Track promotion rates by manager over time. A manager who promotes every eligible engineer in every cycle is not a great developer of talent — they are either managing an exceptional team or avoiding difficult conversations. Both are worth understanding.

**How to address it:** Require managers to articulate the gap between the current level definition and the promoted level definition for each promotion. If the write-up does not distinguish between the two, the case is not ready.

### Recency Bias

**What it looks like:** An engineer who delivered one highly visible project in Q4 gets promoted, while an engineer who delivered consistently across the year does not.

**Why it happens:** Recency is cognitively easier than sustained evaluation. Visible projects get promoted; steady contributors are overlooked.

**How to catch it:** During calibration, ask managers to cite evidence from the full review period — not from the last quarter alone. If all the evidence in a write-up is from the past 90 days, flag it.

**How to address it:** The pre-read format requires evidence across the review period. The facilitator's job is to name recency bias when it is happening: "You've cited three things from Q4. What do you have from Q1–Q3?"

### Proximity Bias

**What it looks like:** Engineers who are co-located with their manager, who are in the same timezone, or who are well-known in the organization get promoted faster than engineers who are remote, on different schedules, or who contribute in less visible ways.

**Why it happens:** Calibration evidence is gathered by the manager, and managers observe their most proximate reports most easily. Platform engineers who work across teams in low-visibility ways accumulate less narratable evidence than product engineers who ship user-facing features.

**How to catch it:** At calibration, ask: "Is there anyone on your team whose work I might not know about? What did they do?" This surfaces contributors who are not on the org's radar.

**How to address it:** Encourage engineers to write their own contribution summaries as input to the pre-read. Some teams run "brag docs" — a running log of completed work that engineers maintain themselves. This is not about self-promotion; it is about ensuring the manager has evidence that is otherwise invisible.

---

## Part 6: Contested Promotions and Appeals

### When a Promotion Is Contested

A promotion is contested when:
- A manager advocates strongly for promotion and the calibration group does not agree
- An engineer expected promotion and did not receive it, and is not satisfied with the explanation
- There is credible evidence that the decision was influenced by bias

### Appeals Process

Every engineer has the right to understand why a promotion did not happen and to request a second review. The appeals process should be:

1. **First step:** Engineer meets with their manager to review the calibration outcome and the specific evidence gaps cited. This meeting should happen within two weeks of the cycle close.

2. **Second step:** If the engineer believes the outcome was based on incomplete evidence or an inconsistent standard, they may request a skip-level conversation with the manager's manager. The skip-level is not a reversal mechanism — it is a quality check. The skip-level can confirm the outcome, reopen the case, or escalate to HR review.

3. **Third step (rarely needed):** If the engineer believes the outcome reflects discriminatory bias, it is escalated to HR and handled through a separate process.

The appeals process should be documented and communicated to every engineer before calibration runs. An undocumented appeals process is not a process — it is a rumor.

### Skip-Level Input

For promotions to Staff and above, skip-level input should be solicited before calibration, not as a last resort. The people best positioned to assess whether an engineer is operating at Staff scope are the Directors and VPs who observe their work across teams. Build skip-level observation into the promotion process: for any promotion above L5, the write-up should include one perspective from someone outside the immediate management chain.

---

## Part 7: Compensation Banding Philosophy

### How Bands Work

Each level maps to a salary band with a minimum, midpoint, and maximum. The band is wide enough to accommodate variance in market and tenure (typically 20–30% spread from min to max). The midpoint represents market rate for someone operating solidly at the level. Engineers in the lower half of the band are either new to level or in markets where labor costs are lower. Engineers at or above the midpoint have been at the level for some time and are performing well.

### What to Communicate Without Causing Attrition

The standard advice — "share the band" — is incomplete. Sharing a band without context tends to produce exactly what it is designed to prevent: engineers who learn they are at 75% of the band max immediately ask why they are not at 100%, and engineers who are near the top of the band conclude there is no room to grow.

The framing that works:

- Share the band for the current level and the band for the next level. The gap between them is the financial incentive for promotion — make it visible.
- Explain that being in the lower half of the band is not a signal of low performance. It reflects market and tenure factors, not a verdict on the engineer's capability.
- If an engineer is significantly below midpoint for their level and has been at level for 18+ months, that is a compensation problem to address in the next comp cycle — proactively, not in response to a resignation.

### When Compensation Correction Is Needed

Compensation corrections (adjustments not tied to a promotion) are sometimes necessary when:
- An engineer was hired into the band low and has not been adjusted upward as they grew
- Market rates have moved significantly and the band has not kept up
- Compression has occurred — junior engineers hired recently are earning close to or more than senior engineers with 3+ years in level

Corrections should be handled before they become retention problems. The signal that a correction is needed is usually a manager instinct ("I'm worried about losing this person") combined with a comp gap that cannot be explained by performance or tenure. Act on that signal early. Waiting for a competing offer is a worse outcome for the engineer and the org.

---

## Part 8: 6-Month Check-In Template for New Levels

When an engineer is promoted, the recurring failure is that no one revisits the expectations for the new level until the next performance cycle. The engineer got the title and the raise; the conversation about what is expected at the new level does not happen systematically.

Run this check-in at the 6-month mark after every promotion:

```
6-Month Level Check-In
Engineer: ___________
New Level: ___________
Date: ___________
Manager: ___________

1. What work have you owned in the past 6 months that reflects your new level?
   (Engineer answer — should reference the impact scope dimension of the level definition)

2. Where do you feel the clearest sense of operating at [new level]?

3. Where does the new level expectation feel least settled or most unclear?

4. In the last 6 months, is there a decision you made that you would characterize as clearly within your new level? Describe it.

5. Is there a decision you escalated or deferred that you now think you should have owned? Describe it.

6. What support would make the biggest difference to you in the next 6 months?

Manager notes:
- Is the engineer demonstrating the core behaviors for the new level consistently?
- Any gaps from the level definition that are showing up in daily work?
- What is the one thing that, if different, would make this engineer's growth in the new level clearly on track?
```

The check-in is not a mini-performance review. It is a calibration conversation between the engineer and their manager on whether the transition to the new level is working. Most promotions succeed. Some stall. This check-in surfaces the stalls before they become performance issues.

---

## Implementation Checklist

- [ ] Draft level definitions using the four-axis format (impact scope, decision autonomy, execution expectations, collaboration expectations) — not skill checklists
- [ ] Get at least two other senior leaders to review each level definition against actual engineers on the team before publishing
- [ ] Write one worked example per level drawn from real work the team has shipped in the past 12 months
- [ ] Decide where the IC/manager fork happens and document both tracks explicitly
- [ ] Define Staff-level work in terms of actual recent initiatives — not abstract capabilities
- [ ] Build the calibration session schedule: at minimum once per year, 30 days before the compensation cycle
- [ ] Publish the pre-read format to all managers 4 weeks before calibration
- [ ] Train the facilitator (Director or VP) on the three failure modes and when to name them
- [ ] Document the appeals process and communicate it to the full engineering org before calibration runs
- [ ] Build the compensation band communication template — both the current level band and the next level band
- [ ] Schedule 6-month check-ins for all engineers promoted in the current cycle
- [ ] Set a calendar reminder to review level definitions every 18–24 months for drift

---

## Common Failure Modes

**Publishing a ladder and not using it.** The ladder only has value if calibration decisions reference it explicitly. If managers are not citing level definitions during calibration, the ladder is a document; it is not yet a system.

**Level definitions that describe potential.** "Has the potential to operate at Staff scope" is not an evidence-based statement. Calibration must be grounded in what has already happened. Promote on evidence; prediction does not survive contact with the next review cycle.

**Skipping calibration for years.** Level drift accumulates silently. Two years without calibration produces a distribution where some teams have been promoting aggressively and others have not, and the discrepancy is invisible until someone's lateral transfer exposes it.

**Using the ladder only for promotions.** The level definitions should be the anchor for growth conversations in every 1:1 at every level. If an L4 engineer cannot tell you what L5 looks like in practice, the ladder is not doing its job.

## Further Reading

| Topic | Resource |
|---|---|
| Career ladders and engineering management | Fournier, *The Manager's Path* (O'Reilly, 2017) |
| Engineering org design and leveling | Larson, *An Elegant Puzzle* (Stripe Press, 2019) |
