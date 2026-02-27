---
name: win-loss-analysis
description: Synthesize sales win/loss data into product and positioning insights. Turns raw intel from sales calls, churn interviews, and customer feedback into roadmap implications and GTM adjustments.
disable-model-invocation: false
user-invocable: true
---

# /win-loss-analysis - Turn Sales Intel into Product Decisions

Systematically analyze why you win and lose deals. Turns raw sales intel into product gaps, positioning adjustments, and competitive insights — so revenue signals drive the roadmap.

## Quick Start

```
/win-loss-analysis

I'll help you synthesize win/loss data into clear product and positioning insights.

Tell me:
1. What's prompting this? (e.g., "we're losing 30% of enterprise deals", "we just had 5 churns in a row", "sales asked for a competitor battlecard")
2. What data do you have? (sales call notes, churn interviews, CRM data, CS feedback, or none yet — I'll help structure data collection)
3. What time period are we analyzing? (last quarter, last 6 months, specific cohort)

I'll check your existing context first, then build the analysis.

Output: outputs/win-loss/win-loss-analysis-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

**Automatic Context Checks:**
When this skill is invoked, immediately check:

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Sales Meeting Notes | `context-library/meetings/*.md` | "lost deal", "lost to", "competitor", "won", "closed", "objection", "pricing" | Win/loss patterns, competitor mentions, objections |
| Churn Research | `context-library/research/*.md` | "churn", "cancel", "switched", "left", "why did you leave" | Churn reasons, competitor switches |
| Competitor Analysis | `context-library/research/competitive-*.md` | competitor name, feature comparison | Known competitive landscape |
| Metrics | `context-library/metrics/*.md` | win rate, close rate, churn rate, NRR | Baseline win/loss percentages |
| PRDs | `context-library/prds/*.md` | "competitive", "objection", "sales" | Features built to address sales gaps |
| Strategy | `context-library/strategy/*.md` | "positioning", "ICP", "segments" | Target customer and positioning context |

**Context Priority:**
1. Internal sales notes and churn interviews FIRST (most honest signal)
2. Existing competitive analysis SECOND
3. Metrics/baseline THIRD
4. External web research LAST (to fill gaps only)

**Cross-Skill Links:**
- Competitor keeps appearing → Link to `/competitor-analysis`
- Churn patterns emerge → Link to `/retention-analysis`
- Product gaps identified → Link to `/prd-draft` to spec the fix
- Positioning issues surface → Link to `/write-prod-strategy`

---

## Step 0: What Intel Already Exists

Before diving into analysis, let me check what's already captured...

**Checking:**
- `context-library/meetings/` for sales call notes, CS syncs, and debrief notes
- `context-library/research/` for churn interviews and win interviews
- `context-library/metrics/` for win rate and churn rate data
- `context-library/research/competitive-*.md` for competitive context

**Based on what I find, I'll show you:**

### Internal Intel Summary

**From Sales Meeting Notes:**
- [List patterns: competitors mentioned, objections raised, deal-loss reasons]

**From Churn Research:**
- [Churn patterns, specific reasons cited, competitor switches]

**From Metrics:**
- [Current win rate, recent trend, churn rate context]

**Gaps in Current Intel:**
- [Missing data: do we have win interviews? loss interviews? competitor-specific data?]

---

## Step 1: Data Collection (If You Don't Have Enough)

Before analysis, you need raw data. Here are the three most valuable sources:

### Win Interviews
Talk to customers who chose you over alternatives. Best done within 2 weeks of close.

**Key questions:**
1. "Walk me through your evaluation process. Who else did you seriously consider?"
2. "What was the moment you decided we were the right fit?"
3. "What almost made you choose a competitor instead?"
4. "What would you tell a colleague who is evaluating similar solutions?"
5. "What would make you leave us for a competitor?"

### Loss Interviews
Talk to prospects who chose a competitor. Warm introductions from sales rep work best.

**Key questions:**
1. "We'd love to understand your evaluation. Who did you end up going with and why?"
2. "What did [competitor] have that we didn't?"
3. "Was there anything we could have done to win your business?"
4. "What did our solution do well, even though you didn't choose us?"
5. "What advice would you give us to improve?"

### Sales Team Debrief Template
Use this consistently after every lost deal:

```markdown
## Deal Debrief: [Account Name] - [Won/Lost] - [Date]

**Decision made:** [Won / Lost to / No decision]
**Lost to (if applicable):** [Competitor or "status quo"]

**Why we lost (sales rep's view):**
- [Reason 1]
- [Reason 2]

**Prospect's stated reason:**
- [What they told us]

**Product gaps mentioned:**
- [Specific features or capabilities cited]

**Pricing feedback:**
- [Too expensive / competitive / too cheap]
- [Specific pricing objections]

**Relationship/Process factors:**
- [Sales cycle too long / wrong champion / evaluation criteria mismatch]

**What we did well:**
- [Strengths that came up during eval]

**Rep recommendation:**
- [What product/sales/marketing should do differently]
```

---

## Analysis Framework

Once data is collected (or using existing data), structure the analysis into four buckets:

### Bucket 1: Why We Win

Look for patterns across wins:
- Which features or capabilities are mentioned most as differentiators?
- Which segments do we win most reliably? (by size, industry, use case)
- What does our best champion look like? (role, level, buying motivation)
- Which competitors do we beat most consistently and why?

**Output:** Win profile — a summary of when/why/who we win, used to refine ICP and messaging.

### Bucket 2: Why We Lose

Look for patterns across losses:
- What product gaps appear most frequently?
- Which competitor beats us most often? In which segments?
- Are losses concentrated in a specific deal size, industry, or use case?
- Is the issue price, product, process, or relationship?

**Loss pattern categorization:**
- **Product gap:** They have a feature we don't
- **Positioning gap:** We have the capability but didn't communicate it clearly
- **Process gap:** Our sales cycle, onboarding, or contract was too painful
- **Relationship gap:** We didn't have the right champion or exec sponsor
- **Price gap:** Our pricing was genuinely uncompetitive (different from "they asked for a discount")

### Bucket 3: Competitive Patterns

From the win/loss data, surface competitive intelligence:
- Which competitors appear most in loss notes?
- What are their consistent selling points vs. us?
- Where do customers say competitors are better?
- Are there deals where we consistently don't even make it to the final round?

### Bucket 4: Product Gaps (Roadmap Implications)

Translate loss reasons into product requirements:
- Which product gaps appear in 3+ losses in the last quarter? (pattern, not noise)
- Which gaps are blocking deals in our most strategic segments?
- Which gaps would close the most dollar volume if fixed?

**Gap prioritization matrix:**

| Gap | Frequency (deals lost) | Revenue at stake | Build complexity | Priority |
|-----|------------------------|------------------|------------------|----------|
| [Gap 1] | [N deals] | [$X] | [H/M/L] | [High/Med/Low] |
| [Gap 2] | [N deals] | [$X] | [H/M/L] | [High/Med/Low] |

---

## Output Template

```markdown
# Win/Loss Analysis: [Time Period]

**Date:** [date]
**Period analyzed:** [Q1 2026 / Last 6 months / etc.]
**Data sources:** [X win interviews, Y loss interviews, Z sales debriefs, churn notes]
**Win rate baseline:** [X%] → [Y%] (trend: improving / declining / stable)

---

## Executive Summary

[2-3 sentences: what's the key finding and what should we do about it]

---

## Why We Win

**Top 3 win drivers:**
1. [Driver] - Mentioned in [N] of [X] win interviews
2. [Driver] - Mentioned in [N] of [X] win interviews
3. [Driver] - Mentioned in [N] of [X] win interviews

**Segments where we win most reliably:**
- [Segment]: Win rate [X%], Key differentiator: [Y]
- [Segment]: Win rate [X%], Key differentiator: [Y]

**Our best champion profile:**
- Role: [job title]
- Motivation: [what they're trying to accomplish]
- Key concern: [what they evaluate most carefully]

---

## Why We Lose

**Top 3 loss reasons:**
1. [Reason] - [N] deals, ~$[X] revenue (Category: [Product / Positioning / Process / Relationship / Price])
2. [Reason] - [N] deals, ~$[X] revenue
3. [Reason] - [N] deals, ~$[X] revenue

**Segments where we struggle most:**
- [Segment]: Win rate [X%], Primary loss reason: [Y]

---

## Competitive Patterns

| Competitor | Times appeared in losses | Primary advantage they cite | Our counter |
|------------|--------------------------|----------------------------|-------------|
| [Name] | [N deals] | [Their pitch] | [Our response / gap to close] |

---

## Roadmap Implications

### Priority Product Gaps to Close

| Gap | Evidence | Revenue at stake | Recommended action |
|-----|----------|------------------|-------------------|
| [Gap 1] | "[Quote from loss interview]" | $[X] in lost deals | [Build / Reposition / Deprioritize] |
| [Gap 2] | "[Quote]" | $[X] | [Action] |

### Positioning Adjustments

- [What messaging isn't landing and what to say instead]
- [Which capabilities we have but aren't communicating effectively]

### Process Improvements

- [Sales cycle, onboarding, or contract issues to fix]

---

## Next Steps

- [ ] [Action 1 with owner and date]
- [ ] [Action 2 with owner and date]
- [ ] Share with: [sales team / product / marketing]
- [ ] Re-analyze in: [next quarter / after roadmap change]
```

---

## Output Integration

### Where Files Go

**Win/loss analyses:**
- Active work: `outputs/win-loss/win-loss-analysis-[date].md`
- When finalized: Move relevant competitive intel to `context-library/research/competitive-*.md`
- Share roadmap implications with relevant PRDs

### Cross-Skill Integration

**Feeds into:**
- `/competitor-analysis` - Win/loss data enriches competitive picture
- `/prd-draft` - Product gaps become PRD inputs; link to evidence from loss interviews
- `/write-prod-strategy` - Win patterns inform ICP; loss patterns inform positioning
- `/slack-message` - Summary for sales team or leadership
- `/launch-checklist` - Positioning gaps identified here inform launch messaging

**Pulls from:**
- `context-library/meetings/` - Sales notes and CS syncs
- `context-library/research/` - Churn and customer interviews
- `context-library/metrics/` - Win rate and churn data
- `/retention-analysis` - Churn patterns complement loss patterns

---

## Common Mistakes to Avoid

**Mistake 1: Trusting sales reps' stated reasons without digging deeper**
Sales reps often say "pricing" when the real issue is value perception. Dig into: "Was it truly price, or did they not see enough value to justify the price?"

**Mistake 2: Analyzing wins and losses in isolation**
The most interesting finding is often the contrast. "We win when X is the champion and lose when Y is the champion" is more actionable than "we lose on feature Z."

**Mistake 3: Treating every loss as a product gap**
Some losses are the right outcome. If a deal required 3 major features you're not building, walking away was correct. Focus analysis on the deals you should have won.

**Mistake 4: Quarterly analysis only**
Monthly patterns catch trends faster. Set up the debrief template with sales so data flows continuously.

---

## Output Quality Self-Check

Before delivering the win/loss analysis, verify:

- [ ] **Internal context checked first:** Reviewed meeting notes, research, and metrics before asking for input
- [ ] **Evidence-based claims:** Every pattern referenced with specific number of deals or direct quotes
- [ ] **Loss reasons categorized:** Each loss reason classified as Product / Positioning / Process / Relationship / Price
- [ ] **Revenue quantified:** Dollar impact estimated for top product gaps (even rough estimates are better than none)
- [ ] **Competitor section completed:** Named competitors with specific evidence of why they're winning
- [ ] **Roadmap implications are actionable:** Not "improve X" but "build Y feature" or "reposition Z capability"
- [ ] **Next steps have owners:** Actions assigned with names, not just "sales team should..."
- [ ] **Output file saved:** Saved to `outputs/win-loss/win-loss-analysis-[date].md`
