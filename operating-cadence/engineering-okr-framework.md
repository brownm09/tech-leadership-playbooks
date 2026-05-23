# Engineering OKR Framework

## Leadership Context

OKRs sit among the widely-adopted and widely-broken planning tools in engineering organizations. When they work, they create a direct line from an IC's quarterly work to a company-level outcome, make accountability legible at every level of the org, and give engineering leaders a credible answer when executives ask "what is engineering doing and why does it matter?" When they fail (the usual outcome), they devolve into performance theater consuming three weeks of Q1 planning and forgotten by April. This playbook covers the mechanics and judgment calls determining which outcome you get.

## Background and Motivation

This framework draws on the OKR setting and cascading work at ActBlue Technical Services (2024–2025). I developed the multi-year platform directorate technical vision and translated it into objectives aligned across 6 teams. The two failure modes documented here (activity OKRs and unmeasurable aspirations) name patterns I encountered and corrected in practice.

## When to Use This

- Annual or quarterly planning cycle is underway and OKRs need to be set, refined, or re-anchored to company priorities
- A new engineering leader is inheriting OKRs that no one owns, no one checks, and no one grades
- Post-Q1 check-in reveals that OKRs were set but not reviewed — the "set and forget" failure mode is active
- An executive or board member asks engineering leadership to explain how engineering work connects to company strategy

---

## The Two OKR Failure Modes

Before writing a single OKR, name the two failure modes and check for them explicitly.

### Failure Mode 1: Activity OKRs (We Will Ship X)

Activity OKRs describe what the team will do; what will change as a result stays absent. They show up as the prevailing failure mode because they write easily and grade easily: if the thing shipped, it scores a 1.0.

**Example (broken):**
> Objective: Improve our infrastructure
> KR1: Migrate 12 services to Kubernetes by March 31
> KR2: Complete the database upgrade project
> KR3: Document all runbooks

These describe project milestones. They belong in a project plan. They do not function as OKRs.

**Why they pose danger:** A team can score 1.0 on every activity OKR and still register no progress on the business problem motivating the work. The Kubernetes migration completes; page load time does not improve. The database upgrade ships; checkout error rate stays unchanged. Activity OKRs measure effort; impact remains unmeasured.

**How to fix them:** For every "we will ship X," ask: "What will be true after X ships that is not true now, and how will we measure it?" The answer to that question is the Key Result.

---

### Failure Mode 2: Aspiration Without Measurement

The opposite failure: the KR sounds impactful but has no measurement mechanism.

**Example (broken):**
> KR: Significantly improve platform reliability

"Significantly" carries no number. Without a baseline and a target, this KR cannot be graded, cannot hold anyone accountable, and will be ignored.

**How to fix it:** Define the baseline metric, the target value, and the measurement method. "Reduce P0 incident frequency from 4/month to 1/month, measured by the incident log, by March 31."

---

## OKR Hierarchy: What Lives at Each Level

### Company Level

The CEO and exec team set company OKRs. They describe the load-bearing outcomes for the business in the next quarter or year. Engineering may inform them; ownership stays at the exec level.

Examples of well-formed company-level objectives:
- Grow net revenue retention from 105% to 115%
- Expand into two new enterprise verticals with at least three design partners each
- Achieve SOC 2 Type II certification

Engineering's job with company OKRs: understand them well enough to connect team-level work to them, and push back when a company-level commitment requires engineering work absent from the plan.

---

### Engineering Org Level

The VP or Director of Engineering owns engineering org OKRs. They describe what the engineering organization will accomplish in the quarter (shipping alone does not count) and they connect directly to company-level outcomes.

A well-formed engineering org OKR:
- Connects to a company OKR (explicitly — name it)
- Is measurable at the end of the quarter
- Sits at a level of abstraction making sense for the org; a single-team level falls short

**Example:**

> Company OKR: Achieve SOC 2 Type II certification
>
> Engineering Org Objective: Build the infrastructure and evidence trail required for SOC 2 Type II certification
>
> KR1: 100% of production systems have audit logging enabled, confirmed by a third-party audit firm pre-scan by March 15
> KR2: Mean time to revoke access for a departed employee reduced from 72 hours to 4 hours, measured in the IAM system
> KR3: Zero critical findings in the third-party penetration test scheduled for February

Each KR carries a number, a measurement method, and a deadline. None describe activities; all describe the state of the world at the end of the period.

---

### Team Level

The engineering manager owns team OKRs, with input from the team. They connect to org-level OKRs and to the specific work the team holds the position to do.

**The connection test:** For every team KR, the manager should be able to name which org-level KR it supports, in one sentence, without hedging. Failure here usually means the team KR functions as a project milestone in disguise.

Team-level KRs should run specific enough that a new team member reading them would know exactly what the team considers success.

---

## How to Write a Good Key Result

A Key Result failing to answer all five questions below stays unready to commit to:

1. **What changes?** Avoid "we complete X"; use "X moves from A to B."
2. **Who measures it?** A named person carries responsibility for tracking this number.
3. **Where does the data come from?** The measurement system exists today or will exist before the measurement period begins.
4. **By when?** Quarter end serves as the default; long-lead-time KRs need interim checkpoints.
5. **Who owns it?** One person's name. Avoid "the team." Avoid "engineering."

