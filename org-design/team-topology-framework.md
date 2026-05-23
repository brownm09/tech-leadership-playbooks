# Team Topology Framework

## Leadership Context

How you structure your engineering organization determines what your software architecture becomes — not the reverse. Conway's Law is not a metaphor; it is a predictive model. When two teams share ownership of a service boundary, that boundary will accumulate negotiation overhead and slow down both teams. When a platform team has no explicit charter to reduce cognitive load on stream-aligned teams, it becomes a bureaucratic gate. Org design is architecture work, and it has the same stakes: get it wrong and you spend years paying the interest.

This playbook treats org structure as a first-class engineering decision with measurable outcomes: deployment frequency, time to detect and recover from incidents, and the percentage of sprint capacity spent on coordination instead of delivery. Designed for Directors and Heads of Engineering inheriting a structure, scaling through a growth inflection, or rationalizing after an acquisition.

## Background and Motivation

This framework was developed from the org design work at ActBlue Technical Services (2022–2024). I identified ownership gaps across the platform, defined team scope for three new teams — DevEx, Payments Architecture, and Database Platform — built hiring processes with recruiting, made the hiring decisions, and onboarded the engineers and managers. The chartering work eliminated cross-team dependency bottlenecks and established domain ownership that had been ambiguous under the previous structure.

---

## When to Use This

Org redesign carries real cost — migration periods, team disruption, knowledge transfer gaps. Don't trigger a reorg without one of the following signals:

| Signal | Threshold to Act |
|--------|-----------------|
| Team size exceeds cognitive load ceiling | Team owns more services than it can hold context for; engineers cannot answer basic questions about half their systems without a Slack thread |
| Ownership confusion | The same incident involves 3+ teams with no clear DRI; postmortems consistently cite "unclear ownership" |
| Platform/product split has happened organically | A subset of engineers is doing infrastructure and tooling work but reporting to product-embedded teams; incentives are misaligned |
| Acquisition integration | Two codebases, two deployment pipelines, two team structures; integration velocity is constrained by the org boundary |
| New Director taking over | Inherited structure that was designed for a different stage or strategy |
| DORA metrics plateau or regress | Deployment frequency, lead time, or MTTR has stopped improving despite technical investment; structural friction is the likely cause |

Do not reorg in response to interpersonal conflict, a single bad quarter, or as a substitute for addressing a performance problem. Reorgs serving as disguised people moves produce cynicism persisting for years.

---

## The Four Team Types

The Team Topologies framework (Skelton & Pais, 2019) identifies four fundamental team types. The labels themselves carry little of the framework's value; the explicit recognition that different team types operate under different interaction modes and different mandates does the work. Conflating them causes predictable failure.

### Stream-Aligned Teams

**What they are:** A team aligned to a single flow of business value. Owns a product area, a customer journey, or a service domain end-to-end, from code to production. Responsible for the full lifecycle: build, deploy, operate, and iterate.

**Interaction mode:** Mostly autonomous. Consumes platform capabilities as self-service. Occasionally asks enabling teams for expertise during a capability ramp.

**Example:** The Payments team at a fintech owns the checkout flow, payment processing integrations, and fraud signals. They deploy independently. They are on-call for their own services. They do not wait for another team to provision infrastructure or review their deployments.

**When to use:** The default team type. If a team is not explicitly one of the other three types, it should be stream-aligned.

**Size:** 5–9 engineers. Below 5, you lose redundancy and on-call coverage. Above 9, coordination overhead begins to exceed delivery throughput.

**Warning signs that a stream-aligned team has too much scope:**
- Sprint velocity is dominated by unplanned work (bugs, incidents, coordination)
- Engineers cannot describe the blast radius of a change without checking with another team
- On-call rotations have fewer than 4 people, creating burnout risk

---

### Platform Teams

**What they are:** A team that builds and operates internal platforms that reduce cognitive load for stream-aligned teams. The platform is the product; stream-aligned engineers are the customers.

**Interaction mode:** X-as-a-Service. Stream-aligned teams consume platform capabilities through well-defined APIs, self-service portals, or shared tooling — not through ticket queues or Slack requests.

**Example:** A Developer Experience (DevEx) platform team at a growth-stage company owns the deployment pipeline, the Kubernetes cluster abstractions, the secrets management interface, and the observability stack. Stream-aligned teams deploy by pushing code; the platform handles the rest.

**When to use:** When 3+ stream-aligned teams are doing the same infrastructure or tooling work independently, or when SRE/infrastructure work is too intertwined with product delivery to be done well by any single product team.

**Critical constraint:** A platform team requiring stream-aligned teams to file tickets and wait operates as a gatekeeper; calling it a platform does not change the dependency dynamic. The success metric for a platform team measures reduction in time-to-production for stream-aligned teams; utilization of the platform team's engineers is the wrong metric.

