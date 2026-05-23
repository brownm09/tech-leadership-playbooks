# RFC and Design Doc Process

## Leadership Context

Every technical team makes architectural decisions about service boundaries, data models, infrastructure choices, integration patterns, and operational practices. In small teams, these decisions happen in conversation and get documented after the fact, if at all. In larger teams or across teams, undocumented decisions become a recurring source of friction: engineers make incompatible assumptions, the person who made the original decision leaves, or a new engineer re-investigates and re-proposes a decision already made for good reason.

The RFC (Request for Comments) process gives significant technical decisions a structured, reviewable, documented path. The RFC process operates as a forcing function: articulating a decision's rationale in writing before implementing it, making the decision visible to the people it will affect, and creating a searchable record future engineers can find when they encounter the same question.

The relationship between an RFC and an Architectural Decision Record (ADR) gets confused frequently. An RFC proposes: it describes a decision not yet made, invites feedback, and documents the options considered. An ADR records: it captures a decision already made, along with the context, rationale, and consequences. Not every RFC produces an ADR — some RFCs conclude with no change to the existing approach. Every significant ADR should be preceded by an RFC-equivalent process, though the process may run lighter for smaller decisions.

## Background and Motivation

I grounded this framework in technical decision-making across two contexts: ActBlue Technical Services (2022–2025) and Community Tech Alliance (2025–2026). At ActBlue, the scale of the platform directorate — six teams, significant cross-system dependencies, regulated infrastructure — required structured decision-making for architectural changes. Decisions of similar irreversibility and blast radius (the payment processor migration to Stripe, the Heroku-to-Kubernetes migration, the PCI environment deprecation) benefit from the structured review the RFC process provides.

The same friction surfaces even in a small team where the instinct is to decide in Slack and move on. The friction RFC process resolves comes from documentation and asynchronous review discipline; the friction is not primarily size-related.

The RFC format in this document draws from the Rust RFC process and from the patterns Will Larson describes in his writing on engineering decision-making.[^1][^2]

## When to Use This

| Trigger | RFC Required? | Format |
|---|---|---|
| Choosing a new external service or infrastructure component | Yes | Full RFC |
| Changing a service's API contract in a backward-incompatible way | Yes | Full RFC |
| Introducing a new architectural pattern across multiple services | Yes | Full RFC |
| Changing an existing pattern within a single service | Maybe | Abbreviated RFC or ADR-only |
| Choosing between two implementation approaches within a ticket | No | PR description is sufficient |
| Documenting a decision already made without significant options analysis | No | ADR directly |

The key threshold question: **would a reasonable engineer on an adjacent team want to know about this decision before implementation?** If yes, an RFC is warranted. If the decision affects only the team making it and has limited surface area for future engineers, document the decision as an ADR without a full RFC process.

---

## Part 1: When an RFC Is Warranted

Beyond the table above, three factors push a decision toward the full RFC process:

**Irreversibility.** Hard-to-undo decisions — schema changes, external API contract commitments, infrastructure migrations — warrant more upfront review than easily-reversible decisions. The asymmetry in the cost of getting these decisions wrong justifies the asymmetry in the process.

**Blast radius.** Decisions affecting multiple teams, multiple services, or customer-facing behavior warrant broader review than decisions contained within a single team's scope. The RFC process forces identification of every stakeholder who should have input before the decision lands.

**Novelty.** Pattern-establishing decisions — the first time the team uses a particular data consistency strategy, the first introduction of a new infrastructure component — warrant documentation of the options considered and the rationale for the choice. Future engineers encountering the same question should be able to find the prior analysis.

---

## Part 2: RFC Lifecycle

### Stage 1: Draft

The author produces a draft RFC using the document skeleton provided below. The draft should support substantive review. A rough sketch is too thin to comment on; a polished single-option document discourages the trade-off discussion the review window exists for.

**Draft criteria:**
- Problem statement is specific and bounded (not "we should improve our database strategy")
- At least two options are presented and evaluated (including "do nothing")
- Author's recommendation is stated with reasoning
- Known risks and open questions are named

The draft is shared with a limited initial audience: the author's manager, the Staff IC if one exists, and one to two engineers from adjacent teams who will be affected. This pre-review surfaces obvious gaps before the RFC is opened to broader comment.

### Stage 2: Open for Comment

The RFC is published to the team or org's designated RFC channel or repository. The review window should be explicit and long enough for async review: a minimum of five business days for team-scoped decisions, ten business days for org-scoped decisions.