**SMART test:**
- **Specific:** The KR describes a single outcome (a cluster of activities does not qualify)
- **Measurable:** A number exists
- **Achievable:** A score of 1.0 stays possible without being guaranteed; guaranteed 1.0 signals an unambitious target
- **Relevant:** The KR connects to an objective connecting to a company priority
- **Time-bound:** The deadline reads explicit

---

## Cascade Mechanics

Cascading OKRs does no line-by-line translation downward. It produces alignment: ensuring what each team works on connects to what the org tries to accomplish, without requiring every team KR to copy an org KR directly.

**How to cascade without micromanaging:**

Step 1: Engineering Director shares org-level OKRs with all managers, with the context for why each one was chosen and which company OKR it connects to.

Step 2: Each manager drafts team OKRs independently and writes one sentence next to each KR explaining which org OKR it supports.

Step 3: Director reviews for coverage (are all org OKRs supported by at least one team?) and for conflicts (are any two teams pulling in opposite directions on the same system?).

Step 4: Director does not rewrite team KRs. When a KR reads unclear or disconnected, the conversation runs: "Help me understand how this KR supports [org OKR]. Can you explain the connection?" That conversation often reveals either a gap in the team's understanding of priorities, or a gap in the director's understanding of what the team does day-to-day.

**Coverage gap vs. conflict:**
- A coverage gap means an org OKR has no team-level work supporting it. Either the org OKR needs a team assigned to it, or it needs to be removed.
- A conflict means two teams hold incompatible assumptions about how a shared system will work. Surface this during cascade; waiting until mid-quarter compounds the cost.

---

## Grading

### The 0.7 Target

Google popularized the idea that a 1.0 score on an OKR means the target ran too easy. The engineering interpretation: when every KR scores 1.0 every quarter, the organization sets unambitious targets. The right calibration treats 0.7 across the board as strong performance.

This only works when grading runs honest. A team inflating its scores to look good destroys the information value of OKRs. The engineering director's job during scoring: press on scores looking inflated; refuse to accept them in the name of positivity.

### What to Do With a 1.0

Two questions to ask:
1. Was this a legitimate stretch that we executed exceptionally well? If yes, celebrate it and use it to recalibrate ambition upward next quarter.
2. Did the target get set too conservatively? If yes, that signals planning feedback; calling it a performance win misreads the score. Recalibrate the target for next quarter.

If a team consistently scores 1.0 on every KR every quarter, the targets run too conservative and the planning process needs fixing.

### What to Do With a 0.3

A 0.3 means something significant did not happen. Before attributing this to execution failure, ask:

1. Did the KR remain valid at the end of the quarter? Company priorities shift; a KR right in January may have been explicitly deprioritized by March. If so, grade it N/A or "deprioritized"; a 0.3 wrongly penalizes a team for acting on changed priorities.
2. Did an unescalated blocker exist? A 0.3 visible in week 4 and unsurfaced until scoring counts as a leadership failure, with execution failure stacked on top.
3. Did the KR run unrealistic from the start? If so, that signals a planning failure. Fix the planning process.

A 0.3 representing genuine execution failure (the team committed, held the capacity, faced no blocker, and still did not deliver) warrants a direct conversation about what happened and what changes next quarter.

---

## Quarterly OKR Review Format

**Cadence:** Monthly check-in (30–45 min), with a formal score at quarter end
**Audience:** Engineering director with all managers; managers run an equivalent with their teams
**Owner:** Engineering director drives the check-in; each manager is accountable for their KRs

**Monthly check-in template:**

| KR | Owner | Target | Current | Score (0–1) | Status | Next 30-day action |
|---|---|---|---|---|---|---|
| P0 incident frequency: 4/mo → 1/mo | Platform lead | 1/mo | 2.5/mo | 0.4 | At risk | Root cause the two November incidents; fix alert sensitivity by Dec 15 |
| Checkout error rate: 2.3% → 0.8% | Payments team TL | 0.8% | 1.1% | 0.7 | On track | Ship the payment retry logic fix in sprint 3 |
| Access revocation time: 72h → 4h | Security eng | 4h | 4h | 1.0 | Complete | Maintain; document the process for audit evidence |

**Status definitions:**
- **On track:** Current score and trajectory suggest 0.7+ by quarter end
- **At risk:** Current score and trajectory suggest 0.4–0.7 by quarter end without intervention
- **Off track:** Current score suggests <0.4 by quarter end even with intervention
- **Complete:** KR is at 1.0 and the work is done
- **Deprioritized:** KR was explicitly deprioritized during the quarter due to changed company direction

The monthly check-in does no autopsy on why scores landed where they landed. It centers on the "Next 30-day action" column: what will change before the next check-in?

---

## Retrospective: What to Learn From OKRs

At the end of each quarter, before setting next quarter's OKRs, run a 30-minute retrospective focused on the OKRs themselves:

