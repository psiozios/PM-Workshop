---
name: pre-mortem
description: Run a structured pre-mortem session before a launch, major decision, or project kickoff. Surfaces failure modes before they happen by imagining failure, mapping root causes, and building mitigations into the plan.
disable-model-invocation: false
user-invocable: true
---

# /pre-mortem - Surface Risks Before They Happen

A pre-mortem is the most underused tool in a PM's toolkit. Instead of asking "what could go wrong?" (which produces vague answers), it asks "this failed spectacularly — what happened?" Thinking in past tense unlocks honest, specific failure modes people are reluctant to name in optimistic forward-looking planning sessions.

## Quick Start

```
/pre-mortem

Running a pre-mortem before a launch or major decision.

Tell me:
1. What are we doing the pre-mortem on? (launch name, feature, decision, or initiative)
2. What's the timeline? (launch date or decision date)
3. Are you running this solo or with a team?

I'll check your existing context (PRD, launch plan, meeting notes) for known risks, then guide the pre-mortem.

Output: outputs/pre-mortems/pre-mortem-[initiative]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

**Automatic Context Checks:**
When this skill is invoked, immediately check:

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| PRD | `context-library/prds/*.md` | initiative name, risks, assumptions, dependencies | Known risks and assumptions already documented |
| Launch Plans | `context-library/launches/*.md` | initiative name, go-live, rollout plan | Timeline, scope, critical path |
| Meeting Notes | `context-library/meetings/*.md` | initiative name, "risk", "blocker", "concern", "worried" | Risks already surfaced in conversations |
| Decisions | `context-library/decisions/*.md` | initiative name | Previous decisions that could have downstream effects |
| Strategy | `context-library/strategy/*.md` | initiative name, strategic context | Strategic assumptions underpinning the initiative |

**Context Priority:**
1. PRD risks and assumptions section FIRST
2. Launch plan critical path SECOND
3. Meeting notes with surfaced concerns THIRD
4. Strategic assumptions FOURTH

**Cross-Skill Links:**
- After pre-mortem → Link to `/launch-checklist` to embed mitigations in launch plan
- If major decision at stake → Link to `/decision-doc` to document go/no-go rationale
- If experiment involved → Link to `/experiment-decision` for kill criteria
- For executive communication of risks → Link to `/status-update` or `/board-deck`

---

## Step 0: What We Already Know About This Initiative

Before surfacing new risks, let me check what's already documented...

**Checking:**
- `context-library/prds/` for known risks and assumptions
- `context-library/launches/` for launch plan and dependencies
- `context-library/meetings/` for concerns raised in planning conversations

**Based on what I find, I'll show you:**

### Known Risk Inventory

**From PRD:**
- [Risks already documented in the PRD, with current mitigation status]

**From Launch Plan:**
- [Critical path dependencies, go-live constraints]

**From Meeting Notes:**
- [Concerns or blockers raised by team members or stakeholders]

**Gaps:**
- [Areas not yet risk-assessed]

---

## When to Run a Pre-Mortem

Run a pre-mortem when:
- You're 2-4 weeks before a major launch
- You're making a significant resource or strategic decision
- The initiative has dependencies across multiple teams
- There's significant uncertainty in one or more key assumptions
- The cost of failure is high (revenue impact, user trust, regulatory)
- The team seems overconfident or has stopped raising concerns

Skip or shorten when:
- This is a small bug fix with limited scope
- You already ran one recently and the risk picture hasn't changed
- The team has had extensive risk discussions documented elsewhere

---

## The Pre-Mortem Session

### Format Options

**With a team (recommended for major launches):**
- 60-minute session
- Start with silent writing (5 min) before group discussion — prevents groupthink
- I'll facilitate the agenda below

**Solo (for individual decisions or small changes):**
- 30-minute thinking exercise
- I'll prompt you through each section

---

### Phase 1: Set the Scene (5 min)

State clearly:
- What are we launching or deciding?
- What would success look like? (specific metrics, outcomes)
- What's the timeline?

Tell the team: "I want you to imagine it's [X months] from now. This initiative failed badly. Revenue is down, users are angry, or we had to roll back. We're in a post-mortem. What happened?"

The past tense framing is critical. "What went wrong?" is much more specific than "what could go wrong?"

---

### Phase 2: Silent Failure Generation (5-10 min)

Each participant silently writes down as many failure scenarios as they can. No discussion yet. No filtering.

**Prompt:** "It's [X months] from now. [Initiative name] failed spectacularly. Write down every possible reason why. Be specific. Be pessimistic. Nothing is too obvious to write."

**Useful failure lenses:**
- What technical issues could kill this?
- What user behavior assumptions turned out to be wrong?
- What did we miss about the competitive landscape?
- What dependencies didn't deliver on time or at spec?
- What stakeholder dynamics went sideways?
- What market or external events disrupted the plan?
- What did we not test that we should have?

---

### Phase 3: Surface and Cluster (15-20 min)

Go around the room (or review your solo list). Each person shares one item at a time. No debate, just capture. Cluster similar failures into categories.

**Standard risk categories:**

| Category | Description | Example failure modes |
|----------|-------------|----------------------|
| Technical | Engineering, infrastructure, quality | Bugs at scale, performance degradation, data integrity issues, third-party API failures |
| User Adoption | Behavior assumptions wrong | Users don't discover the feature, don't understand value, revert to old behavior |
| Timeline | Scope or dependency slippage | Engineering misses date, design iterations take longer, external dependency delayed |
| Stakeholder | Internal alignment breaks down | Key exec changes direction, sales team doesn't adopt, CS team unprepared |
| Market | External environment shifts | Competitor launches similar feature, macro conditions change demand |
| Dependencies | Other teams or systems fail to deliver | Partner API unreliable, analytics not instrumented, legal review delayed |
| Communication | Messaging or positioning fails | Users confused by launch messaging, press coverage negative, internal launch botched |

---

### Phase 4: Score and Prioritize (10-15 min)

For each major risk cluster, assign:

| Risk | Likelihood | Impact | Priority |
|------|------------|--------|----------|
| [Risk] | High/Med/Low | High/Med/Low | Immediate/Watch/Accept |

**Priority definitions:**
- **Immediate:** Address before launch or decision. If this risk materializes and we haven't mitigated, we fail.
- **Watch:** Monitor closely. Build an early warning signal.
- **Accept:** Known risk we're consciously deciding to live with.

---

### Phase 5: Build Mitigations (15-20 min)

For every "Immediate" risk, define:

1. **Prevention:** What can we do NOW to make this failure less likely?
2. **Detection:** How will we know early if this is happening?
3. **Response:** If it happens, what's the plan? Who acts?

The mitigation isn't "be more careful." It's a specific, assigned action with a deadline.

---

### Phase 6: Confidence Assessment (5 min)

Close with an honest go/no-go assessment:

**Questions to answer together:**
1. Given what we surfaced, do we have sufficient confidence to proceed on the current plan?
2. Are there any "Immediate" risks we don't have a credible mitigation for?
3. What single thing, if it went wrong, would we most regret not having planned for?
4. What would change our risk picture significantly before launch?

---

## Output Template

```markdown
# Pre-Mortem: [Initiative Name]

**Date:** [date]
**Launch/decision date:** [date]
**Participants:** [names or solo]
**Facilitator:** [name]

---

## Initiative Summary

**What we're doing:** [1-2 sentence description]
**Success looks like:** [specific metric or outcome]
**Timeline:** [key milestones]

---

## Pre-Existing Risks (from PRD/launch plan)

[Pull from PRD risks section and launch plan — these were already known]

| Risk | Current mitigation | Status |
|------|-------------------|--------|
| [Risk] | [Mitigation] | [On track / At risk / Unmitigated] |

---

## New Risks Surfaced in Pre-Mortem

### Priority: Immediate (must address before launch)

**Risk:** [Description]
**Category:** [Technical / User Adoption / Timeline / Stakeholder / Market / Dependencies / Communication]
**Why it could happen:** [Root cause]
**Failure scenario:** "[In past tense: what happened in the imagined failure]"
**Prevention:** [Specific action + owner + date]
**Detection:** [How will we know early if this is happening? Leading indicator?]
**Response plan:** [If it happens, who does what?]

[Repeat for each Immediate risk]

---

### Priority: Watch (monitor closely)

**Risk:** [Description]
**Category:** [Category]
**Detection signal:** [What to watch for]
**Threshold:** [At what point does this become Immediate?]

[Repeat for each Watch risk]

---

### Priority: Accept (consciously living with)

**Risk:** [Description]
**Why we're accepting it:** [Rationale]
**Monitoring:** [Any passive monitoring we'll do]

[Repeat for each Accepted risk]

---

## Risk Register Summary

| Risk | Category | Priority | Owner | Mitigation deadline |
|------|----------|----------|-------|---------------------|
| [Risk] | [Cat] | [Priority] | [Name] | [Date] |

---

## Confidence Assessment

**Overall confidence to proceed:** [High / Medium / Low / Go with conditions]

**Critical open items before launch:**
- [ ] [Item 1 - must be resolved by date]
- [ ] [Item 2]

**What would change the risk picture:**
- [If X happens, we would escalate to High risk]
- [If Y is resolved, we would upgrade confidence to High]

**Go/No-go recommendation:**
- [Proceed / Proceed with conditions / Delay / Escalate to leadership]
- Rationale: [Why]

---

## Next Steps

- [ ] [Owner] addresses [Immediate Risk 1] by [date]
- [ ] [Owner] sets up detection signal for [Watch Risk] by [date]
- [ ] Share pre-mortem with [stakeholders] by [date]
- [ ] Revisit risk register [X days before launch]
```

---

## Running It Solo

If you're doing this solo, use these prompts to force honest thinking:

1. **The pessimist prompt:** "What's the most cynical reading of why this will fail?"
2. **The competitor prompt:** "If our biggest competitor knew everything about this plan, how would they take advantage of it?"
3. **The user prompt:** "What assumption are we making about user behavior that could be completely wrong?"
4. **The dependency prompt:** "Which external dependency has the highest chance of not being ready?"
5. **The history prompt:** "What failed in a similar launch before? Are we making the same mistake?"
6. **The Cassandra prompt:** "Who on the team has raised a concern that we dismissed? What if they were right?"

---

## Output Integration

### Where Files Go

**Pre-mortem outputs:**
- Active work: `outputs/pre-mortems/pre-mortem-[initiative]-[date].md`
- Mitigations get added to launch checklist in `context-library/launches/`
- Immediate risk decisions → log in `outputs/decisions/`

### Cross-Skill Integration

**Feeds into:**
- `/launch-checklist` - Embed immediate mitigations into launch plan
- `/decision-doc` - Go/no-go decision with documented rationale
- `/status-update` - Risk summary for stakeholder communication
- `/board-deck` - Risk section for executive presentations

**Pulls from:**
- `context-library/prds/` - Known risks from PRD assumptions
- `context-library/launches/` - Launch plan dependencies
- `context-library/meetings/` - Concerns raised in planning conversations

---

## Output Quality Self-Check

Before delivering the pre-mortem output, verify:

- [ ] **Existing context checked:** PRD risks, launch plan, meeting notes reviewed before the session
- [ ] **Past-tense framing used:** Failure scenarios written as if they've already happened, not "could happen"
- [ ] **All 7 categories covered:** Technical, User Adoption, Timeline, Stakeholder, Market, Dependencies, Communication
- [ ] **Priority assigned to every risk:** Every surfaced risk is Immediate / Watch / Accept — none left unprioritized
- [ ] **Immediate risks have specific mitigations:** Prevention, detection, and response plan — not "be careful"
- [ ] **Owners assigned:** Every Immediate mitigation has a name, not just "the team"
- [ ] **Go/no-go recommendation made:** Clear statement on whether to proceed, with conditions if needed
- [ ] **Output file saved:** Saved to `outputs/pre-mortems/pre-mortem-[initiative]-[date].md`
