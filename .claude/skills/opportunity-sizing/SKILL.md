---
name: opportunity-sizing
description: Evaluate the strategic size of a market or product opportunity before committing to build. Combines market analysis (TAM/SAM/SOM), problem frequency, willingness to pay, and competitive landscape into a structured investment thesis.
disable-model-invocation: false
user-invocable: true
---

# /opportunity-sizing - Evaluate the Bet Before You Make It

Impact sizing (for features) and opportunity sizing (for bets) are different things. This skill answers the bigger question: is this a real business opportunity worth pursuing — not just can a specific feature move a metric.

## Quick Start

```
/opportunity-sizing

Tell me:
1. What's the opportunity? (new market, new segment, new product line, new use case)
2. What do you already know? (existing data, research, anecdotes)
3. What's the decision this sizing needs to support? (fund, staff, kill, or continue)

Output: outputs/analyses/opportunity-[name]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Strategy | `context-library/strategy/*.md` | market, bet, initiative, segment | Existing strategic framing |
| Research | `context-library/research/*.md` | segment, customer type, use case | Qualitative customer signals |
| Metrics | `context-library/metrics/*.md` | revenue, ARR, conversion, LTV | Baseline unit economics |
| Competitive | `context-library/research/competitive*.md` | competitor, market share, TAM | Competitive market context |

**Cross-Skill Links:**
- Validated opportunity → `/prd-draft` or `/write-prod-strategy`
- Revenue model component → `/business-model-canvas`
- Need survey data to validate willingness to pay → `/survey-builder` (Van Westendorp)
- Pricing structure → `/pricing-analysis`

---

## Step 1: Define the Opportunity Clearly

Before sizing, make sure you know what you're sizing.

**Opportunity statement:**
> We believe there is a meaningful opportunity to [solve X problem] for [specific user type / market segment], who currently [workaround or go without], by [our proposed approach]. If true, this could represent [rough revenue range] in [timeframe].

Test your statement:
- Is the target customer specific enough? ("small businesses" is not specific; "bootstrapped SaaS founders with <10 employees" is)
- Is the problem specific enough? ("needs help with marketing" is not; "spends 4+ hours/week on manual reporting" is)
- Is the hypothesis falsifiable? Can research or data confirm or deny it?

---

## Step 2: Market Size (TAM / SAM / SOM)

### Top-Down (use when external market data exists)

```
TAM (Total Addressable Market):
Everyone who could theoretically need this
Source: [market research report, analyst estimate]
Estimate: $[X]B or [N]M users globally

SAM (Serviceable Addressable Market):
Who we can actually reach given our product, distribution, and geographic coverage
Calculation: TAM × [% that matches our ICP and channel reach]
Estimate: $[X]M or [N]K users

SOM (Serviceable Obtainable Market):
Realistic share in 3 years, given competitive dynamics and our advantages
Calculation: SAM × [realistic penetration rate, typically 1-10% for new markets]
Estimate: $[X]M ARR or [N] customers
```

### Bottom-Up (use when you know your unit economics)

```
Target customers: [N] businesses / users that match ICP
Conversion rate: [X]% will become paying customers (from current data or estimate)
ACV / ARPU: $[X] per customer per year
Revenue estimate: [N customers] × [ACV] = $[X]M ARR at full penetration

Confidence: [Low/Medium/High] — [what would change this estimate]
```

**Use both methods when possible. If they're far apart, investigate why.**

---

## Step 3: Problem Validation

Market size is necessary but not sufficient. Is the problem real and urgent enough?

| Signal | Strong | Weak |
|--------|--------|------|
| User frequency | "I deal with this every day" | "Occasionally, maybe once a month" |
| Workaround existence | Active, painful workarounds exist | No workaround needed |
| Spending behavior | Users pay something for partial solutions today | No current spend |
| Urgency language | "I would switch immediately" | "Nice to have if the price is right" |
| Failed alternatives | Have tried 2+ solutions that didn't work | Haven't really looked |

---

## Step 4: Willingness to Pay

The problem being real doesn't mean customers will pay what you need to charge.

**Validation approaches:**
- Van Westendorp survey (run via `/survey-builder`) — 4-question pricing sensitivity
- Existing proxy: What do customers pay for adjacent solutions?
- Sales test: Have sales or CS asked about this? What price points came up?
- Early LOIs or pre-sales: Any customers willing to commit before you build?

**Unit economics sanity check:**
```
Required price to make this viable: $[X]/month
Current comparable spend: $[Y]/month on alternatives
Price premium we're asking for: [X - Y] or [X%]
Justification for premium: [specific value advantage]
```

---

## Step 5: Competitive Assessment

Who else is pursuing this opportunity?

| Competitor | Stage | Approach | Our Differentiation |
|------------|-------|----------|---------------------|
| [Name] | [Funded/Launched/Dominant] | [How they solve it] | [Why ours is better for our target] |

**Competitive risk assessment:**
- Who's closest to solving this today?
- If we pursue this, who could respond and how fast?
- Is this a market that's winner-take-all, or is there room for multiple players?

---

## Step 6: Strategic Fit

Does this opportunity align with where we're going?

| Question | Assessment |
|----------|-----------|
| Does this extend or distract from our core? | [Extends / Parallel / Distraction] |
| Does this use our existing strengths? | [Yes — specifically: X / No] |
| Can we win here, or just participate? | [Win — because X / Participate / Unclear] |
| Does this require capabilities we don't have? | [No / Yes — gap: X] |

---

## Output Template

```markdown
# Opportunity Sizing: [Opportunity Name]

**Date:** [date]
**Decision needed:** [Fund / Staff / Kill / Continue investigation]
**Prepared by:** [name]

---

## Opportunity Statement

[1-2 sentences: who, what problem, what approach, rough potential]

---

## Market Size

| | Estimate | Confidence | Method |
|--|---------|-----------|--------|
| TAM | $[X]M | [L/M/H] | [Top-down / Bottom-up] |
| SAM | $[X]M | [L/M/H] | [How filtered] |
| SOM (3yr) | $[X]M | [L/M/H] | [Penetration assumption] |

**Key assumption:** [The biggest assumption in this sizing — what would make it 2x or 0.5x]

---

## Problem Validation

**Signal strength:** [Strong / Moderate / Weak]
**Evidence:** [Specific user quotes, research, behavioral data]
**Gaps:** [What we still need to validate]

---

## Willingness to Pay

**Target price point:** $[X]/[month or year]
**Evidence:** [Survey, sales intel, comparable spend]
**Risk:** [What might cause customers to balk]

---

## Competitive Landscape

[Summary: is this a crowded space or a whitespace opportunity?]
[2-3 rows from competitive table]

---

## Strategic Fit

[1-2 sentences on alignment to current strategy and where we can win]

---

## Investment Thesis

**Pursue if:** [Specific conditions that make this a good bet]
**Don't pursue if:** [Conditions that would kill the thesis]
**Next validation step:** [Most important thing to learn before committing]

---

## Recommendation

[Proceed / Investigate further / Deprioritize] — [1-2 sentence rationale]
```

---

## Output Quality Self-Check

- [ ] **Opportunity statement written:** Specific target customer, specific problem, specific approach
- [ ] **Market size has a method:** Not just a number — explains how it was calculated
- [ ] **Both TAM and SOM included:** Recognizing we can't capture the full market
- [ ] **Willingness to pay addressed:** Not assumed — validated or flagged as a gap
- [ ] **Competitive landscape honest:** Doesn't pretend the space is empty
- [ ] **Recommendation made:** Not just "more research needed" unless that's genuinely the answer
- [ ] **Output saved:** `outputs/analyses/opportunity-[name]-[date].md`
