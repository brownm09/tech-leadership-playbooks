# Difficult Conversations Framework

## Leadership Context

A manager who avoids difficult conversations is not being kind; the avoidance is a form of negligence. The engineer who is underperforming and never told clearly is not given the opportunity to improve. The team watching the performance problem go unaddressed concludes the manager will not hold standards. The high performer who shares a team with someone carrying a performance gap experiences that gap as an unfairness, and eventually leaves. The avoidance that felt like compassion produces bad outcomes for everyone involved.

This playbook covers the mechanics of four recurring difficult conversations in engineering management: underperformance, role mismatch, team conflict, and leveling disagreements. It does not cover HR process — jurisdiction-specific and owned by your People team. It covers the conversation that has to happen before the process does.

## Background and Motivation

The direct context for this framework is calibration and career development work at ActBlue Technical Services (2022–2025): promotions for three engineers over three years (two of whom were promoted twice each, SE2 to SSE2, and one promoted from SSE to SSE2), including two where the conversation required sustained advocacy against calibration pressure, and one where the conversation required honesty about a leveling dispute the engineer did not initially agree with. The latter involved naming clearly, and with evidence, the gap between the engineer's self-assessment and the calibration panel's read. That conversation was uncomfortable and necessary; deferring the feedback to avoid the discomfort would have produced a delayed and more damaging version of the same conversation at the next cycle.

---

## Part 1: Before the Conversation

### Three Things to Clarify First

Every difficult conversation should have answers to three questions before you schedule it:

1. **What specific behavior or situation are you addressing?** Not the label ("your performance is declining") — the evidence. "In the last three sprints, two commitments were missed without early warning, and the code review feedback I gave in [specific PR] has not been reflected in the subsequent work." The more specific the evidence, the less room for misinterpretation and the less room for a defensive pivot to "that's just your opinion."

2. **What do you want to be different after this conversation?** A conversation without a change goal is a complaint, not a feedback session. The goal might be a behavior change ("I want you to surface blockers by Thursday of each sprint week, not in the Monday retrospective"), a role adjustment, or a shared acknowledgment that a problem exists. Name it before you enter the room.

3. **What does the person in this conversation need from you?** Not agreement — but to feel heard and to understand what the path forward looks like. A conversation that leaves the engineer confused about what happens next, or what success looks like, has not landed.

### SBI: Situation, Behavior, Impact

Use SBI as the structure for delivering the core observation:

- **Situation:** When and where? Specific enough to be incontestable.
- **Behavior:** What was observed? Behavior only — not intent, not inference, not character.
- **Impact:** What was the effect on the team, the system, or the relationship?

Example: "In the all-hands on Thursday [situation], you responded to [product manager]'s question about the timeline by saying 'that's not how software works' and ending the conversation [behavior]. She came to me afterward and said she felt dismissed and was not sure the team was invested in the launch [impact]."

The SBI structure makes it harder to argue with the observation and easier to focus the conversation on the impact and the path forward.

SBI is also the feedback model used in regular 1:1s — see the [1:1 Coaching Framework](one-on-one-coaching-framework.md) for how it integrates into the weekly feedback cadence beyond high-stakes conversations.

### When Not to Have the Conversation

**Do not have a difficult conversation:**
- In the heat of the moment (your own or theirs). If you are activated, wait.
- In public or in a group setting. Feedback that involves performance or behavior belongs in a private 1:1.
- When you do not have enough evidence. One incident is an observation; a pattern is a conversation. If you have one data point, name it as an observation in the next 1:1, not as a performance issue.
- When the timing creates an unfair context (end of a high-stress sprint, day of an incident, during a personal crisis you know about). The conversation will not land and you will need to have it again.

---

## Part 2: Conversation Types

### Underperformance

**The two modes:** Early-signal underperformance is handled in 1:1s, directly and informally, before it becomes a formal situation. Formal underperformance (a Performance Improvement Plan or equivalent) is a last resort with HR involvement, a documented plan, and a defined evaluation window. The conversation in this playbook covers early-signal — the moment you recognize a problem and before it becomes formal.

