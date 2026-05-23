# Headcount and Capacity Planning Playbook

## Leadership Context

Headcount planning is where engineering leadership earns or loses credibility with finance and the executive team. A Director or Head of Engineering who arrives at annual planning with a number and a story — without a model — is asking for trust they have not yet established. A leader who arrives with a model showing how each FTE maps to a specific initiative, a projected output, and a risk if the hire does not happen is having a fundamentally different conversation.

The business outcome: accurate headcount modeling increases delivery confidence (teams are not perpetually understaffed), reduces time-to-hire pressure (you know when you need the person before it is urgent), and builds the kind of exec trust that translates into more headcount authority over time. Engineering leaders who consistently hit their delivery commitments get more headcount the next cycle. Leaders who miss because they failed to account for attrition, ramp time, or toil do not.

## Background and Motivation

This playbook was developed from the headcount modeling work for the platform directorate at ActBlue Technical Services (2024–2025). I developed the multi-year platform directorate technical vision and translated it into headcount requirements across 6 teams, presenting to the VP of Engineering and Head of Product across multiple planning cycles.

## When to Use This

**Annual planning.** The org is setting headcount targets and budget for the next fiscal year. Engineering needs to translate a product roadmap into an FTE model that finance can evaluate.

**Reorg.** Teams are being restructured. The question is whether the new org design has the right people in the right places — or whether restructuring has created a coverage gap that needs a backfill or new headcount to close.

**New product line.** Leadership has decided to build something net-new. The question is: what does the team need to look like, and when do you need to hire?

**Missed delivery attributed to staffing.** A quarter ended with a major initiative delayed or descoped. Post-mortem attribution points to insufficient staffing, scope creep, or unexpected attrition. Leadership needs a model for what the team should have looked like to hit the target.

---

## Part 1: The Capacity Model

### Four Inputs to Any Headcount Estimate

Before building a model, acknowledge that engineering time is not uniformly available for new work. Four inputs consume available capacity before any line of new product code is written:

**1. Net new work.** The roadmap initiatives — the things the business is asking engineering to build. Headcount models that start and stop here produce wrong answers.

**2. Maintenance and toil.** Production systems require ongoing attention: dependency upgrades, security patches, performance fixes, monitoring improvements, infrastructure drift, and the long tail of technical debt that never quite makes the sprint but never quite goes away. In a mature engineering org, 20–30% of engineering time is absorbed here. For orgs with significant legacy debt, the number is higher. It does not go on the roadmap; it is always there.

**3. Org overhead.** Engineering time spent on activities that are necessary but do not directly produce code: sprint ceremonies, 1:1s, hiring (phone screens, loops, debrief time), on-call rotations, cross-functional meetings, documentation, and onboarding new hires. Rule of thumb: 15–25% of time depending on team seniority and org scale. Senior engineers in hiring-intensive environments can lose 30%+ of their time to recruiting in a hot quarter.

**4. Attrition replacement.** Engineers leave. The average annual attrition rate in software engineering organizations varies by market and org culture, but 10–15% annually is a reasonable planning assumption in a healthy org; 20%+ is elevated and suggests structural problems. Attrition creates a gap that is not filled until the replacement hire is fully ramped — which takes 60–90 days to start, and then 3–6 months of ramp time for a mid-level engineer to reach full productivity. An org of 40 engineers at 12% annual attrition will lose roughly 5 engineers per year; those 5 slots are generating partial-to-zero capacity during replacement and ramp.

### Approach 1: Initiative-Level Modeling

Best for: annual planning, new product lines, cases where the roadmap is defined at the initiative level.

For each initiative on the roadmap, estimate:
- Team size needed (FTEs, by role where relevant)
- Start date and ramp date (when does the team need to be at full capacity?)
- Duration (how long is this initiative in flight?)
- Required output (launch, capability, risk reduction, cost savings)

Then stack the initiatives against the calendar and identify where demand exceeds available supply.

**Example (simplified):**

| Initiative | Required FTEs | Start | Full Ramp | Duration |
|---|---|---|---|---|
| Payments v2 | 4 | Jan | Mar | Q1–Q3 |
| Data platform rebuild | 3 | Mar | May | Q2–Q4 |
| Mobile rewrite | 5 | Jun | Aug | Q3–Q1+1 |
| Platform reliability | 2 | Jan | Jan | ongoing |

