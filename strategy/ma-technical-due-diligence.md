# M&A Technical Due Diligence Framework

## Leadership Context

Technical due diligence is among the moments when an engineering leader's judgment directly shapes a business transaction. The quality of the technical assessment shapes acquisition price negotiation, integration budget, and — decisively — the timeline at which the combined engineering org will be able to move as a single unit after close. A diligence process that misses a $2M infrastructure migration, a GPL licensing exposure, or a five-person key-person risk does not fail in diligence; it fails 18 months post-close, when the cost is paid under integration pressure, at premium.

The business outcome: thorough technical diligence reduces integration risk, produces defensible inputs to purchase price adjustments, and gives the post-close engineering leadership team a credible integration plan on day one instead of day 60.

## Background and Motivation

This framework was synthesized from multi-year platform and compliance work in regulated environments at ActBlue Technical Services (2022–2025), spanning PCI environment deprecation, multi-regional infrastructure deployment, monolith decomposition, and disaster recovery validation. It does not derive from a single M&A diligence engagement — it represents the technical assessment posture developed across programs that required the same rigor: what would you need to know to take on this system, and what would it cost to fix what you found?

## When to Use This

The diligence emphasis changes based on acquisition type. Use this framework to select and weight domains, not to run every domain at full depth for every acquisition.

**Acqui-hire.** The primary asset is the team — engineering talent, culture, and working relationships. Architecture and technical debt assessments matter less. Key-person risk, team cohesion, and culture compatibility carry the weight. Prioritize: Team & Culture, Integration Complexity (people integration), Security Posture (because you are absorbing an endpoint fleet).

**Product acquisition.** You are buying a working product and its user base. Architecture scalability, operational maturity, and technical debt matter significantly because you will own the ongoing cost of running and evolving the product. Prioritize: Architecture & Scalability, Technical Debt & Quality, Operational Maturity, Data Practices.

**Technology acquisition.** You are buying IP, a technical capability, or a platform component that you intend to integrate into your own product. IP & Licensing and Integration Complexity dominate. Architecture matters insofar as it affects how extractable the technology is. Prioritize: IP & Licensing, Integration Complexity, Architecture & Scalability, Security Posture.

---

## Part 1: The 8 Assessment Domains

### Domain 1: Architecture and Scalability

**What you are evaluating:** Whether the system can handle the scale the combined business will require, and whether extending or evolving the architecture is tractable.

**Artifacts to request:**
- Architecture diagram (current state, not aspirational)
- Service dependency map
- Database schema overview
- Traffic and load data: p50/p95/p99 latency, peak RPS, current infrastructure costs
- Capacity limits and known bottlenecks

**Questions to ask in the architecture walkthrough:**
- What does your system look like at 10x current traffic? What breaks first?
- What was the last major architectural change? How long did it take, and what did you learn?
- What decisions do you regret most about the current architecture?
- What is the single biggest technical risk in the system today?

**Red flags:**
- No architecture documentation; the only authoritative knowledge is in a founder's head
- Single database serving all workloads with no read replicas or caching layer
- Synchronous inter-service dependencies at scale (one slow service blocks all callers)
- No load testing history; the team does not know where the system fails under stress
- Architecture that cannot be extended without a full rewrite (e.g., a monolith with no seam for new services)

**Yellow flags:**
- Significant coupling that will require refactoring before integration, but is documented and understood
- Infrastructure that is not cloud-native but has a plausible migration path
- Scaling limits that are above current traffic but below projected post-acquisition traffic within 12 months

---

### Domain 2: Technical Debt and Code Quality

**What you are evaluating:** The accumulated cost of past shortcuts and the ongoing drag those shortcuts impose on velocity. Technical debt is not inherently disqualifying — every engineering org carries it. The question is whether it is quantified, managed, and affordable.

**Artifacts to request:**
- Code coverage reports
- Static analysis output (SonarQube, CodeClimate, or equivalent)
- Dependency inventory (libraries, frameworks, versions)
- List of known technical debt items (if tracked)
- CI/CD pipeline configuration and pass rates

