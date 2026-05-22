# AI Adoption Readiness Framework (AARF)

## Leadership Context

AI tooling adoption carries measurable ROI and a risk surface if left ungoverned. At ActBlue and CTA, the question was not whether to adopt AI tools — the decision had been made — but how to structure the rollout to capture productivity gains, prevent quality regression, and keep the org from depending on tools no one fully understands. This framework treats adoption as a phased program with gates.

> Practitioner's guide to rolling out AI-assisted development tools in engineering organizations: accelerate leverage, prevent engineers from outsourcing judgment to the tool.

**Author:** Mike Brown  
**Version:** 1.0  
**Status:** Draft / Internal Reference  
**Applies to:** GitHub Copilot, Claude, Claude Code, Cursor, and similar AI-assisted development environments

---

## Background and Motivation

The framework emerged from two successive AI rollouts:

1. **ActBlue Technical Services (2024–2025):** Piloted GitHub Copilot and Claude across ICs and engineering managers. Observability was instrumented into the rollout from day one to track adoption patterns.

2. **Community Tech Alliance (2025–2026):** Designed the Claude Code and Claude integration initiative — Claude Code for engineering workflows, Claude for non-engineering workflows, sequenced in stages to limit organizational risk.

The failure mode both rollouts were designed to guard against: engineers outsourcing judgment to AI. The framework gives managers a shared vocabulary for coaching against it.

---

## Design Principles

Three tensions hold the framework together:

| Tension | Resolution |
|---|---|
| **Leverage vs. dependency** | AI surfaces options and reduces toil; engineers remain accountable for every line they ship. |
| **Speed vs. risk** | The same capability accelerating a junior engineer can introduce subtle errors in high-stakes contexts. Task classification resolves the conflict; blanket policy does not. |
| **Individual fluency vs. org consistency** | Adoption spreads unevenly. The rubric creates a shared coaching vocabulary across the variance, instead of pretending uniformity. |

---

## Part 1: Task Classification

Before deploying AI tooling, publish a shared taxonomy of where AI creates leverage and where it introduces unacceptable risk. The taxonomy becomes the policy reference for ICs and reviewers.