At peak overlap (Q2–Q3), this roadmap requires 14 FTE at full capacity. That does not include maintenance, overhead, or attrition replacement. Add those inputs and the real capacity need is closer to 18–20 FTE.

**Trade-off:** Initiative-level modeling is intuitive and easy to present to non-engineering executives. Its weakness is sensitivity to scope changes — when initiative scope shifts mid-year (as it always does), the model breaks and requires manual reconstruction.

### Approach 2: Story Point / Throughput Modeling

Best for: teams with mature sprint data and stable velocity baselines; useful for quarterly planning horizons.

Calculate historical team velocity (story points per sprint, averaged over 6–12 sprints). Estimate the story point volume of the roadmap. Divide to get the number of sprints required. Then factor in capacity loss (maintenance, overhead, attrition) and derive the FTE requirement that keeps the timeline intact.

**Example:**

- Current velocity: 120 story points per 2-week sprint for a team of 8 engineers
- Roadmap estimate: 960 story points for the next 6 months
- Raw capacity: 12 sprints = 960 points (fits, if capacity is 100% available)
- Adjusted for maintenance (25%): effective capacity = 90 points/sprint → 960/90 = 10.7 sprints ≈ 22 weeks → slips by 6 weeks
- To hit the original timeline: need ~10 FTE, not 8, to absorb maintenance load and maintain roadmap velocity

**Trade-off:** Story point modeling is precise when velocity data is reliable and when the work is well-decomposed. It breaks down for highly uncertain work (new domains, platform-layer work, anything requiring significant discovery) and requires discipline in estimation that many teams do not have.

**Practical recommendation:** Use initiative-level modeling for the annual plan (where initiative-level uncertainty is higher) and throughput modeling for quarterly planning (where sprint-level data is more reliable).

---

## Part 2: Building the Headcount Ask to Finance

### The One-Page Model

The finance team does not need a spreadsheet with 15 tabs. They need to answer one question: is the return on this headcount investment greater than the cost? Structure your ask around that question.

**Format:**

```
Initiative: [Name]
Business output: [What this team will produce — measured outcome, not activity]
Required team: [N engineers + N roles, ramped by Month X]
Fully-loaded annual cost: [$XXX,000 per engineer × N = $X.XM]
Risk if not funded: [Revenue impact, delivery slip, competitor risk — in business terms, not engineering terms]
ROI framing: [This team enables $X in revenue / avoids $X in risk / reduces $X in operational cost]
```

**Worked example:**

```
Initiative: Payments v2 — modernized payment processing engine
Business output: Reduce transaction failure rate from 1.8% to <0.5%, enabling $4.2M in 
  annual revenue currently lost to payment failures; reduce payment team toil by 
  estimated 30%, freeing 1 FTE of capacity for roadmap work
Required team: 4 engineers (2 senior, 1 mid, 1 junior) ramped to full capacity by March
Fully-loaded annual cost: $1.1M (including benefits and overhead burden)
Risk if not funded: Transaction failure rate persists; 6-month delay to any subsequent 
  payments feature (architecture dependency); team attrition risk if legacy burden continues
ROI framing: $4.2M revenue recovery / $1.1M team cost = 3.8x return in year one
```

This framing is legible to a CFO, a board member, and an engineering manager. It does not require translation.

### Prioritizing When You Cannot Hire Everything

The realistic outcome of annual planning is that you will not get every headcount request approved. You will get a budget and need to prioritize within it. The framework for sequencing:

1. **Attrition backfills first.** If you are losing an engineer and not replacing them, you are shrinking the team. Unless the headcount was redundant, backfills protect existing capacity and should be filled before net-new hires.

2. **Critical path initiatives next.** Identify which roadmap initiatives are on the critical path for the business — the ones where a delay has direct revenue, compliance, or competitive consequence. Staff those first.

3. **Enabling investments third.** Platform and infrastructure work that unblocks other teams is often undersold in planning because its output is invisible to product teams until it is absent. Make the dependency explicit: "The data platform team's work in Q2 unblocks three product initiatives in Q3–Q4. Without that team at capacity, those initiatives slip."

4. **Speculative work last.** Initiatives that are exploratory, depend on a strategic bet that has not been validated, or whose timeline is flexible should be the last to get headcount. They should also be the first to be paused if mid-year budget pressure forces a reduction.

---

## Part 3: Backfill vs. New Headcount

When a headcount request is for a departing engineer's replacement, the instinct is to backfill the exact same role. This is sometimes correct and sometimes a missed opportunity to reshape the team.