**Size:** Platform teams are typically smaller than the teams they serve. A 4–6 person platform team can support 30–50 engineers if the self-service surface is well designed. If the platform team is larger than 20% of the total engineering org, something is wrong — either scope is too broad or the team is compensating for poor self-service with manual support.

**Common failure mode:** Platform team does heroic work keeping the infrastructure running but never builds the self-service layer that would allow stream-aligned teams to operate independently. This creates a dependency that grows linearly with the number of stream-aligned teams.

---

### Enabling Teams

**What they are:** A specialist team that helps stream-aligned (or platform) teams acquire a new capability, then steps back. Their goal is to make themselves unnecessary to the teams they help.

**Interaction mode:** Collaboration (time-bounded). An enabling team embeds, pairs, and teaches, then exits. It does not own the outcome — the stream-aligned team does.

**Example:** A security engineering team at Capital One-scale runs a 6-week engagement with the Payments team to implement DAST scanning and threat modeling in the delivery pipeline. At the end, the Payments team owns and operates those controls. The security team moves to the next engagement.

**When to use:**
- Introducing a new practice org-wide (observability, chaos engineering, accessibility testing)
- After an acquisition, to transfer institutional knowledge between codebases
- When stream-aligned teams consistently struggle with a specific domain (database performance, ML model deployment) that does not justify a full-time specialist on every team

**What it is not:** An enabling team operates differently from a shared services team; shared services do work on behalf of other teams indefinitely, and an enabling team drifting into the same pattern has become a bottleneck.

**Size:** 3–5. Enabling teams should be small and temporary. If an enabling team is permanent and large, it has become a de facto platform team and should be chartered as one.

---

### Complicated-Subsystem Teams

**What they are:** A team that owns a component where the complexity is high enough that only deep specialists can make changes safely. This is not general complexity — it is mathematical, scientific, or domain complexity that takes years to acquire.

**Interaction mode:** X-as-a-Service. Stream-aligned teams consume the subsystem through a versioned API or interface; they do not modify the internals.

**Example:** A Pricing Engine team at an insurance company owns a real-time actuarial pricing model. The underlying risk calculations require actuarial expertise that the rest of the engineering org does not have and cannot reasonably acquire. Stream-aligned teams call the pricing API; the team of actuarial engineers owns the model.

**When to use:** Sparingly. This team type fits only when the complexity genuinely requires specialists who cannot be distributed across stream-aligned teams. When the justification reduces to "it's complex," the team type is wrong; complexity engineers can learn belongs on a stream-aligned team.

**Warning signs of overuse:** If more than 10% of the engineering org is on complicated-subsystem teams, you probably have a platform problem being mislabeled, or a knowledge-hoarding dynamic that has been institutionalized.

---

## Cognitive Load Threshold Framework

Team Topologies defines three types of cognitive load: intrinsic (complexity of the domain itself), extraneous (process, coordination, tooling overhead), and germane (the learning investment that builds capability). The goal is to minimize extraneous load so teams can invest in germane load.

In practice, a team is over-threshold when extraneous load is consuming capacity that should go to delivery.

**How to detect an overloaded team:**

Run a capacity audit. Ask engineers to categorize their last two weeks of work into four buckets:

| Bucket | Description |
|--------|-------------|
| Feature delivery | New capability that ships to users |
| Sustaining work | Bug fixes, tech debt, dependency updates |
| Coordination overhead | Meetings, cross-team dependencies, waiting on approvals |
| Unplanned/reactive | Incidents, urgent escalations, context switches |

**Threshold:** If coordination overhead + unplanned/reactive exceeds 40% of capacity across three or more engineers on the team, the team is structurally over-scope. This is not a prioritization problem; it is an ownership problem.

**Secondary signal:** Measure the number of external Slack channels a team is required to monitor. If a team has primary attention responsibilities in more than 4–5 channels outside their own, their information surface is too wide.

**Tertiary signal (for platform-adjacent teams):** If a stream-aligned team is regularly blocked waiting for another team — more than twice per sprint on average — the platform boundary is in the wrong place or the platform is not self-service.

**Worked example:**

A 7-person API Gateway team at a mid-stage startup has accumulated ownership over: the external API, internal service mesh, developer portal, an authentication middleware layer, and the rate limiting service. During a quarterly planning session, engineers report that 55% of sprint capacity goes to coordination (with 4 downstream teams who depend on the gateway) and reactive incident response. Deployment frequency has dropped from 12 deploys/week to 3 over 18 months.