**Questions to ask:**
- How much of your codebase was written more than 3 years ago and has not been touched since?
- What is your oldest dependency? When was it last updated?
- What is your test coverage, and where is it lowest?
- If you had 3 months with no product requirements, what would you fix?

**Red flags:**
- Test coverage below 30% across core business logic
- Critical dependencies on end-of-life libraries with no upgrade path (e.g., Python 2, jQuery < 3, unsupported database versions)
- Build failures exceeding 15% of CI runs in the past 90 days
- No automated deployment; all deployments are manual and require a specific person
- Core business logic in undocumented stored procedures or scheduled jobs that no active engineer owns

**Yellow flags:**
- Coverage is uneven — high in new code, low in legacy systems
- Some dependencies are outdated but not on a critical path and have documented upgrade plans
- Technical debt is tracked and prioritized but has been backlogged due to product pressure (common; the question is whether leadership acknowledges it or rationalizes it away)

---

### Domain 3: Security Posture

**What you are evaluating:** Whether the target has implemented reasonable security controls and whether you are inheriting liability — data breaches that have not been disclosed, vulnerabilities that have not been patched, compliance obligations that have not been met.

**Artifacts to request:**
- Most recent penetration test report and remediation status
- Vulnerability management process documentation
- Access control policies (who has prod access, how is it managed)
- Incident history (security incidents in past 24 months)
- SOC 2 Type II report (if applicable)
- Data classification policy

**Questions to ask:**
- When was your last pen test? What were the critical findings, and what is the status of remediation?
- How is production access managed? Who has it, and how is it audited?
- Do you have a history of security incidents? Walk me through the most significant one.
- How are API keys, credentials, and secrets managed in your codebase and infrastructure?

**Red flags:**
- Unpatched critical CVEs in production systems
- Shared credentials for production access (no individual accountability)
- Secrets committed to version control — historically or currently
- A security incident in the past 24 months that was not publicly disclosed (creates undisclosed liability)
- No pen test in the past 18 months
- PII or payment card data in unencrypted storage or logs

**Yellow flags:**
- Pen test findings that are remediated but not verified by a follow-up test
- Access controls that work but are not formally documented
- Security policies that exist but are not enforced with tooling (honor system)
- SOC 2 in process but not yet completed

---

### Domain 4: Team and Culture

**What you are evaluating:** Whether the engineering team is the asset you think you are buying, whether key talent will stay through and after close, and whether the team's working culture is compatible with the acquiring org.

**Artifacts to request:**
- Org chart with tenure data
- Attrition history (past 24 months)
- Retention agreements (if any) proposed by the target or expected by the target team
- Offer letters and equity schedules for key engineers (to understand post-close vesting cliffs and retention risk)

**Questions to ask (in team interviews — not in leadership interviews alone):**
- What are you most proud of that you've built here?
- What would make you stay through and after the acquisition? What would make you leave?
- How does the leadership team make technical decisions? Give me a recent example.
- What is the biggest source of frustration in your engineering process today?

**Red flags:**
- Founder or CTO is the sole technical decision-maker; no evidence of distributed technical ownership
- Core engineers have significant unvested equity that will accelerate on acquisition (creates perverse exit incentive — they may leave immediately post-close regardless of retention agreements)
- Recent spike in attrition in the 6 months before close (engineers often know a sale is coming and start looking)
- Team culture is highly insular; the team has not worked with outside engineers or integrated external APIs at the human level

**Yellow flags:**
- Team is strong but small; losing 2–3 people post-close would meaningfully impair the team's capacity
- Culture is informal and low-process in ways that will require change under the acquiring org — change management cost, not a disqualifier
- Key engineers are loyal to the founder, not to the company; founder transition is a retention risk

---

### Domain 5: Operational Maturity

**What you are evaluating:** Whether the target can operate its own systems reliably, and what the ongoing operational burden you are absorbing looks like.

**Artifacts to request:**
- Incident history (past 12 months): frequency, severity, time-to-resolve
- On-call rotation structure
- Runbooks for critical systems
- SLA commitments and SLA performance data
- Deployment frequency and rollback rate