### Decision Criteria for Backfill

**Backfill at the same level when:**
- The departing engineer's work is genuinely needed and no one on the team can absorb it
- The team is already understaffed and adding scope would destabilize delivery
- The departing engineer owned critical institutional knowledge that needs to stay on the team

**Backfill at a different level when:**
- The departing engineer was overleveled for the work the team needs done (common after rapid growth; orgs hire at senior levels when the work does not require seniority, then find themselves with a senior-engineer backlog problem)
- The team has grown in seniority and the work distribution has shifted — what you needed two years ago is not what you need now
- You have an internal promotion candidate who can fill the vacated scope if supported

**Use the backfill as an opportunity to reshape when:**
- The org design has changed (a reorg happened) and the work distribution across teams is now different
- A new technology or architecture decision has shifted what skills are most valuable
- The departing engineer was a high-scope individual and absorbing their work into existing team members is a legitimate growth opportunity, not a compression problem

**The budget is not automatically reused.** Finance treats backfills as automatic because the headcount was already approved. Engineering leaders should treat them as a planning choice. If the work does not need to be done, the headcount is better redeployed.

---

## Part 4: Common Headcount Planning Errors

### Ignoring Ramp Time

A new hire who starts in January is not at full productivity in February. Mid-level engineers typically take 60–90 days to contribute independently and 4–6 months to be fully productive in a complex codebase. Senior engineers ramp faster in terms of productivity but slower in terms of organizational context — the organizational influence that justifies their seniority takes time to develop.

**Consequence:** If your model assumes January headcount produces January capacity, your Q1 delivery plan is already overstated.

**Fix:** In your headcount model, represent new hires at 25% capacity in month 1, 50% in months 2–3, 75% in months 4–5, and 100% from month 6 onward. Adjust for role seniority and codebase complexity.

### Ignoring Attrition

Most headcount plans model from current team size and add net-new hires. They do not subtract projected attrition. An org with 40 engineers at 12% attrition will lose approximately 5 engineers per year — roughly one every 10 weeks. If those departures are not in the model, the team ends the year 5 people short of the plan, not 0.

**Fix:** Project attrition for the planning period (use historical rate plus any specific risk signals: engineers in the market, team morale concerns, comp gaps) and add attrition replacement hires to the plan explicitly.

### Conflating Contractor Cost with FTE Cost

Contractors are expensive on a per-hour basis but have no benefits, no equity, no severance, and no long-term commitment. In a short-duration scenario (3–6 months of surge capacity), contractors can be cost-effective. Over 12+ months, the cost differential narrows or reverses, and the organizational investment cost (onboarding, context transfer, knowledge continuity) makes FTE the better choice.

**The conflation error:** Presenting a contractor option as "cheaper" than FTE without accounting for fully-loaded FTE cost (benefits = ~20–30% of base salary, employer payroll taxes = ~7–8%, equity = plan-specific) or for contractor overhead (agency margin, which runs 30–50% of contractor hourly rate for staffing agency placements).

**Fix:** Build a comparison that uses fully-loaded cost for both options and accounts for the duration of the engagement.

### Counting Engineers as Fungible

Not all engineers can do all work. A backend engineer specialized in distributed systems cannot immediately contribute to a mobile codebase. A data engineer cannot immediately staff a platform reliability initiative. When planning headcount, the model needs to account for role specificity — "4 engineers" is not the unit, "4 engineers with the specific skills the initiative requires" is.

---

## Part 5: 12-Month Rolling Model Template

This model should be maintained by the engineering Director and updated monthly. It is the input to quarterly business reviews and annual planning discussions.

| Month | Required FTE | Current FTE | Gap | Open Recs | Expected Fill | Notes |
|---|---|---|---|---|---|---|
| Jan | 38 | 36 | -2 | 2 | Feb | Ramping 1 new hire from Dec |
| Feb | 38 | 37 | -1 | 1 | Mar | 1 filled; 1 still in process |
| Mar | 40 | 38 | -2 | 3 | Apr–May | Payments v2 team buildout begins |
| Apr | 40 | 39 | -1 | 2 | May | One expected attrition in Q2 |
| May | 42 | 41 | -1 | 2 | Jun | Mobile team expansion |
| Jun | 42 | 42 | 0 | 0 | — | Fully staffed for current plan |
| Jul | 44 | 42 | -2 | 2 | Aug | Q3 roadmap expansion |
| Aug | 44 | 43 | -1 | 1 | Sep | |
| Sep | 44 | 44 | 0 | 0 | — | |
| Oct | 44 | 43 | -1 | 1 | Nov | Annual attrition projection |
| Nov | 46 | 44 | -2 | 2 | Dec–Jan | Annual planning headcount |
| Dec | 46 | 44 | -2 | 2 | Jan | Ramp expected in Q1 |

