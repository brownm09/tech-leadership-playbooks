# Launch Readiness Checklist

## Leadership Context

The decision to launch belongs with leadership; engineering provides the information that decision rests on, and the quality of the decision depends on the quality of that information. Unstructured go/no-go conversations produce launches that slip because no one owned the call, launches that proceed when they should not because the relevant concern was never surfaced, and launches that generate on-call crises because operational readiness was assumed without being verified. This playbook establishes a structured gate framework that gives the person making the go/no-go call a complete picture across eight readiness domains, a documented record of who accepted which risks, and a launch day sequence with explicit decision points. When a launch fails, the gate record answers the question executives will ask: what did we know, and what did we decide?

## Background and Motivation

This checklist was developed from the launch readiness work on the payment processor migration to Stripe at ActBlue Technical Services (2022–2025). I led the program and defined scope and sequencing; my team executed the migration. Outcome: 2,600+ entity accounts migrated; payments codebase shrunk by 7,000+ lines. The eight readiness domains reflect the failure modes payment-path releases expose — surfaced through hands-on launch management, not derived from a template.

## When to Use This

Apply this launch gate framework for:

- Any release that directly affects external customers (new feature, pricing change, data processing change)
- Major infrastructure changes (database migrations, cloud provider changes, networking changes, authentication system updates)
- Compliance-relevant releases (data residency, retention policy changes, third-party data sharing, GDPR/HIPAA-relevant features)
- Launches coordinated with a customer, partner, or press announcement
- Any release that, if it fails, would require an all-hands incident response

Do not apply this framework to routine weekly deployments of well-tested features to existing surfaces. The overhead of a full gate process should be reserved for launches with meaningful blast radius. For routine releases, the CI/CD pipeline governance guide provides the appropriate level of controls.

**Minimum threshold:** If rollback would take more than 30 minutes, run the gate process.

---

## The 8 Readiness Domains

### Domain 1: Engineering

Code complete means all features in the launch scope have been merged, reviewed, and deployed to the staging environment. It does not mean "in progress" or "mostly done."

**Checklist:**
- [ ] All launch-scope features merged and code-reviewed (no open PRs in scope)
- [ ] Unit test coverage at or above team threshold (define threshold before launch date, not the day before)
- [ ] Integration tests passing in staging environment
- [ ] Performance benchmarks run and results reviewed — p50, p95, p99 latency under load conditions that represent expected launch traffic, not typical traffic
- [ ] Load test completed at 2× expected peak traffic (or rationale documented for why this threshold is appropriate)
- [ ] No P0 or P1 bugs open in launch scope; P2 bugs reviewed and either resolved or waivered with rationale
- [ ] Feature flags configured correctly for launch state (confirm flags are in the expected position for launch, not left in a test state)
- [ ] Database migrations tested on a production-representative data volume
- [ ] Third-party API integrations tested with production credentials in staging (not test keys, which may have different rate limits and behavior)

**Owner:** Engineering lead for the primary service(s) in scope.

### Domain 2: Operations

A feature that works but cannot be operated is not ready to launch. Operations readiness covers the tooling and documentation the team will use after the launch — the post-launch operating model, not the launch event.

**Checklist:**
- [ ] Runbooks written for the top 3–5 expected failure modes of the launched feature
- [ ] Monitoring dashboards exist and display the right signals (not generic service dashboards repurposed for this feature — dashboards with metrics specific to the launched functionality)
- [ ] Alerting configured with:
  - At least one alert on the primary success metric (e.g., checkout completion rate, API success rate)
  - At least one alert on the primary error signal (5xx rate, exception rate, queue depth)
  - Alert thresholds calibrated to production baseline — not default values from the monitoring platform
- [ ] On-call engineer briefed on the launch, the feature behavior, and the runbooks before launch begins
- [ ] Operational ownership confirmed: which team is on call for this feature, and do they know it?
- [ ] Log verbosity reviewed — sufficient to diagnose failures without flooding the log aggregation pipeline

**Owner:** Engineering lead plus on-call rotation lead.

### Domain 3: Security

Security review is not a checkbox that legal requires. It is the domain where unreviewed assumptions about data flow, access control, and authentication surface before they are exploitable.