**Questions to ask:**
- Walk me through your most recent P0 or P1 incident. How did you detect it, how did you respond, and what changed after?
- How do you deploy? How often? What is your rollback procedure when a deployment goes wrong?
- What is your monitoring and alerting stack? How do you know when something is wrong?
- Who is on call, and what is the on-call burden? How many pages per week on average?

**Red flags:**
- No incident history tracked; incidents are handled ad hoc with no record
- On-call falls entirely on 1–2 engineers; bus factor in operational response
- No runbooks; system operation depends entirely on tribal knowledge
- Deployments are rare (monthly or less) because they are risky — a symptom of no automated testing, no rollback capability, or no confidence in the system
- Monitoring is absent or only surface-level (process-up vs. truly unhealthy)

**Yellow flags:**
- Operational processes exist but are inconsistently followed
- Incident frequency is elevated but trending downward (indicates awareness and action)
- On-call burden is high but the team acknowledges it and has a plan to reduce it

---

### Domain 6: Data Practices

**What you are evaluating:** Whether data is handled in compliance with relevant regulations, whether data assets are reliable and accessible, and what the data integration complexity looks like.

**Artifacts to request:**
- Data map: what data is collected, where it is stored, who has access
- Privacy policy and data retention policies
- GDPR/CCPA compliance documentation (if applicable)
- Data pipeline architecture overview
- Known data quality issues

**Questions to ask:**
- Where does your customer data live, and who can access it?
- How do you handle data deletion requests (right to erasure)?
- What is your data retention policy, and how is it enforced technically?
- How do downstream teams in the org consume data? Is there a data warehouse or BI layer?

**Red flags:**
- Customer PII stored without purpose limitation or retention enforcement
- Data deletion requests fulfilled manually with no audit trail
- Data practices that appear to conflict with stated privacy policy (legal liability)
- No data quality monitoring; downstream consumers have low confidence in data accuracy
- Cross-border data transfer without Standard Contractual Clauses or equivalent (EU-US data flows)

**Yellow flags:**
- GDPR/CCPA compliance is procedural but not fully automated; manual steps in deletion workflow
- Data warehouse exists but is not well-maintained; significant stale or duplicated data
- Data access is controlled but not formally audited

---

### Domain 7: IP and Licensing

**What you are evaluating:** Whether the acquiring company will own clean title to the intellectual property it is purchasing, and whether there are licensing obligations — from open source or vendor agreements — that constrain what you can do with the technology.

**Artifacts to request:**
- All open source licenses used (with version-level detail)
- Third-party software license agreements (commercial tools, SDKs, data providers)
- IP assignment agreements for all current and past employees and contractors
- Patent inventory (if any)
- Any existing IP disputes or claims

**Questions to ask:**
- Do all employees and contractors have signed IP assignment agreements?
- Have you ever used GPL-licensed code in a commercial or closed-source product? How did you handle it?
- Are there any vendor agreements that restrict how you can use or redistribute the technology?
- Have you ever received a DMCA or IP claim? How was it resolved?

**Red flags:**
- GPL v2 or v3 code incorporated into a commercial product without release of the full codebase under GPL (copyleft exposure — the license terms may require release of proprietary code)
- Contractors who built core IP without signed IP assignment agreements (creates a cloud on title — the contractor may have a claim)
- Vendor agreements with anti-assignment clauses (the agreement may terminate on change of control, requiring renegotiation post-close)
- Any pending or threatened IP litigation

**Yellow flags:**
- LGPL use in products; LGPL is permissive in most cases but requires linking to a dynamic library, not static — some architectures create unintended copyleft obligations
- IP assignments that are signed but predated key features; need to verify coverage
- Vendor agreements with no assignment clause specified — may require consent to transfer

---

### Domain 8: Integration Complexity

**What you are evaluating:** What it will cost — in time and engineering resources — to integrate this system into the acquiring company's product, infrastructure, and team.

**Artifacts to request:**
- API documentation (public-facing and internal)
- Authentication and identity architecture
- Infrastructure providers and toolchain (cloud provider, CI/CD, observability)
- Current third-party integrations and data flows