**Required FTE:** What the current roadmap plan requires at full capacity, accounting for maintenance load and overhead.

**Current FTE:** Actual headcount, including engineers in ramp (shown at partial capacity in the capacity model but counted as 1 in the headcount model).

**Gap:** Negative means understaffed; positive means overstaffed (unusual, but worth tracking if a roadmap shift eliminates a team's work).

**Open Recs:** Active job requisitions approved and in process.

**Expected Fill:** When, based on pipeline, you expect the role to be filled. Does not include ramp time.

This model is not a guarantee. It is a shared assumption about what staffing will look like month-to-month, which gives the engineering Director and the product team a common frame for discussing roadmap feasibility.

---

## Part 6: Presenting Headcount Needs at Board Level

When headcount comes up in a board or executive team setting, the audience has changed. They do not need the model — they need the story the model tells. Three bullets, not a spreadsheet:

**Bullet 1: What we are building and why now.**
> "We are investing in a new payments infrastructure that will reduce transaction failure rates from 1.8% to under 0.5%. This is a Q1 priority because the current architecture is a dependency blocker for three product initiatives in the second half of the year."

**Bullet 2: What team we need and what it costs.**
> "We need a team of 4 engineers ramped by March. Fully loaded, that is $1.1M annually. We are requesting approval to open 2 net-new requisitions; the other 2 will be internal transfers from an existing team whose roadmap is complete."

**Bullet 3: The risk of not acting.**
> "If this team is not in place by March, the Q3 product roadmap has two initiatives that slip to Q4 at minimum. One of those initiatives — the enterprise checkout flow — has a contracted delivery commitment. A delay there creates a contract risk worth approximately $800K."

This framing answers the board's actual questions: Is this a good investment? What are we getting? What happens if we don't do it? The spreadsheet is your backup — bring it, but do not lead with it.

---

## Implementation Checklist

- [ ] Identify your capacity model approach (initiative-level vs. throughput) based on roadmap maturity
- [ ] Calculate the four capacity inputs for the current team: net new work, maintenance/toil, org overhead, attrition replacement
- [ ] Build the 12-month rolling model and get it to a shared location where product and finance can see the current state
- [ ] For each headcount request, build the one-page initiative model (output, cost, ROI, risk-of-no)
- [ ] Adjust all model projections for ramp time — no new hire is productive at 100% until month 6
- [ ] Project attrition for the planning period and add replacement hires to the plan explicitly
- [ ] Audit all open backfill requests — confirm each is the right role at the right level before re-posting
- [ ] Prepare the 3-bullet executive summary for each major headcount cluster before the planning meeting
- [ ] Update the rolling model monthly; review the gap column with your product counterpart quarterly

---

## Common Failure Modes

**Headcount as a negotiating anchor.** Asking for 20 engineers expecting to get 12 trains finance to discount every request. If your model supports 12, ask for 12. If the model supports 20 and you believe it, defend 20.

**Treating headcount like budget that rolls over.** Unfilled headcount at end of year is often swept by finance into next year's baseline at a lower level. Carry headcount to the point of an active req; if a role has been open for more than 4 months, surface the fill-rate problem, do not let the role silently expire.

**Planning for headcount without planning for onboarding.** Every new hire requires bandwidth from the existing team to ramp — code review, pairing, architecture walkthroughs. If the team is already at capacity, adding two new hires in the same month will reduce total output before it increases it.

**The "we just need one more person" trap.** When a team is struggling to deliver, the instinct is to add headcount. The more likely cause is scope that is not properly bounded, technical debt that is consuming untracked time, or unclear priorities. Headcount does not fix unclear priorities — it amplifies them. Diagnose before you hire.

## Further Reading

| Topic | Resource |
|---|---|
| Staffing philosophy | Marquet, *Turn the Ship Around!* (Portfolio/Penguin, 2013) |
| Capacity and leverage framing | Grove, *High Output Management* (Vintage, 1995) |