> **Design pattern:** Policy-as-code makes implicit norms explicit and version-controlled. See: [NIST AI RMF 1.0, Govern function](https://airc.nist.gov/Home) for the governance framing; [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/) for risk classification in AI-assisted software development.

| Task Type | AI Leverage | Risk Level | Policy |
|---|---|---|---|
| Boilerplate / scaffolding | High | Low | Encourage; no special review |
| Test generation | High | Low–Med | Encourage; IC reviews all generated assertions |
| Code review assistance | Medium | Medium | Treat as one input; human renders verdict |
| PR summaries / documentation | High | Low | Encourage |
| Debugging production issues | Medium | High | Human-led; AI in a supporting role |
| Compliance-adjacent logic (PCI-DSS, FEC, HIPAA) | Low | Very High | Require senior review of all AI-assisted output |
| Architectural decisions | Low | High | AI generates options; human decides and documents rationale |
| Performance reviews / people decisions | None | Extreme | **Prohibited** |

> **Note on compliance-adjacent tasks:** In regulated environments, AI-generated logic touching payment flows, authentication, data residency, or audit trails requires explicit sign-off. The risk: subtle error surface, high blast radius. Mirrors the defense-in-depth principle from PCI-DSS v4.0 requirement 12.3 (targeted risk analysis).

---

## Part 2: Fluency Rubric

Four stages of AI fluency. ICs self-assess; managers calibrate in 1:1s. The rubric opens a coaching conversation and gives both parties a shared frame for discussing where an engineer sits and what growth looks like.

---

### Stage 1 — Naive User

**Behavior:** Generates output and accepts it without critical evaluation, commits AI-written code without close reading, and treats AI output as authoritative.

**Observable signals:**
- Difficulty explaining AI-generated changes when questioned in code review
- Frequent silent failures (tests pass, behavior is wrong)
- Copy-paste patterns from AI output without adaptation to the codebase

**Coaching target:** Establish the habit of reading and understanding every line before shipping. Use the prompt to start thinking; finish it yourself.

**Coaching question for 1:1s:**
> "Walk me through the last thing you used AI to help with. What did you change before committing it?"

---

### Stage 2 — Skeptical Integrator

**Behavior:** Treats AI output as a draft requiring validation. Identifies model errors. Refines prompts based on output quality.

**Observable signals:**
- Catches hallucinated API signatures or incorrect library usage before they reach review
- Asks better follow-up questions of the tool
- Adds domain context to prompts (relevant file structure, constraints, examples)

**Coaching target:** Move from reactive validation to proactive prompt design. Begin developing a personal library of effective prompts for recurring task types.

**Coaching question for 1:1s:**
> "What context did you give the model to get a useful result? What would you do differently next time?"

---

### Stage 3 — Deliberate Practitioner

**Behavior:** Designs prompts for specific outcomes. Has internalized which task types benefit from AI and which do not. Uses AI earlier in the task lifecycle: design and planning, then implementation.

**Observable signals:**
- Fewer revision cycles; output closer to intent on first pass
- Rejects AI output in appropriate high-risk contexts without needing coaching
- Uses AI to clarify requirements and surface edge cases, then to write code

**Coaching target:** Begin teaching others. Document effective patterns. Identify edge cases where AI proves unreliable in your specific domain.

**Coaching question for 1:1s:**
> "Have you run into a situation where AI misled you? What did you catch it on, and how?"

---

### Stage 4 — Workflow Architect

**Behavior:** Has internalized AI as a design variable in how they structure work. Contributes to team-level patterns. Articulates the failure modes of AI-assisted workflows in their domain and has built mitigations into how they work.

**Observable signals:**
- Proposes process-level workflow changes incorporating AI, beyond task-level uses
- Spots org-level risks before they become incidents (identifies a class of code review needing new norms given AI-generated PRs)
- Mentors Stage 1–2 engineers on specific, observable behaviors

**Coaching target:** Partner with engineering leadership on policy. Contribute to cross-team standards. Reduce single-point-of-failure risk in AI-assisted workflows.

**Coaching question for 1:1s:**
> "If I asked you to defend every line of that PR in a postmortem — including which parts the AI generated and why you accepted them — could you?"

> **The "outsourcing judgment" test.** A Stage 1 engineer cannot answer the question. A Stage 4 engineer has already asked it of themselves before pushing.

---

## Part 3: Role-Differentiated Use Cases

AI tooling creates different kinds of leverage for ICs and for managers. Conflating the two leads to either underutilization or misapplication.

### Individual Contributors

| Use Case | Recommended Tools | Notes |
|---|---|---|
| Code generation & scaffolding | Claude Code, Copilot, Cursor | Primary leverage; apply fluency rubric |
| Test generation | Claude Code, Copilot | Review assertions carefully; AI tends to test the happy path |
| Debugging | Claude, Copilot chat | Useful for hypothesis generation; engineer validates |
| Documentation | Claude, Copilot | High leverage, low risk; good entry point for Stage 1 engineers |
| PR summaries | Claude, Copilot | Encourage adoption; review for accuracy |
| ADR / design doc drafting | Claude | Good for structuring thinking; engineer owns the decision |

### Engineering Managers

| Use Case | Recommended Tools | Notes |
|---|---|---|
| 1:1 prep and note synthesis | Claude | Useful for pattern-finding across multiple 1:1 notes |
| Job description drafting | Claude | Manager reviews and owns final version |
| Roadmap communication | Claude | Drafts only; manager validates alignment to context |
| Performance review drafting | Claude | **Use with caution.** AI drafts only; manager writes final evaluation independently |
| Incident summary drafting | Claude | Useful for structure; manager reviews accuracy and tone |
| Candidate evaluation | **Prohibited** | People decisions require human judgment; AI introduces bias amplification risk |

> **Compliance note:** Every AI-assisted manager output involving specific individuals (performance reviews, PIPs, compensation recommendations) requires manager review and rewrite before submission. AI-assisted drafts cannot ship as final without material revision.

---

## Part 4: Observability

The rubric becomes actionable when paired with signals letting managers and leaders see what is happening. Instrument at three levels.

> **Design pattern:** Mirrors the three-tier observability model (logs → metrics → traces) from SRE practice. See: [Google SRE Book, Chapter 6](https://sre.google/sre-book/monitoring-distributed-systems/) for the underlying observability framework applied here to human+AI systems.

### Individual Level (opt-in)

Lightweight self-tracking via a standing 1:1 reflection prompt:

```
AI Reflection (weekly, 2 minutes):
- What did I use AI for this week?
- Where did it help? (specific task)
- Where did it mislead or slow me down?
- What would I do differently?
```

The reflection builds metacognitive fluency over time and surfaces coaching opportunities.

### Team Level (manager-owned)

Track directional signals over vanity metrics: identify outliers and trends; avoid surveilling individuals.

| Signal | Source | Cadence | What It Tells You |
|---|---|---|---|
| AI-assisted PR rate | PR labels / commit metadata | Weekly | Proxy for adoption; look for unusual spikes or gaps |
| Review comment patterns | Code review tooling | Bi-weekly | Are reviewers flagging AI artifacts? Hallucinated APIs? |
| Incident attribution | Incident postmortems | Per incident | Was AI-assisted code involved? What was the failure mode? |
| Adoption sentiment | Pulse survey (2 questions) | Monthly | Qualitative signal: is AI creating leverage or friction? |

**Sample pulse survey (2 questions, monthly):**
1. *In the past month, AI tools made my work: [much harder / somewhat harder / no change / somewhat easier / much easier]*
2. *The biggest barrier to using AI tools effectively for me right now is: [open text, optional]*

### Organization Level (engineering leadership)

Quarterly review covering:

- Adoption distribution by role tier (staff vs. mid-level vs. junior — meaningful differences?)
- Cross-team variance (which teams are outliers, and why?)
- Policy compliance signals (are prohibited use cases surfacing in postmortems or reviews?)
- Incident attribution trends (is the AI-assisted incident rate trending in any direction?)
- Rubric distribution (aggregate self-assessments: where does the org sit on the fluency curve?)

---

## Part 5: Phased Rollout Gate Model

Expansion to higher-risk workflows gates on demonstrated fluency at the current stage. The sequencing prevents organizations from running ahead of their actual capability.

> **Design pattern:** Applies the *strangler fig pattern* to organizational change — replacing existing workflows incrementally, validating each phase before expanding scope. See: [Martin Fowler, "Strangler Fig Application"](https://martinfowler.com/bliki/StranglerFigApplication.html) for the pattern; the principle transfers directly to phased AI adoption.

```
Gate 0 → Gate 1
  Condition: Tooling deployed; task classification published; team briefed
  Allowed: Low-risk, low-stakes tasks only (boilerplate, docs, test scaffolding)
  Duration: 4–6 weeks observation
  
Gate 1 → Gate 2
  Condition: Team avg fluency at Stage 2 (manager calibration)
              No AI-attributable incidents in Gate 1 period
  Allowed: Medium-risk tasks with light senior review requirements
  Adds: Team-level observability instrumented (PR labels, pulse survey)
  
Gate 2 → Gate 3
  Condition: No AI-attributable incidents in prior quarter
              Org-level observability dashboard active
              At least 2 Stage 4 engineers on team (or 1 staff engineer calibrated at Stage 4)
  Allowed: Agentic workflows with write access (Claude Code in agentic mode,
            automated PR generation, multi-step tool-use loops)
  Requires: Explicit senior review for any agentic output touching compliance-adjacent code
  
Gate 3 (sustained)
  Compliance-adjacent and architectural AI use reviewed quarterly
  Policy document revisited annually or after any AI-related incident
  Rubric calibration done as part of annual performance cycle
```

### Rollback Triggers

Any of the following automatically reverts a team to the previous gate pending review:

- An incident where AI-assisted code was a contributing factor
- A pattern of rubric regression (team avg fluency drops a stage)
- Discovery of prohibited use cases in team workflows
- Material change in the AI tool's behavior (model update, new capabilities with new risk surface)

---

## Appendix A: Worked Example

**Scenario:** A mid-level engineer (Stage 2) is working on a new payment webhook handler. They use Claude Code to scaffold the initial implementation.

**Before commit, the "outsourcing judgment" test:**

> *"If I had to defend every line of this in a postmortem, could I? Which parts did AI generate, and why did I accept them?"*

**What a Stage 2 engineer does:**
- Reviews the full generated output line by line ✓
- Catches the generated HMAC signature verification using a timing-unsafe string comparison ✓
- Fixes the comparison before review ✓
- Notes the fix in the PR description: *"AI-generated skeleton; replaced signature verification with `hmac.compare_digest()` per timing-safe comparison requirements"* ✓

**What a Stage 1 engineer does:**
- Commits the generated output after a quick read ✗
- The timing-unsafe comparison ships to review
- A reviewer catches the issue (best case) or it ships (worst case)

**The coaching intervention:** Stage 2 engineers have internalized the expectation that AI output requires validation proportional to task risk. Stage 1 engineers have not. The rubric makes the expectation explicit so the manager can coach to it.

---

## Appendix B: Reference Reading

| Topic | Resource |
|---|---|
| AI risk governance | [NIST AI Risk Management Framework 1.0](https://airc.nist.gov/Home) |
| LLM-specific security risks | [OWASP Top 10 for LLMs](https://owasp.org/www-project-top-10-for-large-language-model-applications/) |
| Observability principles | [Google SRE Book — Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/) |
| Phased adoption pattern | [Martin Fowler — Strangler Fig Application](https://martinfowler.com/bliki/StranglerFigApplication.html) |
| Prompt engineering standards | [Anthropic Prompt Engineering Overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) |
| Agentic system safety | [Anthropic — Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) |

---

## Appendix C: Demonstration artifacts

> **Demonstration sandbox:** [lifting-logbook](https://github.com/brownm09/lifting-logbook) runs as a personal-project monorepo at single-operator scale. The artifacts below illustrate the framework's mechanisms against an inspectable stack; they do not substitute for the production rollout experience documented in [ORIGINS.md](../ORIGINS.md). See [LINKING.md](../LINKING.md) for the full convention.

The personal-project rollout inverts the org-scale rollouts described above: a single operator, no calibration cohort, no observability dashboard. The framework still applies. The gate model collapses to "self-gate against the rubric"; the observability layer collapses to the artifacts a future reader could audit. Citation links pin to commit [`413f8a6`](https://github.com/brownm09/lifting-logbook/tree/413f8a62f43f12fa200be3e3307da7ef72c7b446); live-state links to `main`.

### On policy-as-code and project-level AI configuration

- **Project AI scaffolding** — [`CLAUDE.md`](https://github.com/brownm09/lifting-logbook/blob/main/CLAUDE.md) (live state). Project-local Claude Code configuration: platform constraints (Windows + Git Bash, no `jq`, npm workspaces), repo layout, and the testing command the global "Test before PR" rule depends on. Demonstrates the framework's Part 4 instrumentation principle at single-operator scale; every project carries the context an AI agent needs in version control where it remains durably retrievable.
- **Project automation hooks** — [`.claude/hook-config.json`](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/.claude/hook-config.json) (citation, SHA-pinned) and [`.claude/propose.json`](https://github.com/brownm09/lifting-logbook/blob/413f8a62f43f12fa200be3e3307da7ef72c7b446/.claude/propose.json) (citation, SHA-pinned). Wire the project to its GitHub Project (epics + milestones) and to the proposal-scaffolding skill drafting ADRs and design proposals against the repo's roadmap. Demonstrates the Gate 2 → Gate 3 prerequisite: agentic write workflows only become safe when the destination structure is itself codified.

### On the gate model and rollback discipline

- **Architecture decision record series** — [`docs/adr/`](https://github.com/brownm09/lifting-logbook/tree/main/docs/adr) (live state). Every architectural decision the AI participated in scaffolding lands as an ADR with an alternatives-considered section. Demonstrates the framework's Stage 4 "outsourcing judgment" test at the artifact level; the rationale for each AI-assisted decision is durably recorded and re-readable, never lost to chat history.
- **Proposal lifecycle** — [`docs/proposals/`](https://github.com/brownm09/lifting-logbook/tree/main/docs/proposals) (live state). Each proposal is drafted with AI assistance, marked through `proposed → accepted → shipped` states, and linked from the implementing PR. Demonstrates the Gate 1 → Gate 2 prerequisite: medium-risk AI-assisted work requires a paper trail, even on a single-operator project.

**Global-layer counterpart — [`dev-env`](https://github.com/brownm09/dev-env) (live state).** Where lifting-logbook demonstrates project-level AI scaffolding, dev-env demonstrates the global layer: cross-device `CLAUDE.md` and `settings.json` symlinked from version control, pre-push and session-boundary hooks enforcing the workflow rules documented in Part 5, custom skills (`/propose`, `/review`, `/journal-compose`, `/research`) extending the agent's capabilities across every project, and autonomous scheduled routines (nightly journal composition, stale-worktree pruning). The Part 2 Stage 4 "Workflow Architect" pattern in practice: the operator has stopped configuring AI tools and started building infrastructure *for* AI tools. The signal a reader should take: the framework's Gate 2 → Gate 3 transition (agentic write workflows gated on codified destination structure) has a global equivalent, beyond the per-project version.

### What is missing — and what the absence demonstrates

The personal-scale stack has no team-level pulse survey, no cross-team variance dashboard, no calibration cohort. The absence is itself a demonstration: the framework's org-level mechanisms (Part 4 *Organization Level*, Part 5 *Gate 3 sustained*) cohere only at organizational scale. A reader evaluating whether to apply this framework should treat the lifting-logbook artifacts as evidence of the per-engineer mechanics, and the [ORIGINS.md](../ORIGINS.md) entries (ActBlue, CTA) as the basis for the org-level claims.

---

## Changelog

| Version | Date | Notes |
|---|---|---|
| 1.0 | 2026-04 | Initial draft; based on ActBlue (2024) and CTA (2025–2026) rollout experience |

---

*Living reference. Open a PR with proposed changes; all policy updates require engineering leadership sign-off before merging.*