**Questions to ask:**
- If you had to expose your core functionality as an API to an external system, what would that take?
- What parts of the system would be hardest to separate or extract?
- What identity and auth system do you use, and what would it take to federate with our SSO?
- What data does your system depend on that comes from outside your system?

**Red flags:**
- Identity is baked into the application instead of abstracted; SSO integration requires a significant rearchitecture
- The system produces data in proprietary formats with no documented schema or export capability
- Heavy dependency on a third-party vendor that the acquiring company does not use and cannot easily replace
- Core logic is embedded in a framework or platform that is incompatible with the acquiring org's standards (e.g., .NET when the acquiring org is all Go/Kubernetes)

**Yellow flags:**
- Integration will require parallel-running systems for 6–12 months before migration is complete
- The target team has never integrated with an external identity system; the capability is not proven
- Toolchain divergence (different cloud providers, CI systems, observability stacks) that creates onboarding friction but not a hard technical blocker

---

## Part 2: Scoring Model

Rate each domain 1–5 for overall health. Apply weights based on acquisition type. Weighted score indicates overall technical health; use it as an input to price and integration budget negotiation, not as a hard go/no-go.

**Rating scale:**
- **5 — Strong.** Domain is a genuine asset; minimal integration or remediation work expected.
- **4 — Solid.** A few gaps; known and manageable. Not a risk driver.
- **3 — Adequate.** Visible weaknesses; integration or remediation will require investment. Budget accordingly.
- **2 — Concerning.** Material issues that will require significant resource or timeline adjustments. Flagged in executive summary.
- **1 — Critical risk.** A finding in this domain materially affects the acquisition decision or price. Escalate before close.

**Weight table by acquisition type:**

| Domain | Acqui-Hire | Product Acquisition | Technology Acquisition |
|---|---|---|---|
| Architecture & Scalability | 10% | 20% | 20% |
| Technical Debt & Quality | 10% | 15% | 10% |
| Security Posture | 15% | 15% | 15% |
| Team & Culture | 30% | 15% | 10% |
| Operational Maturity | 10% | 20% | 10% |
| Data Practices | 10% | 10% | 10% |
| IP & Licensing | 5% | 5% | 20% |
| Integration Complexity | 10% | 10% | 25% |
| **Total** | **100%** | **100%** | **100%** |

**Weighted score interpretation:**
- 4.0–5.0: Low technical risk; proceed with standard integration planning
- 3.0–3.9: Moderate risk; adjust price or require escrow for identified remediation costs
- 2.0–2.9: Elevated risk; integration budget should include 20–30% contingency; specific findings may warrant price reduction
- Below 2.0: High risk; recommend price reduction, extended escrow, or pass

---

## Part 3: The 2-Week Diligence Sprint

Two weeks is a compressed but workable timeline for technical diligence on a 20–100 engineer company. It requires parallel workstreams.

**Day 1–2: Kick-off and artifact collection**
- Establish NDA and data room access
- Request all artifacts across all 8 domains
- Schedule all stakeholder interviews (see below)
- Assign domain owners from the diligence team

**Day 3–5: Stakeholder interviews**
- CTO / VP Engineering (2 hours): architecture, team structure, strategic technical decisions, roadmap
- Staff or Senior Engineers (1 hour each, 3–4 people): "what would you fix if you could?" conversations; technical depth check; retention signal
- Engineering Manager(s) (1 hour): team health, attrition history, process maturity, on-call burden
- Head of Security or SecOps lead (1 hour): security posture, incident history, access control
- Data or Analytics lead (1 hour): data practices, data quality, compliance posture

**Interview ground rules:**
- Interview engineers without their leadership present for at least one session. Engineers speak openly about technical debt, leadership quality, and team morale when not monitored.
- Avoid leading questions ("Do you have any technical debt?"). Ask open-ended questions that require narrative answers ("What would you do first if you had 90 days with no product pressure?").
- Take notes on what is not said as much as what is: evasion, redirection, and vague answers about architecture or security are signals.

