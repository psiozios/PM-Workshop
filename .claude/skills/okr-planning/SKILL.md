---
name: okr-planning
description: Create and align quarterly OKRs with product strategy and North Star. Two modes - write OKRs from scratch or critique existing OKRs for measurability, alignment, and balance.
disable-model-invocation: false
user-invocable: true
---

# /okr-planning - Create Aligned, Measurable OKRs

Build OKRs that actually drive decisions — connected to your North Star, grounded in strategy, and specific enough to measure. Two modes: **Write** (create OKRs from scratch) or **Critique** (review existing OKRs for gaps).

## Quick Start

```
/okr-planning

Two modes available:

Write: I'll help you build product OKRs for the quarter, aligned to company goals and North Star.
Critique: Paste your existing OKRs and I'll flag vanity metrics, measurability issues, and alignment gaps.

Tell me:
1. Mode: Write or Critique?
2. Scope: Company OKRs, Product OKRs, Team OKRs, or a cascade of all three?
3. Quarter: Which quarter are we planning for?

I'll check your strategy and North Star context first.

Output: outputs/okrs/okrs-[quarter]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

**Automatic Context Checks:**
When this skill is invoked, immediately check:

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| North Star | `context-library/strategy/*.md` | "north star", "NSM", "primary metric" | Current North Star definition |
| Existing OKRs | `context-library/strategy/*.md` | "OKR", "objective", "key result", "Q1", "Q2" | Previous quarter OKRs for continuity |
| Business Goals | `context-library/business-info-template.md` | goals, priorities, revenue target, growth | Company-level goals OKRs should ladder to |
| Strategy | `context-library/strategy/*.md` | "priority", "pillar", "strategic bet" | Strategic pillars to align OKRs to |
| PRD Pipeline | `context-library/prds/*.md` | upcoming features, initiatives | Product work that should be reflected in OKRs |
| Metrics | `context-library/metrics/*.md` | "baseline", "current", "benchmark" | Current metric baselines for Key Results |

**Context Priority:**
1. Company goals and North Star FIRST (OKRs must ladder up)
2. Previous quarter OKRs SECOND (continuity and learning from what worked)
3. Strategy pillars THIRD (OKRs should reflect strategic bets)
4. Current metric baselines FOURTH (Key Results need starting points)

**Cross-Skill Links:**
- OKRs → inform `/weekly-plan` (weekly priorities ladder to OKRs)
- OKRs → inform `/metrics-framework` (Key Results become leading/lagging indicators)
- OKRs → inform `/define-north-star` (OKRs should ladder to the North Star)
- OKRs → inform `/write-prod-strategy` (strategy shapes OKRs, OKRs reflect strategy)
- OKR check-ins → `/status-update` for quarterly progress reports

---

## Step 0: Understanding Your Context

Before writing OKRs, let me check what already exists...

**Checking:**
- `context-library/strategy/` for North Star, strategic pillars, previous OKRs
- `context-library/business-info-template.md` for company-level goals and revenue targets
- `context-library/metrics/` for current metric baselines
- `context-library/prds/` for upcoming product work

**Based on what I find, I'll show you:**

### Current State

**North Star Metric:**
- [Current NSM if defined]

**Company Goals This Quarter:**
- [Goals from business-info template]

**Previous Quarter OKRs:**
- [Summary of Q-1 OKRs and how they went — did we hit them?]

**Strategic Pillars:**
- [Current strategic priorities from strategy docs]

**Baseline Metrics:**
- [Key metrics and their current values — needed for Key Result targets]

**PM-Specific Questions to Clarify:**

1. **Scope:** Are these OKRs for the whole product, a specific team, or a single initiative?
2. **Time horizon:** Quarter or half-year? (most product OKRs are quarterly)
3. **Top priority:** What's the single most important outcome for this period?
4. **Carryover:** Are there OKRs from last quarter we're continuing vs. resetting?
5. **Constraints:** Any known constraints (headcount, tech debt, dependencies) that limit what we can commit to?

---

## OKR Framework

### The Structure

An OKR has two parts:

**Objective** — *What* we're trying to achieve. Qualitative, inspiring, action-oriented. Should make people want to work toward it.

**Key Results** — *How* we'll measure success. 3-5 per Objective. Quantitative, time-bound, specific. If it doesn't have a number, it's not a Key Result.

**Initiatives** — *What we'll do* to achieve the Key Results. These are your projects and features. They go under OKRs but aren't part of the OKR itself.

### Strong OKR Formula

```
Objective: [Verb] + [Outcome] + [Why it matters / for whom]
Key Result: [Metric] from [baseline] to [target] by [date]
Initiative: [Project or feature that contributes to the Key Result]
```

**Example:**
- Objective: "Transform onboarding so new users reach their first value moment faster"
- KR1: Increase Day-7 activation from 42% to 60% by end of Q2
- KR2: Reduce time-to-first-value from 14 days to 5 days by end of Q2
- KR3: Increase new user NPS from 34 to 50 by end of Q2
- Initiative: Redesign onboarding flow (PRD-2026-04)
- Initiative: In-app guided tour for core features

---

## Write Mode: Building OKRs From Scratch

### Step 1: Set the Objective

A strong Objective:
- Is qualitative and aspirational (not a metric)
- Describes an outcome, not an activity
- Is specific enough to guide decisions
- Is achievable in the quarter (but ambitious)
- Answers "why does this matter?"

**Objective quality test:** If your team hits this Objective, are they unambiguously winning? If yes, it's a good Objective.

**Common failure modes:**
- "Improve the product" (too vague)
- "Ship feature X" (activity, not outcome)
- "Increase revenue" (too broad, not product-specific)
- "Complete the Q2 roadmap" (this is a task list, not a strategy)

### Step 2: Write Key Results

A strong Key Result:
- Has a number (baseline → target)
- Has a date
- Is measurable with existing data (or you commit to instrumentation)
- Moves when the Objective moves
- Is challenging but achievable (60-70% confidence of hitting it)

**Key Result quality tests:**
1. "If we hit all Key Results but the Objective doesn't feel achieved, something is wrong with the KRs."
2. "If the KR moved but we didn't make progress on the Objective, the KR is measuring the wrong thing."
3. "Would we be proud of this result, or is it a vanity metric?" (vanity metrics go up regardless of strategy)

**Leading vs lagging balance:**
- Include at least one leading indicator (early signal of success)
- Include at least one lagging indicator (final outcome)
- Avoid all-lagging OKRs (you won't know you're off track until it's too late)

### Step 3: Identify Initiatives

For each Key Result, list the 2-3 projects or features that will move it. These are your bets.

Then check: if you complete all these initiatives, is it actually likely to move the KR? If not, you're missing something.

### Step 4: Cascade and Align

**Alignment check questions:**
1. Does each Product OKR ladder to at least one Company OKR?
2. Does at least one Product KR ladder to the North Star?
3. Are there company goals with NO product OKR aligned to them? (flag for discussion)
4. Are there Product OKRs that don't connect to any company goal? (cut them)

**Cascade template:**

```
Company Objective: [Company-level goal]
    └── Product Objective: [Product contribution to company goal]
            ├── KR1: [Metric baseline → target by date]
            │   └── Initiative: [Project that moves KR1]
            ├── KR2: [Metric baseline → target by date]
            │   └── Initiative: [Project that moves KR2]
            └── KR3: [Metric baseline → target by date]
                └── Initiative: [Project that moves KR3]
```

---

## Critique Mode: Reviewing Existing OKRs

When you paste existing OKRs for review, I'll evaluate using these criteria:

### Objective Critique

| Signal | What Good Looks Like | Red Flag |
|--------|---------------------|----------|
| Outcome focus | Describes a meaningful outcome | Activity-based ("ship X", "run Y") |
| Specificity | Specific enough to drive decisions | Too broad to choose between two paths |
| Ambition | Challenging but achievable | Either too safe (no stretch) or fantasy |
| Motivation | Team actually wants to achieve this | Corporate-sounding, no one cares |

### Key Result Critique

| Signal | What Good Looks Like | Red Flag |
|--------|---------------------|----------|
| Has a number | "From X to Y by date" | "Improve", "increase", "optimize" |
| Measures outcomes | Tracks a result that matters | Tracks a task ("complete 5 features") |
| Baseline exists | You know where you're starting | No baseline = no way to know if you're winning |
| Leading indicators | At least one early signal | All lagging = blind until end of quarter |
| Stretch level | 60-70% confidence | Either guaranteed (too easy) or 0% realistic |

### Alignment Critique

- Does each KR move if the Objective is achieved?
- Does the Objective ladder to company goals?
- Are there company goals with no OKR coverage?
- Are there OKRs that exist purely for the team's benefit but don't connect to company outcomes?

### Balance Critique

- Too many OKRs? (3 Objectives max, 3-5 KRs each)
- All health metrics and no growth? Or all growth and no sustainability?
- Missing guardrail metrics? (KRs that define what you WON'T sacrifice)

---

## Check-in Cadence Template

OKRs without check-ins are aspirations. Build the rhythm:

```markdown
## OKR Check-in Cadence: [Quarter]

**Weekly (5 min):**
- What's my confidence level on each KR? (Red / Yellow / Green)
- What did I do this week that moved a KR?
- What's the biggest threat to any KR right now?

**Monthly (30 min):**
- Score each KR (0-1 scale): How far along are we?
- What's working that we should double down on?
- What's not working that we should adjust or cut?
- Any KRs we should renegotiate based on new information?

**Quarterly (60 min):**
- Final scores for each KR
- What did we learn? What surprised us?
- What should carry into next quarter's OKRs?
- What do the results tell us about our strategy?
```

**Scoring guidance:**
- 0.7-1.0: Achieved (if consistently hitting 1.0, your OKRs aren't ambitious enough)
- 0.4-0.6: Partial (learn from the gap — what held you back?)
- 0-0.3: Missed (understand why before setting next quarter's OKRs)

---

## Output Template

```markdown
# OKRs: [Quarter] [Year] - [Product / Team Name]

**Date created:** [date]
**Last updated:** [date]
**Scope:** [Product / Team / Company]

---

## Context

**North Star Metric:** [NSM and current value]
**Company OKRs this quarter:** [summary or link]
**Strategic priority this quarter:** [one-sentence summary]

---

## OKR 1

**Objective:** [Objective statement]
**Why this quarter:** [Brief rationale — why now?]
**Company OKR alignment:** [Which company goal does this ladder to?]

| Key Result | Baseline | Target | Owner | Confidence |
|------------|---------|--------|-------|------------|
| KR1: [metric] | [current] | [target by date] | [name] | [%] |
| KR2: [metric] | [current] | [target by date] | [name] | [%] |
| KR3: [metric] | [current] | [target by date] | [name] | [%] |

**Initiatives:**
- [ ] [Project/feature] → Moves KR[X] by [estimated impact]
- [ ] [Project/feature] → Moves KR[X] by [estimated impact]

---

## OKR 2

[Same structure]

---

## OKR 3 (if needed)

[Same structure]

---

## What We're NOT Doing This Quarter

[Be explicit about strategic trade-offs. What are you deprioritizing?]
- [Initiative or area] - Deprioritized because [reason]
- [Initiative or area] - Deprioritized because [reason]

---

## Guardrail Metrics

Even while chasing the Objectives, we won't sacrifice:

| Metric | Current | Floor | Why it matters |
|--------|---------|-------|----------------|
| [e.g., Churn rate] | [X%] | [Max acceptable Y%] | [Rationale] |
| [e.g., Support SLA] | [X hrs] | [Max Y hrs] | [Rationale] |

---

## Risk Register

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk to OKR achievement] | H/M/L | H/M/L | [Plan] |
```

---

## Output Integration

### Where Files Go

**OKR documents:**
- Active work: `outputs/okrs/okrs-[quarter]-[date].md`
- When finalized: Move to `context-library/strategy/` for reference in future planning

### Cross-Skill Integration

**Feeds into:**
- `/weekly-plan` - Weekly priorities ladder to quarterly OKRs
- `/metrics-framework` - KRs become leading/lagging indicator definitions
- `/status-update` - OKR progress feeds into stakeholder updates
- `/board-deck` - OKR overview and quarterly progress for executive presentations
- `/define-north-star` - OKRs should ladder to the North Star

**Pulls from:**
- `context-library/strategy/` - North Star, strategic pillars, previous OKRs
- `context-library/business-info-template.md` - Company-level goals
- `context-library/metrics/` - Current metric baselines for KR targets

---

## Output Quality Self-Check

Before delivering OKRs, verify:

- [ ] **Context checked first:** North Star, company goals, previous OKRs, and metric baselines reviewed
- [ ] **Objectives are outcome-based:** No activity-based Objectives ("ship X", "complete Y")
- [ ] **Key Results have numbers:** Every KR has a baseline, target, and date — none say "improve" without a metric
- [ ] **Leading and lagging balance:** At least one leading indicator per Objective
- [ ] **Cascade verified:** Every Product OKR aligns to at least one Company OKR
- [ ] **North Star laddered to:** At least one KR connects to or moves the North Star
- [ ] **"Not doing" section included:** Explicit trade-offs documented
- [ ] **Guardrail metrics defined:** What we won't sacrifice while chasing OKRs
- [ ] **Check-in cadence set:** Weekly, monthly, and quarterly rhythms defined
- [ ] **Output file saved:** Saved to `outputs/okrs/okrs-[quarter]-[date].md`
