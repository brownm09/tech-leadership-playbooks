# LaunchDarkly Rollout Governance

## Leadership Context

Feature flags decouple deployment from release, letting an engineering org ship continuously without betting the business on each deploy. Governance determines whether flags remain a controlled instrument or accumulate as hidden complexity slowing delivery, obscuring system behavior, and growing into debt no one cleans up.

## Purpose

Feature flags without governance become permanent conditionals — undocumented, unowned, unremoved. This guide covers the policies and practices keeping a LaunchDarkly implementation useful over time: flag naming, lifecycle management, targeting rules, rollout sequencing, and a cleanup process preventing flag debt from accumulating.

## Background and Motivation

This governance framework was developed from the LaunchDarkly rollout at ActBlue Technical Services (2022–2024). I managed the vendor relationship, designed the proof-of-concept, and drove cross-team adoption across teams inside and outside my direct management. The platform team built the proof-of-concept; product engineering teams participated in rollout. Two tracks ran in parallel from day one: stakeholder and vendor alignment for the non-trivial customer-facing use case, and DevEx implementation of the first trivial flag. Org-wide resistance to the plan required direct intervention; I ghostwrote the VP of Engineering's communications to overcome it. The first trivial feature flag reached production within 4 months of the program mandate; the first non-trivial customer-facing flag shipped at 7 months (4 months of stakeholder and vendor alignment, 3 months of execution running parallel to the DevEx implementation track).

## Flag Taxonomy

Flags serve different purposes and should be managed differently. The most useful distinction:

| Type | Purpose | Expected lifespan |
|------|---------|-------------------|
| Release flag | Gates a feature in development from reaching users | Short: days to weeks; removed after full rollout |
| Experiment flag | Controls exposure for an A/B or multivariate test | Medium: duration of the experiment; removed after conclusion |
| Permission flag | Gates access by user segment, plan tier, or account | Long: may be permanent if tied to entitlements |
| Kill switch | Allows a feature to be disabled in production without a deploy | Indefinite; reviewed periodically |
| Operational flag | Controls system behavior (timeout values, rate limits, algorithm selection) | Varies; reviewed on a defined schedule |

Mixing types without labeling produces a flag list no one can audit: safe-to-remove, load-bearing, and abandoned flags blur together.

**Recommended approach:** Use LaunchDarkly tags to label flag type (e.g., `release`, `experiment`, `kill-switch`). Add a second tag for the owning team.

## Naming Convention

Consistent naming makes the flag list scannable. A flag named `flag-123` or `new-checkout` tells the next engineer nothing.

**Format:** `[team]-[area]-[description]`

Examples:
- `payments-checkout-new-flow` — payments team, checkout area, new flow feature
- `platform-api-rate-limit-v2` — platform team, API area, rate limit v2
- `growth-onboarding-ab-step3` — growth team, onboarding experiment, step 3 variant

Rules:
- All lowercase, hyphen-separated
- No dates in flag names (use the flag creation date in LaunchDarkly metadata instead)
- Description names what the flag controls ("new-flow" over "q3-initiative")

## Flag Lifecycle

Every flag should move through defined states. LaunchDarkly's built-in lifecycle stages (Active, Deprecated, Archived) map to this:

1. **Created:** Flag is in development. Targeting rules are configured. Default serves the control (off/false) for all users.
2. **Active rollout:** Flag is being rolled out. Targeting rules are live. Flag owner monitors metrics.
3. **Fully rolled out:** Flag serves the treatment (on/true) for 100% of users. Flag is a candidate for removal.
4. **Deprecated:** Flag is scheduled for code removal. Engineers are notified. Removal PR is open or assigned.
5. **Archived:** Code references removed. Flag is archived in LaunchDarkly. Not deleted (for audit purposes).

The common failure mode: flags reach "fully rolled out" and stay there indefinitely. Step 3 should trigger a removal task.

## Rollout Sequencing

Do not go from 0% to 100% in one step.

**Standard rollout sequence for a release flag:**

1. **Internal users only** (employees, internal accounts): validate basic functionality
2. **5% of production traffic:** watch error rates, latency, business metrics for 24-48 hours
3. **25%:** continue monitoring; confirm metrics stay stable
4. **50%:** confirm no long-tail issues
5. **100%:** full rollout; schedule flag removal