**Questions to answer:**
1. Which KRs scored below 0.5? Was this an execution problem, a planning problem, or a changed-priorities problem? Each has a different fix.
2. Which KRs scored 1.0? Were they too easy, or were they genuine stretch achievements?
3. Were there things the team did in the quarter that were not in any KR? If yes, why? Was it unplanned work that should have been anticipated? Was it reactive work that should have been prevented? Was it valuable work that should be in next quarter's OKRs?
4. Were the measurement systems in place? If a KR was hard to grade because the data did not exist or was not trustworthy, that is a setup failure.

**What to celebrate:**
- KRs scoring 0.7–0.9 on a genuinely ambitious target; the system works as designed
- KRs where the team surfaced a blocker in week 4 and escalated it, even on a low final score; the system aims to incentivize exactly this behavior
- KRs deprioritized cleanly and transparently when company priorities changed; the system reads this as organizational adaptability, never as failure

**What not to celebrate:**
- 1.0s on targets that were conservative
- Completing activities that were listed as KRs but did not move any outcome metric

---

## Common Failure Modes

### Too Many OKRs

The optimal number of OKRs for a team runs 1–2 objectives with 2–4 key results each. A team with 5 objectives and 15 key results has no priorities; it has a wish list.

A useful forcing function: every new KR proposed must displace an existing one, or the manager must explain in one sentence why the team holds capacity beyond the cap.

### OKRs Set by Committee

OKRs set by a committee of stakeholders with no clear owner produce KRs acceptable to everyone and owned by no one. The manager owning the objective needs to write the first draft. Stakeholders refine. The director approves or asks questions. One name at the end.

### KRs That Measure Output, Not Outcome

"Deploy the new checkout flow" describes an output. "Increase mobile checkout conversion from 62% to 70%" describes an outcome. Deployment alone reads as meaningless when nothing changes in the world.

This distinction matters especially for infrastructure and platform work, where teams face the temptation to use deployment milestones as KRs. The question persists: what will run better for someone (a user, an engineer, an operator) after this work ships?

### OKRs That Cannot Fail

If every KR runs as "complete X initiative" and the team controls whether X completes, the KR cannot fail. It can slip; slipping reports as "in progress" instead of "failed"; the accountability mechanism never engages.

OKRs need structure such that the world can tell you whether you succeeded; an internal retrospective alone falls short.

### Grading Season Theater

Teams spending hours debating whether a KR deserves a 0.6 or a 0.7 waste the mechanism. Scoring should take 15 minutes per KR. The value lives in the honest conversation producing the number; the number itself carries less weight. If grading takes more than two hours for a full team's OKRs, the KRs probably run too vague to measure.

---

## Templates

### OKR Draft Template (Team Level)

```
**Objective:** [One sentence describing the outcome; the activity belongs elsewhere. Should be inspiring 
               but grounded in something real.]

**Connects to:** [Org-level objective name]

**KR1:** [Metric] moves from [baseline] to [target] by [date]
  Owner: [Name]
  Measurement: [How and where this is tracked]
  Confidence: [High / Medium / Low; one sentence on the primary uncertainty]

**KR2:** [Metric] moves from [baseline] to [target] by [date]
  Owner: [Name]
  Measurement: [How and where this is tracked]
  Confidence: [High / Medium / Low; one sentence on the primary uncertainty]
```

### Monthly Check-In Prompt (Manager to Director)

A one-paragraph written update submitted 24 hours before the monthly OKR check-in:

```
This month: [What moved, by how much]
At risk: [Which KR is most at risk and why]
What I need: [Any unblock, decision, or resource needed from leadership]
My call: [Whether I expect to be on track, at risk, or off track at quarter end, in one sentence]
```

---

## Implementation Sequence

**Week 1 of a new quarter (OKR setting):**
- Director shares company and org OKRs with managers, with written context
- Managers draft team OKRs independently, due end of week
- Director reviews for coverage, conflicts, and measurement quality

**Week 2:**
- Director holds 30-minute 1:1s with each manager to review their draft OKRs
- Managers refine and finalize team OKRs
- All OKRs published to a shared, accessible location (Notion, Confluence, or equivalent)

**Month 2 check-in:**
- Managers submit one-paragraph written update
- 30-minute group check-in with director and all managers
- At-risk KRs get an explicit recovery action assigned

**Quarter end (grading):**
- Managers submit scores with one-sentence explanation per KR
- 30-minute retrospective on the OKR process itself
- Planning for next quarter begins the following week

---

## A Note on Trust

The OKR system only works when people believe honest low scores will be treated as information, never as performance failures. An engineering director responding to a 0.3 with criticism instead of curiosity will get inflated scores next quarter. The question when a KR scores low: what does this tell us about what we planned vs. what was possible? That asks a planning question; performance only enters when the pattern of low scores persists across multiple quarters with the same owner and the same explanations.

Building that distinction into how you respond to scores matters more than any template in this document.

## Further Reading

| Topic | Resource |
|---|---|
| OKR methodology | Doerr, *Measure What Matters* (Portfolio/Penguin, 2018) |
| OKRs in practice | Google re:Work — [Set Goals with OKRs](https://rework.withgoogle.com/guides/set-goals-with-okrs/) |