**Day 5–7: Code review and architecture walkthrough**
- Conduct a focused code review on 2–3 areas: core business logic, authentication/authorization, and a recent feature that the team is proud of
- Walk through the architecture diagram with the team's lead engineer; ask them to explain design decisions instead of describing components
- Review CI/CD pipeline: inspect test suite structure, coverage metrics, build history

**Day 8–10: Artifact analysis and scoring**
- Score each domain using the rubric
- Identify red flags and yellow flags per domain
- Draft integration complexity estimate with engineering counterpart

**Day 11–12: Report drafting**
- Write the technical diligence report (see Part 4)
- Identify the top 3 findings that affect price or integration budget
- Prepare executive summary (5 things every executive needs to see — see Part 4)

**Day 13–14: Internal review and delivery**
- Review report with the M&A team and legal counsel
- Surface any findings that affect deal terms (price adjustment, escrow, reps and warranties)
- Deliver report to acquiring company leadership

---

## Part 4: The Technical Diligence Report

### Format

**Section 1: Executive Summary (1 page)**
Five things every executive needs to see:

1. **Overall technical health score** — weighted score, rating, and a one-sentence characterization
2. **Top 3 risks** — the findings driving price, integration cost, or execution risk post-close
3. **Team assessment** — will the team stay? Who are the key people, and what is the retention risk?
4. **Integration cost estimate** — time and FTE cost to integrate the target's systems into the acquiring company's product and infrastructure
5. **Recommendation** — proceed, proceed with conditions (price adjustment, escrow, required remediation pre-close), or pass

**Section 2: Domain Assessments (1–2 pages per domain)**
For each domain: rating (1–5), key findings (bulleted), red flags, yellow flags, and recommended action (buy-down, escrow, integration budget item, or no action required).

**Section 3: Integration Plan Inputs (1 page)**
- Estimated integration timeline (quarters, not days)
- FTE requirements for integration (from the acquiring team)
- Key dependencies and critical path
- Integration risks (what could extend the timeline or cost)

**Section 4: Post-Close Risk Register (see template in Part 5)**

**Section 5: Appendix**
- Artifact inventory (what was provided vs. requested)
- Interview log (who was interviewed, dates, duration)
- Scoring rubric (with weights applied)

---

## Part 5: Common Diligence Blind Spots

### Hidden Dependencies

The architecture diagram shows the target's systems. It does not always show the third-party services, vendor APIs, and data providers that the system depends on silently. A SaaS product that depends on a vendor for payments, identity, email delivery, and search indexing may have four critical dependencies that do not appear in the architecture diagram because they are "just APIs."

**How to surface them:** Ask: "What would break in your product if Stripe stopped working? What about if SendGrid went down? What external services are you dependent on for your core user journey?" Build the dependency map from the user journey backward, not from the architecture diagram outward.

### Key-Person Risk

Every engineering org has people without whom the system becomes significantly harder to operate or evolve. In a small company, this is often the CTO or a founding engineer. In diligence, the risk is routinely disclosed ("oh, Mark knows that part of the system best") and then routinely underestimated.

**How to assess it:** Ask the team, not leadership: "If [person] left tomorrow, what would you lose? Is that documented anywhere?" If the answer is "we'd figure it out" without specificity, the knowledge is undocumented. If the answer is a long pause followed by "a lot," the risk is real.

**Retention risk amplifier:** Key engineers with unvested equity that accelerates on acquisition have no financial incentive to stay through the integration period. Model their post-close retention independently and factor it into the team assessment.

### Licensing Time Bombs

GPL and AGPL licenses can create copyleft obligations that are not immediately visible. A SaaS product that incorporates AGPL-licensed code (common in data tools and ML libraries) may have an obligation to release its source code under AGPL if any user can access the software over a network — even without distribution. This is often discovered post-close when legal reviews the dependency inventory for the first time.

**How to surface them:** Request a full dependency inventory with license data. Many build systems can generate this. Review any AGPL or GPL v3 dependencies used in the product layer (not solely development tooling). Engage IP counsel for any findings in this area — the engineering team may not understand the license implications.