Diagnosis: The team's scope has grown to cover what should be split into two teams — a stream-aligned API Product team and a platform team for the service mesh and developer infrastructure. The auth middleware belongs with neither; it is a complicated-subsystem candidate.

---

## Inverse Conway Maneuver

Conway's Law predicts that organizations produce systems that mirror their communication structures. The Inverse Conway Maneuver turns this into a design strategy: define the architecture you want first, then restructure the org to match it.

**The sequence:**

1. **Define the target architecture.** Not implementation detail — service boundaries, ownership domains, and the key API contracts between them. This is a diagram with team names where service names will go.

2. **Audit the current org against the target.** For each boundary in the target architecture, ask: does one team own both sides of this boundary? If yes, the boundary is at risk — dependencies are invisible and change is uncontrolled. Does the boundary currently require coordination between 3+ teams? If yes, the boundary is overloaded.

3. **Identify the delta.** List the ownership changes required to match the target architecture. Some are team splits; some are team merges; some are responsibility transfers between existing teams.

4. **Execute incrementally.** Full reorgs carry high risk. The lower-risk execution path transfers ownership of one service domain at a time, with a defined overlap period (see Reorg Execution Checklist below).

**Example:** A platform engineering team at a 200-person engineering org wants to move from a shared monolithic data platform to domain-oriented data ownership (Data Mesh pattern). The Inverse Conway Maneuver means: before writing the first line of code for domain data products, reorganize so that each domain team (Payments, Identity, Growth) has an embedded data engineer. The architecture follows the org; trying to ship the architecture without the org change fails because teams will not own data products they were not structured to maintain.

**What it is not:** The Inverse Conway Maneuver is not an excuse to keep changing the org every time the architecture shifts. It should be used once, decisively, when there is a major architectural direction change. Repeated small reorgs in service of incremental architecture changes produce instability without benefit.

---

## Common Reorg Failure Modes

**Reorg that increases coupling**

A reorg merging teams with complementary ownership domains (to "improve alignment") often increases coupling because the combined team now holds a wider mandate but no cleaner boundaries. Engineers previously blocked by cross-team dependencies remain blocked; the block now lives inside the team instead of between teams.

*Detection:* If the reorg does not reduce the number of inter-team dependencies in your DORA data, it made things worse.

**Platform team becomes a bottleneck**

The platform team is formed with the right intent but never builds a self-service surface. Stream-aligned teams file tickets; the platform team has a growing backlog; everyone is frustrated. The platform team is understaffed and overloaded; the stream-aligned teams are slower than before the platform team existed.

*Fix:* Set an explicit KPI for the platform team: percentage of platform interactions resolving as self-service (no ticket, no Slack request). Target 80%+ within 12 months. Below 50% means the platform team needs to rebuild the interface; adding capacity does not address the root cause.

**Enabling team that never exits**

An enabling team gets stood up to spread a new practice, does excellent work, and then becomes a permanent fixture because the receiving teams have become dependent on them. The enabling team is now doing work that should belong to stream-aligned teams.

*Prevention:* Every enabling engagement has an explicit exit criterion defined before the engagement starts. Example: "The Security Enablement team's engagement with the Payments team ends when: DAST is integrated in the CI pipeline, threat model is documented and reviewed, and two Payments engineers have completed the security engineering certification."

**Reorg as cover for a people problem**

A reorg whose real purpose is moving a difficult person or a low-performing team out of a reporting line they have been blocking will not fix the underlying problem. The people problem re-emerges in the new structure, usually faster, and the reorg has cost the org 3–6 months of productivity.

**Over-specialization too early**

A 30-engineer org creating 4 team types because the framework says so. Complicated-subsystem teams and enabling teams at early stage are premature. A 30-person org should have stream-aligned teams with a nascent platform function. Adding enabling teams before there is consistent practice to enable produces process overhead without payoff.

---

## Decision Table: Signals to Team Type

| Signal | Recommended Action |
|--------|--------------------|
| Multiple teams duplicating infrastructure work | Stand up a Platform team; define the self-service interface first |
| One team owns too many service domains | Split into two stream-aligned teams along a clean ownership boundary |
| A capability gap (observability, security, ML) is blocking multiple teams | Time-bounded enabling team engagement; exit criteria defined upfront |
| A mathematical/scientific component requires deep specialists | Evaluate complicated-subsystem team; validate that the complexity is truly non-distributable |
| Team has >9 engineers with shared ownership | Split the team; split the service boundary with it |
| Team has <4 engineers carrying on-call rotation | Merge with adjacent team or redistribute ownership to reduce on-call risk |
| Post-acquisition team integration | Treat as Inverse Conway Maneuver; target architecture first, then org delta |
| DORA metrics plateau despite technical investment | Run cognitive load audit before assuming structural change is needed; the audit may surface a tool problem when a structure problem looks likely |