**The structure:**

1. **Name the pattern clearly.** "I want to talk about something I have been seeing over the last [timeframe]. [SBI description of the pattern.]"
2. **Check your read.** "Is there something going on that I don't have context on?" This is not a rhetorical question. There are legitimate reasons for performance dips — burnout, personal circumstances, unclear expectations — and understanding the cause changes the conversation.
3. **Name the stakes.** "If this continues through [next cycle/sprint/quarter], it will affect your standing in calibration. I do not want that outcome — that is why we are having this conversation now." Be direct about the stakes. Engineers who find out at calibration that a performance problem was visible for months without being named to them have a legitimate grievance.
4. **Agree on next steps.** Specific, measurable, time-bounded. "By [date], I want to see [specific behavior change]." Write it down. The shared 1:1 doc is the right place.
5. **Follow up at the agreed interval.** Not at the performance cycle — at the timeframe you named. Feedback that is not followed up on is not taken seriously.

**Common mistakes:**
- Softening the message so much that the engineer does not understand there is a problem ("I just wanted to check in on how things are going")
- Naming the problem but not the stakes ("just something to think about")
- Having the conversation once and considering it handled

### Role Mismatch

Role mismatch is distinct from underperformance: the engineer may be doing their current job adequately, but the job has changed (or is changing) in a direction that does not match their skills, interests, or trajectory. The conversation is about fit, not performance.

Common scenarios:
- A team's technical complexity has grown beyond what an engineer can follow with their current skill level
- A role that started as individual contribution has shifted to require significant cross-team coordination the engineer is not suited for
- An engineer who was hired for one kind of work is consistently gravitating toward different work, suggesting a genuine mismatch of the role to the person

**The structure:**

1. **Name the gap without blame.** "I want to talk about the direction this role is evolving and whether it is the right fit for where you want to go." This framing puts the conversation on trajectory, not fault.
2. **Share your read.** "What I am seeing is [specific observation about the gap]. I want to understand whether that matches your experience."
3. **Explore options.** A role mismatch does not have one resolution. Options include: adjusting the role to better fit the person (if the work allows it), finding a different internal role, and an honest conversation about whether the org is the right fit at all. The engineer should participate in identifying the path.
4. **Do not force the conclusion.** This conversation rarely resolves in one meeting. Give the engineer time to process and come back with their read.

### Team Conflict

Team conflict between two engineers, or between an engineer and another team, ranks among the harder conversations because it often involves competing valid perspectives. The manager's job is to restore the conditions for collaborative work; adjudicating who is right is not the goal.

**The structure for a peer conflict:**

1. **Have separate conversations first.** Talk to each party individually before bringing them together. Understand each person's experience of the conflict before trying to resolve it. Do not share what one person said with the other in a way that escalates the conflict.
2. **Name the behavior; the person is not the target.** "I have heard [specific behavior that is creating friction]" instead of "I have heard you are difficult to work with."
3. **Name the impact on the team.** "The current situation is affecting [sprint velocity / code review culture / the team's ability to collaborate on X]. That is the problem I need to solve."
4. **Bring the parties together with a specific agenda.** Not "let's talk about the conflict" — "I want to agree on how we are going to handle [specific recurring situation] going forward." The conversation should produce a behavioral agreement, not a feelings resolution.

**What managers get wrong in team conflict:**
- Picking a side. Even if one person is clearly more at fault, declaring a winner creates a permanent alliance and opposition within the team.
- Waiting too long. A team conflict that is visible to the rest of the team and is not being addressed will be interpreted as management tolerance of the behavior.
- Treating the conversation as the end. A behavioral agreement reached in one conversation needs follow-through — check in on whether the agreement is holding.

### Leveling Disagreements / Calibration