**Checklist:**
- [ ] Threat model reviewed and updated for the launched feature (if the feature processes new data types, integrates a new third party, or changes authentication/authorization logic — a fresh threat model is required)
- [ ] Pen test scope defined: if the launch is compliance-relevant or involves a new attack surface, define what will be tested and by whom, even if the pen test runs post-launch
- [ ] Dependency vulnerability scan completed and results reviewed (SCA); critical and high CVEs either remediated or waivered with expiration date
- [ ] Authentication and authorization behavior verified in staging — test the paths that should fail in addition to the paths that should succeed
- [ ] Secrets management confirmed: no credentials, tokens, or API keys in application code or pipeline configuration
- [ ] Data encryption confirmed: data at rest and in transit are encrypted per the organization's baseline
- [ ] Access control review: who can access what the feature exposes, and does that match the intended design?

**Owner:** Security lead (or engineering lead if no dedicated security team, with sign-off from a senior engineer who was not the feature author).

### Domain 4: Compliance

Compliance readiness is the domain most often treated as a final rubber stamp and least often treated as an integrated part of launch planning. When compliance review is not started until two weeks before launch, either the launch slips or corners are cut.

**Checklist:**
- [ ] Legal sign-off obtained on: terms of service changes (if any), data processing changes, any change that affects the contractual relationship with customers
- [ ] Data privacy review completed: what new personal data is collected, where it is stored, how long it is retained, who has access, and is this consistent with the privacy policy?
- [ ] GDPR/CCPA/HIPAA applicability confirmed and controls verified (if applicable — confirm which regulations apply before the gate, not during it)
- [ ] Data residency requirements verified: if customers have contractual data residency requirements, confirm that the launched feature complies
- [ ] Third-party data sharing review: if the feature sends customer data to a new third party, confirm that DPAs (data processing agreements) are in place and the data flow is disclosed in the privacy policy
- [ ] Regulatory notification requirements reviewed: some changes require advance regulatory notification; confirm this requirement has been evaluated

**Owner:** Legal/compliance lead. Engineering lead is responsible for ensuring this review was initiated with sufficient lead time (minimum 2 weeks; 4 weeks for complex privacy implications).

### Domain 5: Support

Customer support is the first team to feel the blast from a launch failure and the last team included in launch planning. Launching without a support-ready state generates a wave of unroutable tickets and a customer experience that undermines the feature's reception.

**Checklist:**
- [ ] Support team trained on the feature before launch: what it does, what the expected user experience is, what the common error states look like, and how to interpret them
- [ ] Escalation path defined: what questions or issues does support escalate to engineering, and to whom specifically? Named escalation contacts, not "the engineering team"
- [ ] Known issues documented for support: the bugs that exist and will not be fixed before launch — support needs to know about these before customers report them
- [ ] Internal knowledge base or FAQ updated (if the feature changes existing workflows)
- [ ] Support SLA impact assessed: will this launch meaningfully increase ticket volume? If yes, capacity planning completed

**Owner:** Support lead (or customer success lead if support is embedded). Engineering lead initiates the support briefing at least one week before launch.

### Domain 6: Communications

The communication plan covers both internal and external audiences. Internal communications prevent launch-day surprises for people who find out about the launch from a customer complaint. External communications set customer expectations.

**Checklist:**
- [ ] Internal announcement drafted and approved (audience: all of engineering, sales, account management, support, executive team)
- [ ] External communications drafted and reviewed: blog post, email announcement, in-product notification, or press release — whichever applies
- [ ] Legal and compliance reviewed external communications (required if communication describes data handling, pricing, or regulatory compliance)
- [ ] Communication timing confirmed: internal announcement goes out at T-24h; external at T-0 or coordinated with launch timeline
- [ ] Social media/PR response plan in place: who responds if there is negative public reaction to the launch?
- [ ] Customer-facing documentation (help center, API docs) updated and ready to publish at launch

**Owner:** Product or marketing lead for external communications. Engineering lead for internal technical announcements.

### Domain 7: Rollout Plan

A launch without a rollout plan is a binary on/off event. Most non-trivial launches should not be binary.

**Checklist:**
- [ ] Canary percentage defined: what percentage of traffic or users receives the launch in the first phase? Typical starting points: 1%, 5%, 10% depending on risk level
- [ ] Canary success criteria defined: which metrics must be healthy at canary scale before expanding rollout? (e.g., "error rate below 0.1%, p99 latency below 300ms, zero payment failures in the cohort")
- [ ] Canary hold period defined: how long does the launch stay at canary scale before expanding? (minimum 1 hour for low-risk; 24–48 hours for high-risk)
- [ ] Rollback trigger defined and documented: what specific metric or event causes the team to roll back, and who makes that call?
- [ ] Rollback procedure tested end-to-end — a person has run through the rollback procedure in staging and confirmed it works within the target time window
- [ ] Full rollout schedule defined: canary → partial (25–50%) → full, with time estimates between each phase