---

## Reorg Execution Checklist

The mechanics of a reorg matter as much as the design. A well-designed reorg executed poorly produces the same outcome as a poorly-designed one.

### Pre-announcement (4–6 weeks before)

- [ ] Target org design is documented: team names, charters, ownership domains, reporting lines
- [ ] All current service ownership is mapped; no orphaned services in the transition
- [ ] Every engineer has a home in the new structure; no one learns their team no longer exists without also learning where they land
- [ ] Key technical leads and staff engineers have been briefed and have had input; they are not surprises
- [ ] Manager-to-engineer ratios in the new structure are reviewed; no manager has more than 8 direct reports post-reorg
- [ ] Overlap periods are defined for each ownership transfer (minimum 2 weeks; 4–6 weeks for high-criticality services)
- [ ] Knowledge transfer plan is documented for each ownership transfer: what documentation needs to exist, who writes it, when

### Announcement

- [ ] Announce the full structure simultaneously; staged announcements create anxiety in the groups who have not heard yet
- [ ] Explain the "why" in terms of business outcomes; org efficiency alone reads as a platitude. "We are restructuring so that the Payments team can deploy independently without waiting on the Gateway team" lands more credibly than "We are restructuring to improve team autonomy"
- [ ] Separate the announcement from the effective date; give engineers 2–4 weeks between "this is happening" and "this is your team starting Monday"
- [ ] Provide a written FAQ that directly addresses: reporting changes, on-call rotation changes, project ownership changes, what stays the same

### Transition period

- [ ] Dual-rostering during overlap: engineers from both old and new teams are on-call together for transitioning services
- [ ] Runbooks and architecture documentation for transferred services are completed before the transition date; post-dated promises do not count
- [ ] A single DRI owns the transition for each service transfer; ambiguity about who is responsible for what produces incidents
- [ ] Weekly check-in with new team leads to surface friction early; the first 60 days are when structural problems become visible

### Post-reorg stabilization (30–90 days)

- [ ] First sprint retrospectives in the new structure are deliberately run; surface structural issues before they become resentments
- [ ] Measure: have the cross-team dependencies that motivated the reorg dropped? (Check DORA inter-team dependency data)
- [ ] Check on-call load distribution: is the reorg moving toward its load-balancing goal?
- [ ] Check in with engineers who changed teams; track engagement signals

---

## Metrics to Track Post-Reorg

Do not reorg and then wait 18 months to find out if it worked. Define a measurement plan before the reorg executes and review at 30, 60, and 90 days.

**Delivery metrics (DORA)**

| Metric | What to watch for post-reorg |
|--------|------------------------------|
| Deployment frequency (per team) | Should increase or hold steady; a drop indicates ownership confusion |
| Lead time for change | Should decrease as cross-team dependencies are reduced |
| Change failure rate | May increase temporarily during overlap period; should return to baseline within 60 days |
| Time to restore service (MTTR) | Should not worsen; if it does, ownership clarity is the likely issue |

**Team health metrics**

| Metric | Target | Notes |
|--------|--------|-------|
| Sprint capacity on coordination + unplanned work | <35% | Baseline before reorg; improvement expected at 90 days |
| On-call incident count per engineer per week | <2 (P2+) | Reorgs that spread ownership should reduce per-engineer load |
| Inter-team blocker frequency | Tracked in Jira; baseline + delta | The primary signal that the reorg addressed its root cause |

**Platform-specific (if a Platform team was created)**

| Metric | Target | Notes |
|--------|--------|-------|
| % of platform interactions that are self-service | >70% at 6 months, >85% at 12 months | Ticket count as denominator; self-service interactions as numerator |
| Time-to-first-deploy for new service | Should decrease quarter-over-quarter | A clear outcome metric for a platform team |

**What not to measure:** Do not track individual engineer productivity in the 90 days post-reorg. Context switching, knowledge transfer, and team forming all suppress individual output temporarily. This is expected and healthy. Measuring individual productivity during a transition creates pressure to avoid the knowledge transfer work that makes the transition succeed.

---

## Further Reading

- Skelton, Matthew, and Manuel Pais. *Team Topologies: Organizing Business and Technology Teams for Fast Flow.* IT Revolution Press, 2019. The foundational text for this framework; chapter 6 covers the Inverse Conway Maneuver in depth.
- Conway, Melvin E. "How Do Committees Invent?" *Datamation*, April 1968. The original paper; short and still precise.
- Forsgren, Nicole, Jez Humble, and Gene Kim. *Accelerate: The Science of Lean Software and DevOps.* IT Revolution Press, 2018. Provides the DORA metric definitions and the statistical evidence for their relationship to org performance.
