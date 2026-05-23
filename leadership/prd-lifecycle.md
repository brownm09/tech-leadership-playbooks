# PRD Lifecycle Management

> **Leadership context:** PRDs fail in predictable ways — frozen at kick-off, accumulating stale assumptions, treated as something to route around instead of consult. The PM owns the PRD; the engineering leader sets the standard for how the document evolves, enforces its use as a working artifact, and keeps the team building against current understanding instead of the snapshot from last quarter's planning session.

## Purpose

This playbook defines how a PRD gets structured, how it evolves over the life of a product, and what "done" means at each stage. It applies the continuous discovery model: the PRD tracks evolving product understanding instead of locking scope. One living PRD per product. Git holds the version history; no "v1.3 Final (2)" document needed.

> **Demonstration sandbox:** [lifting-logbook](https://github.com/brownm09/lifting-logbook)
> is a personal-project monorepo, not a production system at scale. The artifacts linked
> in the Further-reading section illustrate the lifecycle described here at proposal-document
> scale; production-scale application of the same model is documented in
> [ORIGINS.md](../ORIGINS.md) where applicable.

---

## The One Living PRD Principle

The frequent PRD failure mode: treating the document as a contract. You write it during discovery, lock it at kick-off, and then run a separate change-control process for every deviation. The consequences:
- Engineers build against stale requirements because PRD updates feel bureaucratic
- PMs maintain two sources of truth (the PRD and the Jira description where the real decisions happen)
- Post-mortems treat "we built what the PRD said" as an absolution

The one living PRD model inverts this. The document always reflects current intent. Version control holds the history. The changelog (see below) communicates what changed and why — a section of the same document, not a separate one.

**The test:** Any engineer joining the team mid-project should be able to read the PRD and get accurate context about what the team is building, why, and for whom. If the document fails this test, it has drifted.

---

## PRD Structure

Every PRD has five required sections. Sections 1–3 are set during discovery and rarely change. Sections 4–5 evolve continuously.

### Section 1 — Problem Statement

One paragraph. What can the user not do, or do only poorly, because this product does not exist? Written as a job-to-be-done: *"When [situation], users want to [goal], but [obstacle] prevents them. The cost of this is [outcome]."*

Avoid solution language in this section. "Users need a dashboard" describes a feature; a problem statement names what the user cannot do today.

### Section 2 — User Job and Outcome Table

| User type | Job to be done | Success looks like | Current workaround |
|-----------|----------------|-------------------|-------------------|
| (e.g.) Product manager | Monitor experiment results across multiple flag variants without switching tools | One view, no manual aggregation, results available within 24h of flag creation | Exporting from LaunchDarkly, joining in a spreadsheet |

Fill one row per distinct user type with a meaningfully different job. Two or three rows is normal. More than four typically signals unclear product scope; genuine user diversity rarely needs more than three.

### Section 3 — Personas

Two or three personas, maximum. Each persona names a specific person; a demographic label fails the format. Use Cooper's persona format: a name, a role, one sentence on what they aim to accomplish in their work, and one sentence on where this product fits in their day.

Personas describe individuals; "data-driven PMs at mid-market SaaS companies" names a market segment, which the persona format deliberately rejects. A persona reads: "Maria, a senior PM who runs 8–10 concurrent experiments and needs to brief the CPO weekly on business impact."

### Section 4 — Hypothesis and Bets

The specific bets this product makes. Written as falsifiable hypotheses: *"We believe [action] will produce [outcome] for [persona]. We will know this worked when [metric] moves by [amount] within [timeframe]."*

Update this section when bets change. When discovery or user research invalidates a bet, strike it through with a one-line explanation and a changelog entry (see Section 5); do not delete it.

### Section 5 — Changelog

Append-only log of substantive changes to this document. Format:

```
## Changelog

### 2026-03-15 — Scope reduction: dropped async notifications
**What changed:** Removed async email notification feature from v1 scope.
**Why:** User interviews (n=8) showed in-app visibility was sufficient for the primary persona. Async adds two weeks; the bet doesn't require it.
**Impact:** Engineers on notification service unblocked for other work. Revisit in v2 if retention data shows re-engagement gap.

### 2026-02-28 — Hypothesis updated: engagement metric changed
**What changed:** Success metric changed from DAU to experiment-start rate.
**Why:** DAU is affected by marketing campaigns we don't control. Experiment-start rate isolates product value.
**Impact:** None to scope; affects how the data team instruments the event.
```

---

## Two-Tier Update Process

Not all changes to the PRD are equal. Use two tiers to reduce change-control friction for minor updates while keeping major changes visible.

### Tier 1 — Minor (update and log, no approval required)

- Clarifications that don't change scope or success criteria
- Updated metrics or targets based on baseline data
- Persona refinements from user interviews
- Removing features deferred to a later version (log the reason; if the cut affects team allocation or delivery commitments, use Tier 2 instead)
- Copy edits and format fixes

**Process:** Edit the document, add a changelog entry, notify the team in the project communication channel.

### Tier 2 — Major (requires PM + EM alignment before update)

- Changes to the core problem statement
- Adding new user types with distinct jobs
- Changing a success metric after development has begun
- Scope additions that affect timeline or team allocation
- Invalidating a hypothesis (the bet was wrong)

**Process:** PM and EM discuss async or sync (depending on urgency), align on the change and its rationale, then update the document together. Add a changelog entry. If the change affects delivery commitments, communicate upstream before updating the document.

---

## Lifecycle Stages

A PRD passes through four stages. What each stage expects:

### Discovery

In Discovery the PRD functions as a working hypothesis. Sections 1–3 are drafts. Section 4 carries at least one hypothesis. Section 5 stays empty.

**Gate to Alignment:** Problem statement is crisp, job-outcome table has ≥1 validated row, at least one persona has been confirmed with a real user, and the PM can articulate the primary bet in one sentence.

### Alignment

Review and socialization happen here. Engineering scopes; design explores. The document gets updated frequently as assumptions surface.

**Gate to Delivery:** PM and EM have signed off on Section 4. Engineering has estimated. Dependencies are identified. No open "TBD" in Sections 1–3.

### Delivery

The PRD serves as a reference document. Minor updates surface as implementation hits edge cases. Major changes go through Tier 2 process.

**In-flight discipline:** When an engineer cannot reconcile a decision with the PRD, the PRD has the wrong content — update it; do not paper over it in the code.

**Gate to Shipped:** Feature ships to production, instrumentation runs live, and the team can observe the success metrics in Section 4.

### Shipped

In Shipped, the document is archived read-only. A brief post-ship retrospective appended to the changelog documents the outcomes: what the metrics showed, whether discovery validated the bets, and what the team learned.

---

## Ownership and Collaboration

**PM owns the PRD.** The PM authors it, decides content, and keeps it current.

**EM partners on accountability.** The EM flags drift from reality, pushes back on hypotheses lacking falsifiability, and ensures engineers can reference the PRD to resolve implementation ambiguity.

**Engineers read and flag.** Any engineer who cannot reconcile a decision with the PRD should flag it to the PM within one working day; routing around it makes the drift worse.

**Design owns the persona depth.** Design research feeds Sections 2 and 3. Personas written without user contact signal a process gap to fix — the PRD itself is not the problem.

---

## Common Failure Modes

**The frozen PRD.** The document captures intent at kick-off and drifts within six weeks. Engineers stop consulting it. Fix: make PRD review a standing agenda item in the weekly team sync — five minutes to ask "does this still hold?"

**The PRD-as-contract.** The PM treats deviation from the PRD as a process violation. Engineers treat "it's in the PRD" as grounds for skipping concerns. Fix: Tier 2 process should take hours, not days. A slow change process costs undocumented drift.

**The solution PRD.** The document describes what will be built and skips why. Section 1 says "Build a real-time dashboard" in place of describing the user's problem. Fix: ban solution language from Sections 1–3. A problem statement that could have been written after the solution was decided was.

**The thousand-page PRD.** The document grows to cover every edge case and specification, duplicating content belonging in design files and tickets. Fix: the PRD answers why and for whom; it does not answer how. Technical specs, API contracts, and edge-case handling belong in linked documents.

**The invisible changelog.** Changes happen but go unlogged. Two months in, no one remembers why the scope changed. Fix: make the changelog append-only and treat entries the same way you treat commit messages — permanent, reasoning-led, written for someone who wasn't in the room.

---

## Background and Motivation

I developed this lifecycle model during the feature lifecycle and roadmapping process revision at Community Tech Alliance (2025–2026). I designed and drove the process changes; the team adopted them. The revision shifted emphasis from scope-locked product specifications to outcome-oriented living documents — a change reducing planning debt and improving IC alignment with the problems the engineering org needed to solve.

---

## Further reading: demonstration artifacts

The artifacts below illustrate the lifecycle stages described in this playbook against the demonstration sandbox introduced after the Purpose section. See [LINKING.md](../LINKING.md) for the full convention. Citation links pin to commit [`413f8a6`](https://github.com/brownm09/lifting-logbook/tree/413f8a62f43f12fa200be3e3307da7ef72c7b446) per the LINKING.md SHA-pinning rule. Where an artifact is intended to evolve as the project does, a `main` link is provided alongside.

### On the lifecycle state machine (`draft → accepted → shipped`)

- **Proposal lifecycle definition** — citation: [`docs/proposals/README.md` at 413f8a6](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/docs/proposals/README.md); live state: [same path on `main`](https://github.com/brownm09/lifting-logbook/blob/main/docs/proposals/README.md). Defines the four-state lifecycle (`draft`, `accepted`, `shipped`, `declined`) used across every proposal in the directory. Demonstrates the playbook's claim that lifecycle stages must be *named* and *gated* — without explicit names, "in progress" silently absorbs both Alignment and Delivery and the gates between them get skipped.
- **Proposal index** — live state: [`docs/proposals/`](https://github.com/brownm09/lifting-logbook/tree/main/docs/proposals). Each filename is dated and slugged; each file declares its current `Status`. The directory itself is the lifecycle dashboard — no separate tracking system, no "v1.3 Final (2)" naming pathology. This is the playbook's "version history is in git" principle applied concretely.

### On the worked end-to-end example

- **On-call readiness proposal** — citation: [`docs/proposals/2026-05-08-on-call-readiness.md` at 413f8a6](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/docs/proposals/2026-05-08-on-call-readiness.md); live state: [same path on `main`](https://github.com/brownm09/lifting-logbook/blob/main/docs/proposals/2026-05-08-on-call-readiness.md). Walks the full lifecycle in one document: Problem → Proposed Solution → Acceptance Criteria → Out of Scope → linked GitHub issue, with the status field updated as the work progresses. The "Milestone fit note" paragraph is the part most worth reading: it explicitly records *why* the work was placed in the milestone it was, anticipating the kind of question that gets asked six months later when no one remembers the original framing.

### On ADRs as the append-only changelog companion

- **ADR series** — live state: [`docs/adr/`](https://github.com/brownm09/lifting-logbook/tree/main/docs/adr). The proposal directory captures *what to build*; the ADR directory captures *why we built it the way we did*. Together they implement the playbook's separation of concerns: PRD/proposal answers "why and for whom"; ADRs answer architectural "how and at what trade-off." Notable: [ADR-019: SLO Methodology](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/docs/adr/ADR-019-slo-methodology.md) was written as part of the on-call-readiness proposal's acceptance criteria — the proposal explicitly required the methodology decision to be captured as an ADR before the work was considered shipped.

### On the `/propose` skill (process automation)

- **Proposal scaffolding configuration** — live state: [`.claude/propose.json`](https://github.com/brownm09/lifting-logbook/blob/main/.claude/propose.json). Encodes the proposal directory, the PRD location, the active milestones, and the epic taxonomy as machine-readable configuration. The `/propose` skill reads this to generate a proposal stub, file the linked GitHub issue with the right milestone and epic assignment, append a roadmap entry, and open a PR — automating the per-proposal mechanical work so the document author can focus on Sections 1–4. Demonstrates the playbook's principle that lifecycle discipline is best preserved by removing friction from following it.

### What this sandbox does *not* demonstrate

- **Personas and the job-outcome table** are minimally represented — the project has one user (the author), so Section 2 and Section 3 of the playbook collapse. The lifecycle structure transfers; the user-research discipline animating Sections 1–3 in a real product context does not transfer here.
- **Tier 1 / Tier 2 update process** does not appear, because the same person is PM and EM. The state-machine and changelog discipline transfers; the alignment process does not transfer.

---

## References

- [Clayton Christensen, Taddy Hall, Karen Dillon, and David Duncan — "Know Your Customers' 'Jobs to Be Done'" (*Harvard Business Review*, September 2016)](https://hbr.org/2016/09/know-your-customers-jobs-to-be-done) — The canonical HBR treatment of the Jobs to Be Done framework. Establishes that customers hire products to accomplish specific outcomes; the feature inventory is incidental. The job-outcome table format in §2 (job + "success looks like") is a direct application.
- [Alan Cooper — *The Inmates Are Running the Asylum* (Sams, 1998)](https://www.amazon.com/Inmates-Are-Running-Asylum-Products/dp/0672326140) — Origin of Goal-Directed Design and user personas as a product design tool. The guidance to keep personas to two or three reflects Cooper's observation: more personas typically signal unclear product scope; genuine user diversity rarely requires more than three.
- [Marty Cagan — *Inspired: How to Create Tech Products Customers Love*, 2nd ed. (Wiley, 2018)](https://www.svpg.com/books/inspired-how-to-create-tech-products-customers-love-2nd-edition/) — Establishes continuous discovery and outcome-oriented product thinking. The "one living PRD" design and the rejection of per-version document freezes aligns with Cagan's product-team model, where the document tracks evolving product understanding instead of locking scope.