**Owner:** Engineering lead, confirmed by TPM.

**Rollback trigger examples:**
- Error rate exceeds 1% for 5 consecutive minutes at canary scale
- Any payment failure attributed to the launched feature
- p99 latency exceeds 500ms (or 2× baseline, whichever is more restrictive) for 10 consecutive minutes
- On-call engineer's judgment that the launch is creating customer impact

### Domain 8: Business

The business gate exists because launches affect more than engineering. Sales teams need to know what they can commit to. Account managers need to know what their customers will see. Executives need to know they are aware before the launch happens, not after.

**Checklist:**
- [ ] Executive sponsor has provided explicit go/no-go sign-off (email or documented verbal in the gate record)
- [ ] Sales enablement completed: if the feature changes pricing, packaging, or the product's competitive positioning, sales leadership has been briefed and enablement materials are ready
- [ ] Account management notified: for features that directly affect existing customer accounts, the account team has been informed and has a response plan for customer questions
- [ ] Success metrics confirmed: what are the 30-day and 90-day metrics the business will use to evaluate whether the launch achieved its goals? These should be agreed before launch, not reverse-engineered after.
- [ ] Revenue or contractual commitments tied to this launch reviewed: if a customer contract or sales commitment depends on this launch date, confirm the launch is on track to honor it

**Owner:** Product or engineering executive sponsor.

---

## Owner-Per-Domain Accountability Table

Before the gate meeting, confirm that every domain has a named owner who will attest to readiness. An owner is an individual — not a team, not "TBD."

| Domain | Owner | Status | Notes |
|--------|-------|--------|-------|
| Engineering | Marcus Webb | 🟢 Ready | Load test completed Mar 28 |
| Operations | Sarah Chen | 🟡 At Risk | Alert thresholds not yet calibrated; completing by Apr 1 |
| Security | Priya Agarwal | 🟢 Ready | Threat model reviewed Mar 25 |
| Compliance | Jordan Lee (Legal) | 🟢 Ready | DPA signed with vendor Mar 20 |
| Support | Chris Nguyen (Support Lead) | 🟢 Ready | Training completed Mar 29 |
| Communications | Alex Park (Marketing) | 🟡 At Risk | Blog post pending final legal review |
| Rollout Plan | Marcus Webb | 🟢 Ready | Canary plan documented |
| Business | VP Product | 🟢 Ready | Executive sign-off received |

---

## Go/No-Go Decision Process

### Who Calls It

One person calls the go/no-go. Not the committee. The committee provides input; one person decides and is accountable. For most external-customer launches: the engineering lead or VP of Engineering. For compliance-relevant launches: VP of Engineering plus legal lead, with explicit dual sign-off.

If the decision-maker is unclear before the gate process begins, clarify it in the program brief — not the day of the launch.

### Blocker vs. Waiver

**A blocker** is a readiness item that is not complete and, if the launch proceeds, would directly cause customer harm, data loss, security exposure, or a compliance violation. Blockers stop the launch.

**A waiverable item** is a readiness item that is not complete but the risk of proceeding can be accepted, documented, and mitigated. Examples: load test not run at exactly 2× peak (ran at 1.5×); alert thresholds calibrated but not validated against historical data; external blog post delayed by 24 hours.

### Waiver Approval Chain

All waivers are documented before the launch, not after. The waiver record includes:

- What item is being waivered
- Why it is not complete
- What the risk is if the item is not complete
- What mitigation is in place
- Who approved the waiver and when

| Domain | Waivered Item | Risk | Mitigation | Approved By | Date |
|--------|--------------|------|------------|-------------|------|
| Engineering | Load test at 1.5× peak, not 2× | Higher-than-expected traffic could degrade p99 latency | Canary phase at 1% with 24h hold; expand only if p99 < 300ms | VP Eng | Apr 1 |

No verbal waivers. If it is not in the waiver log, it is not waivered — it is a blocker.

---

## Launch Day Runbook Template

### T-24 Hours

- [ ] Confirm all domain owners are available and reachable for launch window
- [ ] Final status check: all blockers resolved, all waivers documented
- [ ] Go/no-go call confirmed in writing (email or Slack, timestamped)
- [ ] Rollback procedure confirmed with the person responsible for executing it
- [ ] On-call engineer briefed and has access to runbooks
- [ ] Monitoring dashboards opened and baselined — record pre-launch metric values for comparison
- [ ] Communication drafts finalized and ready to send on schedule
- [ ] Support team notified: launch is proceeding at [time]

