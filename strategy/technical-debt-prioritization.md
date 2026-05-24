# Technical Debt Prioritization Framework

## Leadership Context

Technical debt accumulates from every speed trade-off, including the ones that were correct at the time. The problem is structural: engineering organizations rarely have a systematic way to reason about which debt matters, in what order to address it, and how to argue for the time to do so in a roadmap context dominated by feature delivery.

Leaders who treat debt as a single category make two predictable errors: they either argue for a dedicated cleanup quarter that never arrives, or they absorb it silently until it becomes a reliability crisis. Neither outcome is acceptable. This framework covers classification, visibility, and sequencing — the three things you need to talk about technical debt in a way that changes what gets prioritized.

## Background and Motivation

This framework was developed from programs at ActBlue Technical Services (2022–2025). A multi-year PCI environment deprecation initiative eliminated approximately 30% of the PCI-scoped service codebase and migrated 2,600+ accounts to Stripe; the parallel payment-processor migration to Stripe shrunk the payments codebase by 7,000+ lines while maintaining production payment continuity. A separate infrastructure migration from Heroku to Kubernetes reduced infrastructure costs by approximately $64,000 per year. All of these efforts required arguing for and winning dedicated engineering time against competing feature roadmap priorities, and required translating technical risk into the language that finance, product, and legal leadership could evaluate.

The core insight from these programs: debt that cannot be described in business terms will not be prioritized in business planning. The classification framework exists to make that translation possible.

---

## Part 1: Debt Classification

Not all debt is equal. Treating it as a single category is the reason debt backlogs become infinite lists nobody acts on. The four-class model forces a distinction between debt that will kill you, debt slowing you down, and debt that frustrates engineers when they look at it.

### Class 1: Load-Bearing Debt

Debt that, if not addressed, will cause a system failure. The "if not addressed" is the critical qualifier — this is debt that is presently stable but is on a trajectory toward instability. Examples:

- A database schema that cannot be migrated without downtime as data volume continues to grow at the current rate
- A library with a security vulnerability where no upstream fix exists and the current version is approaching end-of-support
- A monolith service boundary that cannot accommodate the load forecasted for the next 18 months without a structural change

**How to identify it:** Load-bearing debt typically lives in the minds of senior engineers, in your incident post-mortems (as "contributing factors"), and in architecture diagrams that people annotate with "we don't talk about this." Surfacing it requires asking directly: "What are you most worried about in this system that you have never been given time to fix?"

**How to argue for it:** Frame it as risk management. "If we do not address X by [timeline], we will lose the ability to [migrate / scale / patch] without a high-risk manual intervention. The cost of that intervention is [estimate]. The cost of addressing it now is [smaller estimate]." This is insurance language; the framing is deliberate.

### Class 2: Risk Debt

Debt that does not immediately threaten system stability but creates compliance, security, or operational exposure. The timeline pressure is external — a compliance audit, a regulatory deadline, a contractual SLA — instead of intrinsic to the system.

Examples:
- Dependencies not in scope of the current compliance boundary that extend the audit surface area
- Authentication flows that predate the current security requirements and have not been reviewed against them
- Logging gaps that make it impossible to reconstruct what happened in an incident with the specificity required by an SLA

**How to identify it:** Risk debt lives in compliance checklists, security audits, and post-incident reviews. It also lives in conversations where an engineer says something like "technically we could do X, but if anyone looked at it closely..." That qualifier is the signal.

**How to argue for it:** Frame it against the cost of the risk materializing. "This gap has a [probability] chance of surfacing in the next audit. If it does, the remediation will require [estimate] under time pressure, plus potential penalties. Addressing it now costs [estimate] with no time pressure." Organizations that have been through a compliance finding understand this framing immediately.

### Class 3: Velocity Debt

Debt that is not threatening stability or compliance but is slowing the team down every day. The compounding interest model applies here: the cost is not a single event but an ongoing tax on every sprint.

Examples:
- A test suite that takes 45 minutes to run, causing engineers to skip tests locally and only catch failures in CI
- A deploy pipeline that requires manual steps, adding 20 minutes to every release
- An internal API that is undocumented and whose behavior is discovered through trial and error, adding 2–3 days to onboarding for every engineer who touches it

**How to identify it:** Ask your engineers how long common tasks take versus how long they should take. The gap is velocity debt. Cycle time metrics and deployment frequency (from DORA) make this visible at the system level; sprint retrospectives surface it at the team level.

**How to argue for it:** Quantify the tax. "The current test suite takes 45 minutes. Engineers run it [N] times per week across [M] engineers. At [loaded engineer cost], that is [dollar amount] per year in waiting time — and that does not include the cost of bugs that pass because people skip the suite locally." Product managers who think debt remediation is abstract often respond immediately to this kind of arithmetic.

### Class 4: Cosmetic Debt

Code that is ugly, inconsistent, or embarrassing — but does not threaten stability, compliance, or velocity. Naming conventions that were wrong in 2019. A component that nobody touches and that works fine. A function that is too long but has adequate test coverage and stable behavior.

**How to treat it:** Do not prioritize cosmetic debt on its own. Address it opportunistically — when an engineer touches a file for another reason, they can clean it up. When a component needs to be understood for a new feature, it can be documented. But cosmetic debt does not go into the roadmap and does not get its own quarter.

The cost of misclassifying cosmetic debt as velocity or risk debt is significant: it consumes the finite goodwill you have for debt work on items that do not change the trajectory.

---

## Part 2: Making Debt Visible

### The Debt Inventory

Start with a debt audit — a structured exercise where the team produces a list of debt items, classified using the framework above. The audit aims at diagnosis; planning comes later. The goal is to know what exists, not to commit to fixing it.

