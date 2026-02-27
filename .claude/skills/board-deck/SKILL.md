---
name: board-deck
description: Prepare executive and board-level presentations. Structures narrative, builds slide outlines, identifies data needed, and ensures strategic clarity. Covers board updates, all-hands, executive business reviews, and investor updates.
disable-model-invocation: false
user-invocable: true
---

# /board-deck - Prepare Executive Presentations

Build board-ready presentations that get decisions made. Every executive deck has two jobs: inform and ask. This skill helps you do both clearly.

## Quick Start

```
/board-deck

Preparing an executive presentation. Tell me:

1. Deck type: Board update / All-hands / Executive Business Review (EBR) / Investor update
2. Quarter or time period this covers
3. Key narrative: What's the headline story of this period? (e.g., "strong growth quarter with a key risk to flag", "we missed revenue but the product pivot is working")
4. Key ask: What decision do you need from this audience? (approval, awareness, resources, or just an update?)

I'll check your strategy, OKRs, and metrics context to populate what I can.

Output: outputs/board-decks/[deck-type]-[quarter]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

**Automatic Context Checks:**
When this skill is invoked, immediately check:

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| OKRs / Strategy | `context-library/strategy/*.md` | OKR, Q1, Q2, objective, key result, strategic priority | Current quarter goals and progress |
| Metrics | `context-library/metrics/*.md` | revenue, ARR, NRR, DAU, MAU, activation, retention, NPS | Key metrics for the period |
| PRDs / Feature Results | `context-library/prds/*.md`, `context-library/launches/*.md` | shipped, launched, released | Product wins and milestones |
| Decisions | `context-library/decisions/*.md` | major decision, trade-off, approved | Key decisions made this period |
| Stakeholder Profiles | `context-library/stakeholder-template.md` | board, CEO, CFO, investors | Audience context and priorities |

**Context Priority:**
1. Quarter OKR progress FIRST (what were our goals, how are we tracking?)
2. Key metrics SECOND (numbers that tell the story)
3. Product wins and milestones THIRD (what shipped, what's next)
4. Key decisions and risks FOURTH

**Cross-Skill Links:**
- After deck → Share with stakeholders via `/slack-message`
- Before deck → Run `/pre-mortem` on any risk you're surfacing
- Metrics section → Pull from `/feature-results` if relevant to quarter narrative
- OKR section → Pull from `/okr-planning` for current quarter OKRs

---

## Step 0: Understanding the Context

Before building the deck structure, let me check what exists...

**Checking:**
- `context-library/strategy/` for current OKRs and strategic priorities
- `context-library/metrics/` for the key numbers of the period
- `context-library/launches/` for what shipped this quarter
- `context-library/decisions/` for major decisions made

**Based on what I find, I'll show you:**

### Quarter Summary

**OKR Status:**
- [Progress on each Objective: Red / Yellow / Green]

**Key Metrics:**
- [Core business metrics and current values vs. targets]

**Product Milestones:**
- [Major features shipped, experiments run, launches executed]

**Major Decisions:**
- [Key decisions made this quarter]

### Audience and Ask Clarification

Before structuring the deck, confirm:

1. **Who's in the room?** (Board + exec team / all-hands / external investors / department heads)
2. **What do they already know?** (Context they have vs. what needs setting)
3. **What decision or reaction do you need?** (Approve resource, greenlight strategy, absorb update, vote)
4. **Tone:** (Celebration, course-correction, urgent escalation, steady progress)

---

## Deck Type Selector

### Board Update

**Purpose:** Inform board on company progress, surface strategic decisions, request oversight on key risks.

**Standard structure (12-15 slides, 30-45 min):**

1. **Opening headline** — The one thing to take away from this meeting
2. **State of business** — Business health dashboard (revenue, growth rate, key metrics vs. plan)
3. **Product progress** — What we shipped, what it's doing for users and revenue
4. **OKR scorecard** — How we're tracking against the quarter's Objectives and Key Results
5. **Strategic context** — What's changed in the market, competitive landscape, or macro environment
6. **Key decisions needed** — Specific asks to the board (budget, strategy, governance)
7. **Risks** — What keeps leadership up at night, with mitigation status
8. **Next quarter outlook** — What we're focused on, what to expect
9. **Appendix** — Deep dives on any slide that needs supporting data

**Board presentation rules:**
- Lead with numbers first — board members are pattern-matching on metrics before anything else
- Never bury the lede — the most important thing goes on slide 1 or 2, not slide 10
- Be explicit about what you need — "We're here to inform you about X" vs. "We need a decision on Y"
- Surface bad news early and with a plan — boards respond better to "we have a problem and here's how we're handling it" than discovering it in Q&A
- Leave 40% of the meeting for Q&A — they want to talk, not just listen

---

### All-Hands

**Purpose:** Align the full team on company progress, celebrate wins, clarify direction, answer questions.

**Standard structure (10-12 slides, 45-60 min):**

1. **Opening energy** — Set the tone (celebration, challenge, transition)
2. **Company performance** — Business health metrics in plain language (revenue relative to plan, growth)
3. **Product wins** — What we shipped and why it matters for users and the business
4. **Team wins** — Individual and team callouts, hires, anniversaries
5. **Honest update** — What's working, what's hard, what we're doing about it
6. **Strategic direction** — Where we're headed and why (reinforce or update)
7. **Priorities for next quarter** — Top 3 things we're focused on
8. **Q&A** — Real questions, real answers (save ample time)

**All-hands rules:**
- Speak in plain language — no jargon, no acronyms without definition
- Acknowledge hard things — teams know when things are hard; pretending otherwise destroys trust
- Make space for real questions — anonymous Q submission tools help surface honest concerns
- Close with energy and clarity — people should leave knowing what to focus on

---

### Executive Business Review (EBR)

**Purpose:** Customer-facing or internal quarterly review of product performance against goals. Often used with enterprise customers.

**Standard structure (10-12 slides, 45-60 min):**

1. **Agenda and context** — Why we're meeting and what we'll cover
2. **Goals we set together** — OKRs or success metrics agreed upon at last EBR
3. **Progress against goals** — How we tracked, with specific metrics
4. **Product updates** — What shipped that's relevant to their use case
5. **Usage and adoption trends** — How their team is using the product
6. **Gaps and action items** — Where we fell short and what we're doing
7. **Next period goals** — Proposed goals for next quarter
8. **Product roadmap** — What's coming that matters to them
9. **Partnership health** — Relationship, contract, renewal context

---

### Investor Update

**Purpose:** Maintain investor confidence, share progress, flag needs (capital, introductions, board input).

**Standard structure (8-10 slides, 20-30 min):**

1. **Business summary** — Revenue, ARR, burn rate, runway (the numbers investors track)
2. **Highlights** — Top 3-5 wins since last update
3. **Lowlights** — Top 1-3 challenges (don't hide these — investors find out anyway)
4. **Metrics dashboard** — Core growth, retention, and unit economics
5. **Product update** — What shipped, what's next
6. **Team update** — Key hires, departures, org changes
7. **Ask** — Specific help needed (introductions, advice on X, follow-on conversation)
8. **Appendix** — Full financials, cap table, detailed metrics

---

## Data Request List

Before building the deck, surface the data you need to gather:

```markdown
## Data Needed for [Deck Type]: [Quarter]

**Business Performance**
- [ ] Revenue vs. plan (and vs. same period last year if applicable)
- [ ] ARR / MRR growth rate
- [ ] Gross margin (if applicable)
- [ ] Burn rate and runway (board/investor only)
- [ ] Key conversion rates (trial → paid, free → paid, etc.)

**Product Metrics**
- [ ] Active users (DAU / WAU / MAU) and trend
- [ ] Activation rate and trend
- [ ] Retention / churn rate and trend
- [ ] NPS score and trend
- [ ] Top features by usage

**OKR Status**
- [ ] Confidence level on each KR (Red / Yellow / Green)
- [ ] Actual vs. target for each Key Result

**Product Wins**
- [ ] List of features shipped this quarter
- [ ] Impact metrics for major launches (if available via /feature-results)

**Risks and Challenges**
- [ ] Top 3 risks with current mitigation status
```

---

## Narrative Arc Builder

Every deck needs a story. Before building slides, answer these questions:

**Hook:** What's the single most important thing this audience should take away?
> "This was our strongest product quarter, with activation up 18 points, though we need to flag growing competitive pressure from [Competitor]."

**Current state:** Where are we relative to plan?
> "We're at 87% of our Q1 ARR target with one month left. Product metrics are ahead of plan. Sales pipeline is the watch item."

**Progress:** What moved forward?
> "We shipped [Feature X] — our activation rate went from 42% to 60% in the 4 weeks since launch."

**Challenges:** What's hard and what are we doing about it?
> "[Competitor] launched a similar feature at a lower price point. We're monitoring deal impact — 2 deals lost so far. Response plan: [X]."

**Ask:** What do you need from this audience?
> "We're asking the board to approve $500K in additional sales headcount for H2."

Once you have these five answers, the slides write themselves.

---

## Speaker Notes Template

For slides with complex or sensitive content:

```markdown
**Slide [N]: [Slide Title]**

**What to say:** [2-3 sentences of talking points]
**What NOT to say:** [Anything that might create confusion or unintended questions]
**Likely question:** [The question this slide will prompt]
**Your answer:** [The answer you want to give]
**If pressed:** [What to say if they push further]
```

---

## Output Template

```markdown
# [Deck Type]: [Quarter] [Year]

**Date:** [date]
**Audience:** [Board / All-hands / EBR / Investors]
**Key narrative:** [One-sentence summary of the story]
**Key ask:** [What you need from this audience]

---

## Slide Outline

### Slide 1: [Title]
**Purpose:** [What this slide accomplishes]
**Content:** [What goes on it]
**Key data points:**
- [Data point 1]
- [Data point 2]
**Speaker notes:** [What to say]

[Repeat for each slide]

---

## Data Still Needed

- [ ] [Data point to gather and from whom]
- [ ] [Data point to gather]

---

## Key Message Anchors

- **If they ask about [topic]:** [How to respond]
- **If they ask about [topic]:** [How to respond]

---

## Pre-Meeting Checklist

- [ ] Slides reviewed by [stakeholder] by [date]
- [ ] Data confirmed with finance/analytics by [date]
- [ ] Dry run scheduled with [person] on [date]
- [ ] Backup slides prepared for likely deep-dives
```

---

## Output Integration

### Where Files Go

**Board deck outlines:**
- Active work: `outputs/board-decks/[deck-type]-[quarter]-[date].md`
- After meeting: Archive notes from the meeting to `context-library/meetings/`
- Key decisions made → log to `context-library/decisions/`

### Cross-Skill Integration

**Feeds into:**
- `/status-update` - Board deck narrative informs stakeholder updates
- `/slack-message` - Send pre-read or follow-up to meeting participants

**Pulls from:**
- `/okr-planning` - OKR progress feeds the OKR scorecard section
- `/feature-results` - Post-launch results feed the product wins section
- `/impact-sizing` - Revenue impact claims need sizing backing
- `/pre-mortem` - Risk section draws from pre-mortem outputs
- `context-library/metrics/` - All key metrics for the period

---

## Common Mistakes

**Mistake 1: Too many slides, not enough story**
Board members won't read 40 slides. 12-15 is the max. If it doesn't fit, cut it or move to appendix.

**Mistake 2: Leading with your best news**
Lead with the most important thing (positive or negative), not the most flattering thing.

**Mistake 3: No clear ask**
Every board meeting needs a specific ask. Even "awareness updates" should end with "Here's what we need you to do / not do / keep in mind."

**Mistake 4: Not practicing the Q&A**
The Q&A is often harder than the prepared remarks. List your 10 most likely hard questions and know your answers cold.

**Mistake 5: Hiding bad news in appendix**
Board members will find it. Surface it directly and with a response plan.

---

## Output Quality Self-Check

Before delivering the deck outline, verify:

- [ ] **Context checked:** OKRs, metrics, product launches, and decisions pulled from context library
- [ ] **Audience-specific structure:** Board / All-hands / EBR / Investor — correct template used
- [ ] **Key narrative is one sentence:** Clear headline story defined before building slides
- [ ] **Key ask is explicit:** Audience knows exactly what you need from them
- [ ] **Data gaps flagged:** List of data still needed before the deck can be finalized
- [ ] **Speaker notes drafted:** At least for slides covering sensitive or complex topics
- [ ] **Likely Q&A prepared:** 5+ likely questions with suggested answers
- [ ] **Appendix flagged:** Topics that need backup slides identified
- [ ] **Output file saved:** Saved to `outputs/board-decks/[deck-type]-[quarter]-[date].md`
