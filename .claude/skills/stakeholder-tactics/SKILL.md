---
name: stakeholder-tactics
description: Navigate stakeholder dynamics with confidence. Map influence, align competing priorities, handle resistance, prepare for difficult conversations, and build the political capital to move your roadmap forward.
disable-model-invocation: false
user-invocable: true
---

# /stakeholder-tactics - Influence Without Authority

Products die in alignment gaps. This skill helps you read the room, manage competing agendas, and get the decisions you need from people who don't report to you.

## Quick Start

```
/stakeholder-tactics

Modes:
--map       Build a stakeholder influence map for an initiative
--align     Create an alignment strategy for a specific stakeholder
--conflict  Navigate a competing priority or stakeholder conflict
--meeting   Prepare for a difficult stakeholder conversation

Tell me:
1. Mode (or describe the stakeholder situation)
2. What decision or outcome you need
3. Who the key stakeholders are (name and role if possible)

I'll check context-library/stakeholder-template.md for existing profiles.

Output: outputs/analyses/stakeholder-[initiative]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Stakeholder Profiles | `context-library/stakeholder-template.md` | name, role, priority, communication | Existing stakeholder context |
| Meeting Notes | `context-library/meetings/*.md` | stakeholder names, alignment, concern, pushback | Recent interaction history |
| Strategy | `context-library/strategy/*.md` | priority, roadmap, OKR, bet | Strategic context for alignment |
| Decisions | `context-library/decisions/*.md` | stakeholder, approved, rejected | Past decisions and how they were made |

**Cross-Skill Links:**
- After alignment → `/decision-doc` to formally document the agreed path
- For a difficult meeting → `/meeting-agenda` to structure the conversation
- Stakeholder comms → `/slack-message` or `/status-update`
- Competing priority needs analysis → `/prioritize`

---

## Mode: --map (Stakeholder Influence Map)

### Step 1: Identify All Stakeholders

For a given initiative, list everyone who:
- Approves or blocks (decision-makers)
- Influences decision-makers (advisors, champions)
- Is affected by the outcome (customers of this decision)
- Has relevant context others don't (technical, customer, market SMEs)

### Step 2: Assess Each Stakeholder

```
## [Name], [Title]

Stance: [Champion / Supportive / Neutral / Skeptical / Opposed]
Influence level: [High / Medium / Low] — [why]
Primary motivation: [What they care most about — growth, efficiency, risk, recognition, budget]
What they gain from this initiative: [Specific benefit to them]
What they might lose or fear: [Their risk — territory, budget, workload, failure visibility]
Information they have that others don't: [Their unique context]
Communication preference: [Data-first / Story-first / Async / Sync / Written / Verbal]
```

### Step 3: Prioritize Engagement

Plot stakeholders on a 2x2:

```
          HIGH INFLUENCE
               |
    MANAGE     |    COLLABORATE
    CLOSELY    |    CLOSELY
               |
  ─────────────┼─────────────
               |
    KEEP       |    CONSULT &
    INFORMED   |    INVOLVE
               |
          LOW INFLUENCE
    OPPOSED   ---  SUPPORTIVE
```

- **Collaborate Closely:** High influence + supportive. These are your champions. Give them early access, involve in decisions.
- **Manage Closely:** High influence + skeptical/opposed. Highest priority. Address their concerns before the big meeting.
- **Consult and Involve:** Low influence + supportive. Keep them informed and use them to build social proof.
- **Keep Informed:** Low influence + skeptical. Don't over-invest; don't ignore.

---

## Mode: --align (Alignment Strategy for One Stakeholder)

### Understand Their Reality First

Before you try to bring someone along, understand their world:

| Question | Why it matters |
|----------|---------------|
| What are their top 3 priorities this quarter? | Alignment happens by helping them win, not just explaining your initiative |
| What's their biggest fear about this initiative? | Address it directly — don't wait for them to raise it |
| Who do they trust for product advice? | Work through influencers before the big meeting |
| What past decisions have they been burned by? | Context for their skepticism |
| What would make them look good for supporting this? | Frame your ask in their language |

### Alignment Conversation Structure

1. **Start with their win** — "I know you're focused on X this quarter. I think this helps."
2. **State your hypothesis, not your conclusion** — "I believe this would [outcome], but I want to understand if you see it differently"
3. **Ask before advocating** — "Before I walk through our thinking, what are your instincts on this?"
4. **Acknowledge their concern directly** — "I've heard that [concern] is your worry. That's legitimate. Here's how we've thought about it..."
5. **Give them a clear ask** — "What I need from you is [specific thing]. What would make that possible?"

---

## Mode: --conflict (Competing Priority or Stakeholder Conflict)

### Diagnose the Type of Conflict

| Type | Characteristics | Resolution approach |
|------|----------------|---------------------|
| **Resource conflict** | Both initiatives need the same team/budget | Explicit prioritization conversation with sponsor |
| **Strategic conflict** | Different views on what matters most | Escalate to shared decision-maker with clear trade-offs |
| **Data conflict** | Disagreement on facts or user needs | Shared research or data pull; get to common facts first |
| **Relationship conflict** | Personal history or style clash | Address the meta-issue; don't litigate the object-level |
| **Timing conflict** | Right thing, wrong sequence | Propose a phased approach; don't fight over when |

### Conflict Resolution Playbook

**1. Get to shared facts first**
"Before we debate what to do, can we agree on what the data shows? Here's what I'm seeing..."

**2. Acknowledge the other position's validity**
"I understand why [alternative] is appealing. From your vantage point, [reason their view makes sense]."

**3. Make the trade-offs explicit**
"If we do [their preference], we gain [X] but give up [Y]. If we do [your preference], we gain [A] but give up [B]. Which trade-off is more acceptable to the business?"

**4. Propose a decision-making process, not just a decision**
"Given we see this differently, here's how I'd suggest we resolve it: [specific process]. Does that seem fair?"

**5. Know when to escalate**
Escalate when: you're in a loop without progress, the decision has significant downstream impact, or the conflict is blocking team execution. Escalate with the trade-offs framed, not just the disagreement.

---

## Mode: --meeting (Difficult Stakeholder Meeting Prep)

### Pre-Meeting Checklist

```
Initiative: [Name]
Meeting goal: [Specific decision or alignment you need]
Key stakeholders: [List]

For each participant:
- What do they want out of this meeting?
- What might they say that you're not prepared for?
- What's the one thing you most need them to understand?

My ask: [Specific, clear, one sentence]
My fallback: [If they can't agree to the ask, what's the minimum viable outcome?]
```

### Handling Common Objections

| Objection | Likely subtext | Response approach |
|-----------|---------------|-------------------|
| "This isn't the right priority" | Fear of losing budget or team to this | Show how it serves their priorities too |
| "We don't have the data to decide" | Avoidance or legitimate uncertainty | Distinguish "need more data" from "paralysis"; propose a bounded experiment |
| "Sales needs X first" | Sales is loud; data may not support | Quantify both asks; let the numbers make the case |
| "We tried this before and it failed" | Fear of history repeating | Acknowledge, explain what's different, propose small-bet validation |
| "Legal/security/compliance won't allow it" | May be gatekeeping vs. genuine block | Get the specific objection in writing; most have solutions |

### Post-Meeting

After every stakeholder meeting, capture:
- What was decided
- What was left open
- Any change in their stance
- Next action and who owns it

Run `/meeting-notes` and update `context-library/stakeholder-template.md` with anything new you learned about them.

---

## Output Template

```markdown
# Stakeholder Strategy: [Initiative Name]

**Date:** [date]
**Decision needed:** [Specific outcome]
**Timeline:** [When the decision must happen]

---

## Stakeholder Map

| Stakeholder | Stance | Influence | Primary concern | Engagement priority |
|-------------|--------|-----------|----------------|---------------------|
| [Name] | [Champion/Neutral/Skeptical] | [H/M/L] | [Their concern] | [High/Medium/Low] |

---

## Alignment Plan

### [Priority Stakeholder 1]
**Their win from this:** [Specific]
**Their concern:** [Specific]
**My approach:** [Steps to bring them along]
**Ask:** [What I need from them]

---

## Conflict Diagnosis (if applicable)

**Conflict type:** [Resource / Strategic / Data / Relationship / Timing]
**Resolution approach:** [Specific steps]
**Escalation path:** [If needed, who resolves this and by when]

---

## Pre-Meeting Prep (if applicable)

**Goal of meeting:** [Decision or alignment outcome]
**My ask:** [One sentence]
**My fallback:** [Minimum viable outcome]
**Anticipated objections and responses:**
- [Objection] → [Response]
```

---

## Output Quality Self-Check

- [ ] **All stakeholders mapped:** Influence, stance, and primary concern for each
- [ ] **Their motivation addressed:** Not just "convince them we're right" — what's in it for them
- [ ] **Conflict diagnosed correctly:** Root cause identified, not just symptoms
- [ ] **Clear ask articulated:** One sentence, specific, actionable
- [ ] **Fallback defined:** Not walking in without a Plan B
- [ ] **Post-meeting capture planned:** Update stakeholder profiles after the conversation
- [ ] **Output saved:** `outputs/analyses/stakeholder-[initiative]-[date].md`

---

## Second Brain Integration

The `stakeholders` focus area of the second brain is now the **canonical store** for stakeholder profiles, replacing a parallel local store. `/stakeholder-tactics --map` reads from and writes to `context-library/second-brain/stakeholders/wiki/`.

**Before any mode runs:** query the `stakeholders` focus area for the relevant person or group. Pull:
- Communication preferences (channel, cadence, depth)
- Priorities (stated and inferred)
- Past decisions involving them and how they went
- Relationship history (trust level, friction points, wins)
- Signals from recent meetings (filed there by `/meeting-notes`)

**At the end of any mode that produces new stakeholder information, offer: "File to Second Brain? (y/n)"**

If yes, ingest into `stakeholders`:
- `--map` creates or updates a wiki page per stakeholder (one page per person, aggressive `[[cross-links]]` between people on the same team or in the same reporting chain)
- `--align`, `--conflict`, `--prep` modes add new observations as updates to existing pages

Invoke `/second-brain ingest` with the output as the source. If `stakeholders` doesn't exist yet, offer `/second-brain init stakeholders` first.

Stakeholder memory is the hardest PM knowledge to keep because it lives in dozens of meetings. The brain is where that memory actually sticks.