**Opening announcement format:**
```
RFC-[number]: [title]
Author: [name]
Review deadline: [date]
Scope: [team / multiple teams / org-wide]
Summary: [one sentence]
Link: [link to document]

I'm looking for feedback specifically on:
- [Question 1]
- [Question 2]
```

During the review window, the author should actively engage with comments: clarifying ambiguities, acknowledging concerns, and updating the document when feedback surfaces gaps. The job calls for engagement; defending the proposal against every objection misreads the format.

### Stage 3: Decision

At the end of the review window, the designated decision-maker (typically the author's manager for team-scoped decisions, a Director or VP for org-scoped decisions) makes a decision. The three outcomes:

- **Approve:** Accept the RFC. Implementation may begin. Record the decision as an ADR.
- **Revise:** The RFC requires significant changes before a decision can land. The author revises and the review window reopens.
- **Decline:** Reject the RFC. The "do nothing" option carries, or a different option from the RFC gets selected. Record the decision as an ADR either way — why not to change something carries as much weight as why to change it.

The decision should be communicated in the same channel where the RFC was published, with a brief rationale. Engineers who commented on an RFC and received no decision communication are likely to comment on future RFCs with lower engagement.

### Stage 4: Decision Record

Convert the RFC's decision into an ADR (Architectural Decision Record). The ADR captures the final decision and its rationale; the RFC document persists as the record of the review process and the options considered.

**Relationship between RFC and ADR:**
- The RFC lives in the RFC archive (a directory or wiki section designated for in-progress and completed RFCs)
- The ADR lives in the codebase or repository it applies to (typically `/docs/adr/` or equivalent)
- The ADR should link back to the RFC for the full context; the RFC should link forward to the ADR for the final decision

---

## Part 3: Review Mechanics

### Who Has Blocking Authority vs. Advisory Voice

Not every comment on an RFC carries the same weight. Making this explicit prevents the RFC process from collapsing into a consensus-seeking exercise that stalls on every objection.

**Blocking authority:** The decision-maker (manager or designated approver) holds the sole blocking authority. A commenter raising concerns cannot block an RFC; the decision-maker can block it by agreeing those concerns remain unresolved.

**Technical blocking signals:** A Staff or Principal IC who identifies a technical flaw in the proposed approach carries a strong signal. Address the concern or acknowledge it explicitly in the decision rationale. Ignoring a Staff IC's technical objection and approving the RFC anyway without explanation damages the RFC process's credibility.

**Advisory voice:** Everyone else. Advisory comments contribute valuable input to the decision; they carry no veto power. The author and decision-maker should engage with advisory comments seriously, but "I disagree with this approach" from an advisory commenter does not block the decision.

### Async vs. Sync Review

Most RFC review should happen asynchronously. The review window gives people room to read, think, and respond on their own schedule; the window does not exist to schedule a meeting. Reserve synchronous discussion for:

- RFCs where the problem space is genuinely complex and a conversation would produce better signal than written comments
- RFCs where the comment thread has reached an impasse that a structured conversation could resolve
- RFCs with significant cross-team implications where a joint session prevents misunderstanding

If a synchronous session is held, document the key points of the conversation and add them to the RFC before the review window closes. Engineers who could not attend should be able to read the RFC and understand the full context of the decision.

### The Decision-Maker's Responsibility

The decision-maker's job is not to find consensus. Natural consensus has value; forced consensus — delaying the decision until objections go away — produces slow organizations and rewards persistent objectors. The decision-maker's job is to:

1. Confirm that the review window was sufficient
2. Confirm that significant objections were addressed or acknowledged
3. Make the decision with the available information
4. Communicate the decision and its rationale

Disagreement after the decision lands is a normal outcome. Engineers who disagree should be able to express their dissent in the ADR's "dissenting views" section, which exists precisely to avoid the false consensus of a document pretending the decision was unanimous.

---

## Part 4: The RFC-to-ADR Relationship

### What Goes in the RFC vs. the ADR

| Content | RFC | ADR |
|---|---|---|
| Problem statement | Full description | Brief reference |
| Options considered | Full analysis of all options | Summary of options, detail on chosen option |
| Review comments | Yes | No (archived in RFC) |
| Author's recommendation | Yes | N/A |
| Final decision | Decision recorded at end | Primary content |
| Consequences | Options for each alternative | Consequences of the decision as made |
| Dissenting views | Comments section | Explicit "dissenting views" section |
| Link to counterpart | Link to ADR (added after decision) | Link to RFC |

### ADR Decision-Record Footer Block

Add this block to the RFC document after the decision is made:

```markdown
---

## Decision Record

**Decision:** [Approve / Decline / Revise]
**Decision-maker:** [Name and title]
**Date:** [Date of decision]
**ADR:** [Link to the ADR document]

**Rationale:** [2–3 sentences summarizing why this decision was made]

**Significant objections considered:**
- [Objection]: [How it was addressed or why it was acknowledged but not determinative]
```

---

## Templates

### RFC Document Skeleton

Write the Summary section last; it should compress the completed document, not introduce it.

```markdown
# RFC-[number]: [Title]

**Author:** [Name]
**Created:** [Date]
**Status:** Draft | In Review | Decided | Superseded
**Review deadline:** [Date]
**Decision-maker:** [Name or role]
**ADR:** [Link — added after decision]

---

## Summary

[One paragraph: the problem, the proposed solution, and why it matters.]

---

## Problem Statement

[What problem are we solving? Be specific. Describe the current state and
why it is insufficient. Include evidence where possible: error rates,
performance data, developer time estimates, incident patterns.]

---

## Goals

- [Specific, testable outcome 1]
- [Specific, testable outcome 2]

## Non-Goals

- [What this RFC is explicitly not trying to solve]
- [Scope boundary]

---

## Options Considered

### Option 1: [Name] (Recommended)

[Description of the option]

**Pros:**
- [Pro]
- [Pro]

**Cons:**
- [Con]
- [Con]

**Risk:** [Primary risk of this option]

### Option 2: [Name]

[Description]

**Pros / Cons / Risk** [same format]

### Option 3: Do Nothing

[What happens if we don't act. This option should always be included.
If "do nothing" is clearly worse than every proposed option, say why.]

---

## Recommendation

[Author's recommended option and 2–3 sentence rationale. Be direct.
"I recommend Option 1 because..." not "Option 1 may be worth considering."]

---

## Open Questions

- [Question that reviewers should specifically address]
- [Dependency or assumption that needs validation]

---

## Consequences

If the recommended option is adopted:
- [What changes]
- [What ongoing cost is introduced]
- [What future flexibility is constrained]

---

## References

- [Link to relevant prior RFC or ADR]
- [Link to external documentation]
```

### Reviewer Guidance Card

```
When reviewing an RFC:

1. Focus on the problem statement first. Is this the right problem to solve?
   Is it bounded enough to be actionable?

2. Evaluate the options analysis. Are the options genuinely distinct? Are the
   trade-offs accurately represented? Is there an important option that's missing?

3. Check the recommendation. Does the rationale follow from the analysis?
   Is the author's preference legible, or does the recommendation appear
   disconnected from the options as written?

4. Name your role in the comment. "I think this is wrong" is less useful than
   "As the team that owns the downstream service, we will need to [change X]
   if this RFC is adopted — is that accounted for in the cost estimate?"

5. If you have a blocking concern, say so explicitly. "This RFC should not
   proceed without resolving [X]." Don't bury a blocking concern in a list
   of advisory suggestions.

6. If you don't have a blocking concern but have a preference, say that too.
   "I'd prefer Option 2, but I can live with Option 1 if [condition]."
```

---

## Anti-Patterns

**The RFC as a Rubber Stamp.**
A process where RFCs get written after the decision has already been made and implemented, used only to create documentation. The RFC loses its value as a review mechanism. Engineers learn their comments on RFCs have no effect and stop engaging.

**The Consensus Trap.**
Delaying decision-making until every objection is resolved. In a technically opinionated team, some objections will never be fully resolved; the decision-maker's job is to acknowledge them and decide anyway. A process requiring consensus to move forward produces paralysis on contested decisions.

**The Scope Creep RFC.**
An RFC starting with a specific technical question and expanding to cover organizational philosophy, process reform, or questions belonging in a different forum. Scope-creep RFCs become long, unfocused, and hard to decide on. Keep the problem statement bounded.

**The Missing "Do Nothing" Option.**
An RFC presenting two implementation approaches without considering whether the change is necessary at all. "Do nothing" is always an option and should always be analyzed. An RFC unable to make the case against "do nothing" may lack a sufficiently compelling problem statement.

**The Undocumented Decision.**
An RFC concluding with a decision communicated verbally or in a Slack thread, without an ADR. The ADR captures the RFC process's value; a decision living only in memory or a Slack archive functions as undocumented, regardless of how thorough the RFC was.

[^1]: The Rust RFC Process. https://github.com/rust-lang/rfcs — the canonical open-source RFC process from which many engineering RFC conventions derive.
[^2]: Larson, W. (2019). *An Elegant Puzzle: Systems of Engineering Management*. Stripe Press. Chapter 4 discusses architectural decision-making and documentation at the org level.