Audit mechanics:
- Run as a team workshop, not an async survey. Async produces a list; a workshop produces a shared understanding of which items the team rates as critical.
- Give each item a class (1–4) and an estimated remediation cost (rough order of magnitude, not a sprint commitment).
- Flag any items where there is disagreement about the class. Those disagreements are the conversations the audit exists for.

Output: a debt register — a living document (not a ticket queue) that captures class, estimated cost, and the rationale for the classification. Review it quarterly. Items that are not in Class 1 or 2 do not get a place in roadmap conversations; they get addressed opportunistically.

### Framing for Product and Finance Audiences

The engineering debt conversation fails when it is presented in engineering terms to a non-engineering audience. The reframe:

| Engineering language | Business language |
|---|---|
| We have a lot of technical debt | We are carrying [specific] risks that will cost us more to address later than now |
| We need a refactoring sprint | We need to invest [X] to reduce delivery time by [Y%] starting in [quarter] |
| Our test coverage is insufficient | Defect escape rate is [N%]; each escaped defect costs [average] to remediate in production |
| We're running on a legacy framework | We are dependent on [library/platform] that reaches end-of-support in [date]; migration cost grows [rate] per quarter of delay |

The business language version answers the question every non-technical leader is asking: "What happens if we don't?"

### The Debt Roadmap Entry

Class 1 and Class 2 debt should appear in the roadmap — not in a vague "tech debt" bucket, but as named initiatives with scopes, owners, and timelines. When debt items are anonymous, they compete with named features and lose. When they are named, they can be evaluated on the same terms.

A roadmap entry for debt work should include:
- **What it is:** a one-sentence description in non-technical language
- **Why it matters now:** the risk or cost of delay
- **What done looks like:** specific success criteria, not "we paid down debt"
- **The resource ask:** team(s), timeframe, what will not be done in parallel

---

## Part 3: Sequencing Debt Paydown

### Against Delivery Commitments

A common failure mode in debt remediation is treating it as a separate work stream from feature delivery. In practice, the teams doing the debt work are the same teams doing the feature work, and the sequencing has to account for both.

A sustainable model: reserve 15–20% of sprint capacity for Class 1 and Class 2 debt work, every sprint. (This range is calibrated to mid-size product engineering teams; tune it against your team's actual risk register and delivery pressures — smaller teams often need proportionally more headroom, larger orgs with dedicated platform capacity may carry less.) The reservation is a capacity allocation, not a separate track. The specific items change quarter to quarter based on the debt register; the capacity reservation is constant. This approach produces steady debt reduction without requiring dedicated cleanup sprints, which are difficult to get and difficult to sustain.

The capacity reservation has to be explicit and defended. "We have 20% reserved for debt" needs to be in the team's working agreements, communicated to product, and visible in sprint planning. If it is implicit, it will be the first thing compressed when delivery pressure increases.

### The "Refactor Alongside Feature" Pattern

When a feature touches a system containing Class 3 (velocity) debt, include the remediation in the feature scope instead of treating it as separate work. "We will build the notification service, and in the same sprint we will migrate the adjacent logging code to the current standard." This applies the opportunistic cleanup model at feature-planning time, not retrospectively.

The discipline: scope the refactor before the sprint starts, not during it. Mid-sprint scope additions almost always compress the refactor when time gets tight.

### Dedicated Cleanup Sprints

Reserve for Class 1 debt only: items where the risk justifies taking engineering capacity fully offline from feature work. A dedicated cleanup sprint is a high-cost signal — it tells product and leadership that the debt is urgent enough to stop everything else. Using it for Class 3 or Class 4 debt destroys credibility for the times when the signal is genuinely warranted.

Mechanics for running a dedicated sprint:
- Frame it as a risk mitigation initiative, not a cleanup exercise
- Define success criteria before the sprint starts (what state does the system need to be in when this is complete?)
- Communicate the scope and justification to product and leadership before the sprint begins
- Run a lightweight retrospective at the end: did we hit the success criteria, and what did we learn about the actual scope vs. the estimate?

---

## Part 4: Common Failure Modes

### The Infinite Debt Backlog

A debt backlog with 200 items functions as a guilt list, not a register. No one will work through 200 items; the list will be used as evidence that debt is unmanageable and therefore not worth trying to manage. Cut ruthlessly. If an item is not Class 1 or Class 2 and has been on the list for more than six months without a plan to address it, either schedule it or delete it.

### "We'll Clean It Up Later"

The phrase "we'll clean this up after the launch" is almost never true. Post-launch, the next feature is already in sprint planning, and the debt item has no advocate. Address debt at the time it is created or schedule a specific follow-up with an owner — not "the team" — assigned. A debt item without an owner is a debt item that will not be addressed.

### The Big Bang Rewrite

The proposal to rewrite a system from scratch instead of incrementally improving it. This is almost always the wrong answer. Big bang rewrites:
- Take longer than estimated, because the original system's behavior is never fully understood until the rewrite reveals it
- Risk migrating bugs from the old system to the new one (and introducing new ones)
- Require maintaining two systems simultaneously during the transition period, doubling operational burden

The alternative: the strangler fig pattern. Replace one piece of the system at a time, running the old and new code in parallel for each piece until the old piece is fully replaced. This is slower per piece and much faster overall, because each piece can be shipped and validated independently.

Reserve full rewrites for systems where the existing architecture cannot accommodate the incremental change — not for systems that are merely unpleasant to work in.

### Debt as Leverage

Using debt as leverage in roadmap negotiations ("we can't add scope until we address the debt backlog") is tempting and almost always counterproductive. It frames engineering as the obstacle to delivery, damages the engineering/product partnership, and puts the engineering team in a defensive posture. The debt should be represented as a risk and a cost, not as a bargaining chip.
