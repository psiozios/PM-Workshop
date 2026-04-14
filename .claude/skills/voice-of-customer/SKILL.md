---
name: voice-of-customer
description: Aggregate customer feedback into Voice of Customer (VoC) reports. Cross-channel synthesis from support tickets, sales calls, user interviews, NPS comments, reviews, and social feedback. Surfaces patterns, themes, and product implications.
disable-model-invocation: false
user-invocable: true
---

# /voice-of-customer - Aggregate Customer Feedback into Insight

A VoC report synthesizes what customers are actually saying — across every channel — into patterns the product team can act on. Not another list of complaints. A structured view of customer reality.

## Quick Start

```
/voice-of-customer

I'll help you synthesize cross-channel customer feedback into a structured VoC report.

Tell me:
1. Time period (last month, last quarter, last 6 months)
2. What feedback sources do you have? (support tickets, NPS comments, sales call notes, interviews, G2/Capterra reviews, social, CS escalations)
3. Any specific product area or theme you want to focus on? (or "all" for a broad synthesis)
4. Who's the audience for this report? (product team, leadership, exec, or all-hands)

Paste your raw feedback or I'll check your context library first.

Output: outputs/research-synthesis/voc-report-[period]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

**Automatic Context Checks:**

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| User Research | `context-library/research/*.md` | interview, user, customer, feedback | Qualitative themes from interviews |
| Meeting Notes | `context-library/meetings/*.md` | customer, support, CS, sales, escalation | CS/sales feedback captured in meetings |
| Past VoC | `context-library/research/voc-*.md` | theme, complaint, request, praise | Prior themes to track change over time |
| Metrics | `context-library/metrics/*.md` | NPS, CSAT, support volume | Quantitative signal to anchor qualitative |

**Cross-Skill Links:**
- Themes surface product gaps → `/prd-lite` or `/prd-draft`
- Churn signal in feedback → `/retention-analysis`
- Competitive mentions → `/competitor-analysis`
- Activation friction patterns → `/activation-analysis`
- Feature requests → `/feature-request-analysis`

---

## VoC Synthesis Framework

### Step 1: Categorize Feedback by Channel

Different channels have different biases. Account for them:

| Channel | Signal strength | Common bias |
|---------|----------------|-------------|
| Support tickets | High volume, specific | Selection bias toward frustrated users |
| NPS verbatims | Structured, comparable | Extremes only (promoters and detractors speak up) |
| User interviews | Deep, contextual | Small sample, may not represent majority |
| Sales call notes | Decision-moment signal | Skews toward deal-blockers and purchase reasons |
| G2/Capterra reviews | Public, third-party | Mix of genuine users and incentivized reviews |
| Social/community | Unfiltered emotion | Amplifies frustration, undercounts satisfaction |
| CS escalations | High urgency, specific | Most severe issues, not representative of average |

### Step 2: Code and Cluster

For each piece of feedback, tag with:
- **Type:** Bug report / Feature request / Praise / Confusion / Comparison to competitor
- **Area:** [Product area: onboarding / billing / core feature / integrations / etc.]
- **Sentiment:** Positive / Negative / Neutral
- **Urgency signal:** Mention of churn, cancellation, blocking use case, competitor switch

Then cluster by theme — not by channel. The goal is to see what users care about, not what each channel produces.

### Step 3: Identify Patterns vs. Noise

**Pattern:** Same theme from 3+ independent sources, or same theme from different channels (support + interviews + sales = signal)

**Noise:** Single data point, cannot be corroborated, or contradicted by other data

**Frequency vs. intensity distinction:**
- High frequency but low intensity: Annoyance. Worth fixing if cheap. Don't overweight.
- Low frequency but high intensity: Potential deal-breaker for a segment. Investigate.
- High frequency + high intensity: Urgent. This is what churn papers are written about.

### Step 4: Surface the Implications

For each major theme, answer:
- What is the user trying to accomplish? (job to be done)
- What's getting in their way? (functional or emotional friction)
- What does this tell us to build, change, or communicate differently?
- What segment of users is affected? (all users / power users / new users / enterprise / SMB)

---

## VoC Report Template

```markdown
# Voice of Customer Report: [Period]

**Date:** [date]
**Period covered:** [date range]
**Sources analyzed:** [list: support tickets (N), NPS comments (N), interviews (N), sales calls (N), etc.]
**Audience:** [product team / leadership / exec]

---

## Executive Summary

[3-5 sentences. What's the biggest takeaway? What's new since last period? What needs a decision?]

**Top signal this period:** "[Most impactful direct quote]" — [attribution: new user / churned customer / enterprise power user]

---

## Theme 1: [Theme Name]

**Frequency:** [High / Medium / Low] — mentioned in [N] of [X] feedback items
**Intensity:** [Critical (blocking) / High (frustrating) / Medium (annoying) / Low (nice-to-have)]
**Sources:** [Support (N%) / Interviews (N%) / Sales (N%) / Reviews (N%)]

**What customers are saying:**
- "[Direct quote 1]" — [Channel, approximate date or cohort]
- "[Direct quote 2]" — [Channel]
- "[Direct quote 3]" — [Channel]

**Pattern analysis:**
[What's the underlying job or unmet need? Is this getting better or worse over time?]

**Segments most affected:**
[Which user type, tier, or use case generates most of this signal?]

**Product implication:**
[What should we do about it? Build / Fix / Communicate differently / Investigate further]

---

[Repeat for Themes 2, 3, 4...]

---

## Praise Themes (What's Working)

Don't skip this. Understanding why users love us is as important as knowing what frustrates them.

| Theme | Frequency | Example quote | Implication |
|-------|-----------|---------------|-------------|
| [Theme] | [High/Med/Low] | "[quote]" | [Protect this / Expand this] |

---

## Competitive Mentions

| Competitor | Context | Sentiment | Frequency |
|------------|---------|-----------|-----------|
| [Name] | [Why they came up] | [Positive / Negative] | [N mentions] |

**Implication:** [What does this competitive signal tell us?]

---

## Churn Signal

Any mentions of cancellation intent, switching, or active churn in this period:

- "[Quote]" — [context]
- "[Quote]" — [context]

**Pattern:** [What's driving churn risk? What's the underlying job not being done?]

---

## Product Implications (Prioritized)

| Theme | Urgency | Effort (rough) | Recommended action |
|-------|---------|----------------|-------------------|
| [Theme 1] | Critical | Small | Fix in current sprint |
| [Theme 2] | High | Medium | Add to backlog as P1 |
| [Theme 3] | Medium | Large | Investigate before committing |

---

## What's Changed Since Last Period

[If prior VoC exists: What's new? What improved? What got worse? What's consistent?]

---

## Appendix: Raw Theme Counts

| Theme | Total mentions | Support | Interviews | Sales | Reviews |
|-------|---------------|---------|------------|-------|---------|
| [Theme] | [N] | [N] | [N] | [N] | [N] |
```

---

## Single-Source VoC (Lite Mode)

If you only have one source (e.g., just NPS verbatims or just support tickets), I'll still run the synthesis but note the channel limitations:

- Flag single-channel bias explicitly
- Recommend 1-2 additional channels to validate themes
- Note which themes need corroboration before acting

---

## Output Integration

**Files:** `outputs/research-synthesis/voc-report-[period]-[date].md`
- After finalizing: Move to `context-library/research/` for longitudinal tracking

**Cross-Skill Integration:**
- Themes → `/feature-request-analysis` for deeper prioritization
- Churn signal → `/retention-analysis`
- Competitive mentions → `/competitor-analysis`
- Product gaps → `/prd-lite` for quick proposals
- New themes → Update `/user-research-synthesis` with emerging patterns

---

## Output Quality Self-Check

- [ ] **Multiple channels synthesized:** Not just one source — cross-channel validation noted
- [ ] **Pattern vs. noise distinguished:** Themes backed by 3+ sources or corroborated across channels
- [ ] **Direct quotes included:** Every major theme has at least 2 verbatim customer quotes
- [ ] **Segment specificity:** Each theme identifies which user segment is most affected
- [ ] **Praise captured:** What's working is documented alongside what's broken
- [ ] **Product implications actionable:** "Build / Fix / Communicate / Investigate" — not "consider improving"
- [ ] **Churn signal called out:** Any cancellation or switching mentions flagged explicitly
- [ ] **Output saved:** `outputs/research-synthesis/voc-report-[period]-[date].md`

---

## Second Brain Integration

**Before aggregating:** query the `customer-insights` focus area for prior VoC themes. The goal is to compare this period against the baseline — what's new, what's persistent, what's faded. Pull prior theme counts and sentiment.

**At the end of the run, offer: "File to Second Brain? (y/n)"**

If yes, ingest into `customer-insights`:
- Consolidated themes with source counts (e.g., "Billing confusion — 23 mentions across support, sales, NPS")
- Trends vs. last period (new themes, intensifying themes, fading themes)
- Churn/cancellation signals
- Competitor mentions (also cross-file into `competitive-intelligence`)
- Praise worth preserving (what's working)

Invoke `/second-brain ingest` with the VoC report as the source. Competitor mentions get a second ingest into `competitive-intelligence`.

VoC is inherently comparative — a report without a baseline is half a report. The brain is the baseline.