---

## Part 6: Post-Close Integration Risk Register Template

Built during diligence; handed off to the integration team at close.

| Risk | Domain | Severity | Probability | Integration Impact | Mitigation | Owner |
|---|---|---|---|---|---|---|
| Key engineer (Alex, auth system) leaves post-close | Team & Culture | High | Medium | Auth system becomes unmaintainable without 3-month knowledge transfer | Retention agreement; pair Alex with acquiring-side engineer immediately post-close | Engineering Director |
| GPL v2 dependency in payments module | IP & Licensing | High | Confirmed | May require codebase disclosure or module rewrite | Legal review; rewrite module to remove dependency (est. 6 weeks, 1 senior engineer) | Engineering + Legal |
| No runbooks for database failover | Operational Maturity | Medium | Confirmed | First production incident will have extended TTR | Write runbooks in first 30 days post-close; acquiring team shadows target team on-call for 60 days | Site Reliability |
| On-call burden unsustainable at 2 people | Team & Culture | Medium | High | Attrition risk if on-call is not redistributed | Expand on-call rotation to 5 people using acquiring team members by month 2 | Engineering Manager |
| Infrastructure on custom bare-metal (not cloud) | Architecture | Medium | Confirmed | Cloud migration required before convergence with acquiring team's K8s platform (est. 12 months) | Parallel-run strategy; no forced migration in first 6 months | Platform Engineering |
| CCPA deletion workflow is manual | Data Practices | Low | Confirmed | Compliance risk; 1–2 week remediation | Automate deletion workflow before customer data migration | Data Engineering |

**Severity:** High (affects close decision or price), Medium (integration budget item), Low (operational task post-close)

**Probability:** Confirmed (known finding), High (>70% likely to materialize), Medium (30–70%), Low (<30%)

The risk register should be reviewed at 30, 60, and 90 days post-close in integration governance meetings. Risks that materialize should be tracked against their mitigation plans. Risks that resolve should be closed with a note on outcome.

---

## Implementation Checklist

- [ ] Determine acquisition type (acqui-hire, product, technology) and apply the appropriate domain weights before starting diligence
- [ ] Assign domain owners from the diligence team — one person per domain, accountable for artifacts, interviews, and scoring
- [ ] Request all artifacts at kick-off; track what is provided vs. what is not (gaps in artifact provision are themselves a signal)
- [ ] Schedule interviews with engineers without leadership present for at least one session
- [ ] Run the code review against core business logic and authentication — not the cleanest code, the most important code
- [ ] Build the dependency inventory with license data; flag all GPL, LGPL, and AGPL dependencies for legal review
- [ ] Verify IP assignment agreements exist for all current and past contributors to core systems
- [ ] Score all 8 domains; calculate weighted score; draft the executive summary before the full report
- [ ] Build the post-close risk register during diligence — not after; information is freshest during active diligence
- [ ] Identify top 3 findings that should affect deal terms and socialize them with M&A counsel before the report is finalized
- [ ] Hand off the risk register to the integration team lead at close with a 90-day review cadence established

---

## Common Failure Modes

**Architecture-only diligence.** A common failure is a technical diligence that spends 80% of its time on system architecture and 20% on everything else. Architecture matters, but it is also the domain a prepared target company can present well in a short window. Key-person risk, licensing obligations, and operational maturity are harder to assess and harder to fake.

**Trusting the data room over interviews.** Artifacts are curated. The data room contains what the target company chose to show you. Interviews — especially with engineers without leadership present — surface what the data room does not.

**Delivering a report that requires translation.** The technical diligence report serves two audiences: the engineering team driving integration planning, and the executive team making the final decision. Both audiences need their questions answered without translation. Write the executive summary first; if the engineering team lead cannot explain it to the CFO in 5 minutes without the appendix, the summary is not ready.

**Treating post-close integration as someone else's problem.** The engineer who ran diligence knows where the bodies are buried. If that knowledge does not transfer to the integration team at close, the integration team will rediscover every risk register item the hard way. Build the handoff into the diligence process, not as an afterthought.