Adjust the timing between steps based on traffic volume. A high-traffic payments flow needs more time at each step than a low-traffic admin feature. For high-risk changes (payment processing, authentication), treat the 5% step as a canary with automated rollback triggers configured.

**Rollback:** The flag serves as the rollback mechanism. Before a feature ships behind a flag, the team confirms: "if something goes wrong, we can set this flag to off and the problem goes away." Failed confirmation means the flag controls the wrong surface area; find the actual control before shipping.

## Targeting Rules

Targeting rules determine who sees what. Poorly configured targeting drives a large share of flag-related incidents — often more than the flag logic itself.

**Principles:**

- Default rule (what users get when no other rule matches) should be the safe/control state until the flag is fully rolled out
- Do not use targeting rules to approximate what should be a permission system; if access control is the requirement, use the application's permission layer
- Targeting by user ID for experiments; by percentage rollout for gradual releases; by account/org attribute for entitlement flags
- Document non-obvious targeting rules in the LaunchDarkly flag description; LaunchDarkly indexes and searches descriptions

**Experiment targeting:** A/B tests require consistent user assignment (the same user sees the same variant on every visit). Use LaunchDarkly's user-key consistent bucketing. Session-keyed assignment produces inconsistent results across visits and degrades experiment validity.

## Metrics and Guardrails

For any flag backing a release or experiment, define success metrics and guardrail metrics before the flag goes live.

**Success metrics:** What the flag is intended to improve (conversion rate, page load time, error rate).

**Guardrail metrics:** What must not get worse (payment success rate, p99 latency, error budget). If a guardrail metric degrades during rollout, the rollout pauses and the flag rolls back, regardless of success metric performance.

LaunchDarkly Experimentation integrates with analytics and monitoring platforms. For teams using Datadog, connect flag state changes to Datadog events; rollouts then appear on dashboards alongside infrastructure and application metrics, and a metric change traces back to a flag state change in one view.

## Flag Cleanup Policy

Flag debt compounds. A project shipping a flag per sprint without a cleanup policy accumulates hundreds of stale flags within a year.

**Cleanup triggers:**
- Release flag at 100% rollout for 2+ weeks with no rollback incidents: schedule removal
- Experiment flag with a concluded result: archive within one sprint of conclusion
- Flag with no evaluation events in 30 days: review and archive if no longer needed

**Cleanup process:**
1. Identify candidates (LaunchDarkly flag health report, or a periodic audit in the sprint backlog)
2. Search codebase for flag key references
3. Open PR to remove flag evaluation code and dead code branches
4. Merge and deploy
5. Archive flag in LaunchDarkly (do not delete; archived flags preserve audit history)

Assign flag cleanup as a recurring task. One sprint per quarter dedicated to flag debt keeps the list manageable.

## Governance Checklist for New Flags

Before creating a flag:

- [ ] Flag type is defined (release, experiment, kill switch, permission, operational)
- [ ] Flag name follows the naming convention
- [ ] Owning team tag is applied
- [ ] Default rule serves the control state
- [ ] Success and guardrail metrics are defined (for release and experiment flags)
- [ ] Rollout plan is documented (percentage steps and timing)
- [ ] Rollback procedure is confirmed: setting flag to off restores the prior state

Before archiving a flag:

- [ ] All code references to the flag key have been removed
- [ ] Dead code branches have been cleaned up, including the conditional logic the flag controlled (not only the flag call itself)
- [ ] PR is merged and deployed to all environments
- [ ] Flag is archived in LaunchDarkly

## Common Problems

**No off state:** Removing the "off" code path after full rollout took the kill switch with it; disabling the flag now requires a deploy. Fix: keep the off code path until the flag is archived.

**Experiment results ignored:** An experiment concluded, one variant won, and the flag stayed in the codebase along with the losing variant. Fix: treat experiment conclusion as a cleanup trigger — schedule the removal PR in the same sprint.

**Targeting rules owned by one person:** A targeting rule was set up by an engineer who left the team. No one knows what it does or whether it can be changed. Fix: document targeting rules; review and reassign ownership during offboarding.

**Flag created to avoid a deploy:** A deploy sometimes solves what a flag would only delay. Flags add evaluation overhead and long-term maintenance cost; reach for one when gradual rollout, instant rollback, or controlled experimentation justifies the cost. Avoiding the discomfort of deploying does not.