The calibration session structure, level definition format, and failure modes (grade inflation, recency bias, proximity bias) are covered in [career-ladder-calibration.md](career-ladder-calibration.md). This section covers the conversation practice that operates within that process.

Leveling disagreements are difficult because they combine two elements that rarely go well together: a consequential decision (compensation, title, promotion) and a subjective assessment that the person being assessed almost always sees differently than the assessor.

**Before calibration:**
The leveling conversation that lands is the one that happens before calibration; conversations after the decision rarely change anything. If an engineer believes they are at the next level and calibration is likely to disagree, have the conversation in advance:

1. **Share your read, with evidence.** "I want to give you my honest read on where I think calibration will land this cycle, and why." Then share the specific evidence for why you believe the next level has or has not been demonstrated.
2. **Give the engineer's case a hearing.** "Walk me through how you see it." Listen — not to be persuaded against evidence, but to understand whether there is evidence you have missed.
3. **Name the gap specifically.** If the disagreement is real, be direct: "The gap I see is [specific]. Here is what addressing it would look like. I think it is closable, and I want to work with you on it."

**After calibration:**
If calibration produces an outcome the engineer disagrees with, the conversation requires the manager to hold two things at once: advocacy for the engineer (you can share what you argued for) and honesty about the calibration outcome (you cannot pretend the decision was wrong when you will need the engineer to trust the process next cycle).

Do not:
- Promise a different result next cycle without evidence that the gap will close
- Attribute the outcome to external factors (the panel, the process) in a way that exonerates you from the conversation
- Avoid the conversation and let the engineer find out from the announcement

---

## Part 3: Follow-Through

### The Check-In Cadence

A difficult conversation that is not followed up on is worse than no conversation. It signals that the named expectation was not real. The follow-up schedule:

- **Underperformance:** at the specific timeframe named in the conversation (two weeks, one sprint, one month)
- **Role mismatch:** at four weeks, to see how the engineer has processed the conversation and whether they have a path in mind
- **Team conflict:** one week after the behavioral agreement, then monthly for a quarter
- **Leveling disagreements:** at the next significant milestone (mid-cycle, pre-calibration)

### Documentation Discipline

Document the conversation in the 1:1 shared doc — not in exhaustive detail, but enough to create a record of what was said and what was agreed. For underperformance situations: record the specific behavioral expectation and the timeline. This documentation protects both parties: it gives the engineer a clear record of what was asked of them, and it gives the manager a record of the conversation if formal process becomes necessary later.

Do not document in a way that feels punitive or that the engineer cannot see. If something goes in the 1:1 doc, assume the engineer will read it — because they should.

### When to Escalate to HR

Escalate to HR when:
- A formal PIP or equivalent process is warranted (persistent underperformance that has not responded to coaching)
- The conversation involves a complaint of discrimination, harassment, or hostile work environment
- You have a conflict of interest in the situation (a relationship, a personal history, or a financial stake)
- The outcome of the conversation may involve a role change, demotion, or separation

HR escalation is not a failure; it is the right level of support for decisions with significant consequence. The mistake is escalating too late — after the situation has become a crisis — instead of involving HR early as a thought partner.

---

## Anti-Patterns

**The Compliment Sandwich.** Positive feedback → difficult feedback → positive feedback. The difficult feedback gets lost. Name it directly: "I want to talk about something specific."

**The Group Feedback.** Addressing individual behavior in a team setting to avoid a direct conversation. This is public humiliation with plausible deniability.

**The Proxy Conversation.** Having the conversation through someone else — another manager, HR, a trusted colleague. The person who owns the relationship owns the conversation.

**The One-And-Done.** Having the conversation once and considering the matter closed. Behavior change takes time and follow-through. A single conversation without follow-up produces at best a temporary adjustment and at worst the impression that you were not serious.

**The Future-Tense Softening.** "You might want to think about..." or "it could be worth considering..." These phrasings are not feedback. They are suggestions the engineer can safely ignore, because they carry no observable expectation.