### T-1 Hour

- [ ] Deployment artifact confirmed in the release pipeline (correct SHA, correct environment)
- [ ] Feature flags confirmed in launch position in staging
- [ ] All domain owners or designated stand-ins online or on-call
- [ ] Incident channel or war room opened (Slack channel or Zoom bridge — ready to use, not to create in the moment)
- [ ] Monitoring alerts muted for any alerts known to fire during deployment (document which ones and why)

### T-0: Launch Begins

- [ ] Deploy initiated by designated engineer
- [ ] Canary traffic confirmed routing to new version (verify in monitoring; the deployment tool alone is insufficient)
- [ ] First 5-minute check: error rate, latency, primary success metric — compare to baseline
- [ ] Internal announcement sent
- [ ] Clock starts on canary hold period

**If any metric exceeds rollback trigger within the first 15 minutes:** the engineering lead calls the rollback. No committee decision. One person calls it, names the trigger, and initiates rollback. Document the decision.

### T+1 Hour

- [ ] Canary metrics stable (error rate, latency, primary success metric within expected range)
- [ ] On-call engineer has reviewed the monitoring dashboard and confirmed no anomalies
- [ ] Any issues identified in the first hour documented in the issue log
- [ ] Rollout decision: hold at canary, expand to next tier, or roll back — engineering lead makes the call

### T+24 Hours

- [ ] 24-hour metric review: success metrics compared to target; any negative trends?
- [ ] Support ticket volume reviewed: is volume within the expected range?
- [ ] Any issues resolved or tracked in the issue log
- [ ] Decision: expand rollout, hold at current %, or roll back
- [ ] Engineering lead sends 24-hour launch update to executive stakeholders (2–3 sentences: status, key metric, any issues)

---

## Post-Launch Review Cadence

### 24-Hour Review

Attendees: engineering lead, TPM, on-call lead. 30 minutes.

- Key metrics vs. targets
- Any issues opened in the first 24 hours
- Rollout progression decision
- Action items for the first week

### 72-Hour Review

Attendees: engineering lead, product lead, support lead. 30 minutes.

- Metric trends (are they stable, improving, degrading?)
- Support ticket volume and top issues
- Any rollback-warranting signals?
- Go/no-go for full rollout if not already complete

### 2-Week Review

Attendees: full domain owner group. 60 minutes.

- Business metrics vs. targets (30-day and 90-day targets framed)
- What went well in the launch process?
- What should change for the next launch?
- Any follow-on work required (bug fixes, performance work, documentation gaps)?
- Gate framework improvements based on what was discovered during this launch

---

## Common Launch Failures

**Launching on Friday.** The logic is always the same: the launch has been delayed and the team wants to get it out before the weekend, or the launch is on a fixed schedule and Friday happened to be the date. The problem: on-call coverage is thinner on weekends, incident response is slower, and a Friday launch that starts going wrong at 5pm has the worst possible conditions for rapid response. Move the launch to Tuesday–Thursday, or accept that the Friday launch requires explicit weekend coverage planning and approval from the on-call lead.

**Skipping canary.** Full-traffic launches feel faster, and they are — until something goes wrong. A 1% canary that catches a bug affects 1% of users. A full-traffic launch that catches the same bug affects 100% of users. The canary is not for launches you are confident about. It is for every launch.

**Go/no-go by committee with no one owning the call.** The meeting ends with "we think we're good" and no one wrote down who said go. When the launch fails, no one remembers who made the call. Designate the decision-maker before the gate process begins. Record their decision.

**Operations readiness treated as post-launch work.** "We'll add proper monitoring after we see how it behaves in production." This is not a plan — it is an intention to operate blind and learn from failures because the metrics that would have prevented them do not exist yet. Monitoring and alerting are launch requirements, not follow-on items.

**The readiness gate that is really a status broadcast.** Domain owners show up, report green across all domains, and the meeting ends in 10 minutes. No one asked hard questions. The gate is serving as a ceremony, not as a real control. A well-run gate asks: what could still go wrong, who would know first, and what would we do about it? If the answer to all three is clear, the gate is doing its job.

## Further Reading

| Topic | Resource |
|---|---|
| Release readiness and continuous delivery | Humble & Farley, *Continuous Delivery* (Addison-Wesley, 2010) |
| Release engineering practices | Google SRE Book — [Release Engineering](https://sre.google/sre-book/release-engineering/) |
