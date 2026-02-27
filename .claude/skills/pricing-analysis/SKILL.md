---
name: pricing-analysis
description: Design and analyze pricing strategy for products and features. Covers pricing models (freemium, subscription, usage-based, tiered), research methods (Van Westendorp, conjoint, competitive benchmarking), and packaging decisions.
disable-model-invocation: false
user-invocable: true
---

# /pricing-analysis - Get Pricing Right Before You Ship

Pricing is product strategy made visible. Every pricing decision communicates value, targets a segment, and shapes the business model.

## Quick Start

```
/pricing-analysis

Modes:
--research    Design a pricing study (survey or analysis)
--model       Design or evaluate a pricing model
--packaging   Structure tiers, plans, or add-ons
--benchmark   Competitive pricing comparison

Tell me:
1. Mode (or describe your pricing question)
2. What you already know about customer willingness to pay
3. Current pricing model (if any)
4. What decision this analysis needs to support

Output: outputs/analyses/pricing-[product]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Strategy | `context-library/strategy/*.md` | pricing, revenue, monetization, tier | Existing pricing rationale |
| Research | `context-library/research/*.md` | price, willingness to pay, cost, budget | Customer WTP signals |
| Metrics | `context-library/metrics/*.md` | ARR, ACV, ARPU, conversion, LTV | Current unit economics |
| Competitive | `context-library/research/competitive*.md` | pricing, plan, tier, free, paid | Competitor pricing intel |

**Cross-Skill Links:**
- Quantify WTP → `/survey-builder` (Van Westendorp 4-question pricing survey)
- Market sizing for revenue model → `/opportunity-sizing`
- Package features into tiers → `/prd-lite` for each tier definition
- Business model implications → `/business-model-canvas`

---

## Mode: --research (Pricing Research Design)

### Van Westendorp Price Sensitivity Meter

The fastest qualitative WTP research method. 4 questions asked to your target customers:

```
Q1: "At what price would [product] be so cheap that you'd question its quality?"
   → Too cheap threshold

Q2: "At what price would [product] be a bargain — great value for the money?"
   → Acceptable cheap threshold

Q3: "At what price would [product] start to seem expensive, though you'd still consider it?"
   → Acceptable expensive threshold

Q4: "At what price would [product] be so expensive you'd refuse to buy it?"
   → Too expensive threshold
```

**Interpretation:**
- Acceptable price range: Between Q2 and Q3 for the majority of respondents
- Optimal price point: Where "too cheap" and "too expensive" curves cross
- Indifference price: Where "bargain" and "expensive" curves cross (most won't notice price change here)

**Sample size:** 100-300 for B2C, 30-80 for B2B (harder to recruit)

Run this survey via `/survey-builder` — the Van Westendorp template is built in.

### Additional Research Methods

**Direct WTP question** (simple, slightly less reliable):
> "If this feature/product were available as a standalone, what would you expect to pay per month?"

**Anchoring test** (for existing products):
- Show different price points to different user segments
- Measure conversion rate and churn by price cohort
- Only valid with sufficient traffic for a clean experiment

**Conjoint analysis** (for complex feature/price tradeoffs):
- Present combinations of features and prices
- Identify which features drive the most value
- Requires a research tool (Conjointly, SurveyMonkey conjoint)

---

## Mode: --model (Pricing Model Selection)

### Pricing Model Comparison

| Model | Best for | Revenue predictability | Aligns with customer value? |
|-------|----------|----------------------|----------------------------|
| **Flat-rate subscription** | Simple product, known use case | High | If usage is consistent |
| **Per-seat / per-user** | Team collaboration tools | High | If teams grow with value |
| **Usage-based** | API, infrastructure, data | Variable | High — pay for what you use |
| **Tiered plans** | Broad market with different segments | Medium-High | If segments have distinct needs |
| **Freemium** | High volume B2C or PLG B2B | Low until conversion | Depends on free-to-paid conversion |
| **Outcome-based** | Enterprise, professional services | Low | Highest — share upside |

### Pricing Model Decision Framework

Answer these questions to select the right model:

```
1. Is usage variable across customers?
   - Yes: consider usage-based or tiered
   - No: consider flat-rate or per-seat

2. Who buys vs. who uses?
   - Buyer = user: consider per-seat or flat-rate
   - Buyer ≠ user: consider usage-based or outcome-based

3. What's our growth motion?
   - PLG / viral: freemium or usage-based (land and expand)
   - Sales-led: flat-rate or tiered with negotiated enterprise

4. Can we measure value delivered?
   - Yes, clearly: outcome-based is possible
   - No: subscription with proxy metrics

5. What do competitors use?
   - Same model = table stakes
   - Different model = potential differentiation if better aligned to customer value
```

---

## Mode: --packaging (Tier and Plan Design)

Good packaging makes the right tier obvious for each customer segment.

### Tier Design Principles

1. **Each tier targets a different buyer persona** — don't design tiers by feature count, design by customer job
2. **Middle tier is the hero** — most customers should land here; it has the highest margin
3. **Free tier drives volume, not revenue** — if your free tier is too good, paid doesn't convert
4. **Enterprise tier unlocks trust** — security, SSO, contracts, dedicated support, custom terms

### Tier Template

```
## Free / Starter (volume driver)
Target: [Persona — individual / small team / evaluator]
Job: Let them experience core value before buying
Limits: [What's capped — seats, records, actions, features]
Goal: Conversion to [Pro] within [N] days

## Pro / Growth (hero tier)
Target: [Persona — the main buying persona]
Price: $[X]/[month/seat/usage unit]
Value prop: [What this tier uniquely enables vs Free]
Key features: [3-5 features that define this tier]
Goal: [X]% of revenue, [Y]% of paying customers

## Business / Scale (expansion driver)
Target: [Persona — growing team or more complex use case]
Price: $[X]/[month/seat]
Key features: [Advanced features that justify the jump]
Goal: Expansion revenue from Pro customers

## Enterprise (trust and compliance)
Target: [Persona — large org, procurement-led, security review]
Price: Custom (contact sales)
Key features: SSO, audit logs, SLA, dedicated CSM, custom contract
Goal: High ACV, long-term retention
```

### Add-On vs. Core Feature Decision

Add-on if: Only a subset of customers need it, it requires extra cost to deliver, or it's a natural upsell after initial purchase.

Core feature if: It's part of the core value loop, most customers need it, or limiting it creates too much friction.

---

## Mode: --benchmark (Competitive Pricing Analysis)

### Competitive Pricing Grid

| Competitor | Entry plan | Mid plan | Enterprise | Model | Notable |
|------------|-----------|---------|-----------|-------|---------|
| [Name] | $[X]/mo | $[X]/mo | Custom | [Model] | [Key differentiator] |

### Competitive Positioning Options

```
Premium (charge more, justify with quality/support/outcomes)
  Condition: Strong brand, measurable outcomes, or switching costs
  Risk: Lose price-sensitive deals

Parity (match market)
  Condition: Competing on product, not price
  Risk: Race to the bottom, no pricing power

Penetration (charge less to win share)
  Condition: New market entrant, trying to displace incumbent
  Risk: Sets expectations low, hard to raise later

Value-based (price to outcome, not feature set)
  Condition: Can clearly quantify customer ROI
  Risk: Requires sophisticated customer understanding
```

---

## Output Template

```markdown
# Pricing Analysis: [Product / Feature]

**Date:** [date]
**Decision:** [Pricing model change / New tier / Research design / Benchmark]

---

## Current State

**Current pricing:** $[X]/[unit] — [model]
**Conversion rate:** [X]% free to paid (if freemium)
**Average ACV:** $[X]
**Price-related churn signal:** [Any exits attributed to price? Quote examples]

---

## Research Findings

**Method:** [Van Westendorp / Direct WTP / Competitive benchmark]
**Acceptable price range:** $[X] – $[Y]/month
**Optimal price point:** $[X]
**Key finding:** [Most important pricing insight]

---

## Recommendation

**Recommended model/price:** [Specific recommendation]
**Rationale:** [Why this model and price point]
**Expected impact:** [Revenue, conversion, or expansion impact]
**Risk:** [What could go wrong]

---

## Packaging (if applicable)

| Tier | Price | Target | Key features |
|------|-------|--------|-------------|
| Free | $0 | [persona] | [2-3 features] |
| Pro | $[X]/mo | [persona] | [3-5 features] |
| Enterprise | Custom | [persona] | [4-6 features] |

---

## Open Questions

- [What still needs validation before changing pricing]
- [Experiment needed to confirm recommendation]
```

---

## Output Quality Self-Check

- [ ] **Research method matched to question:** Van Westendorp for WTP, benchmark for positioning, conjoint for feature value
- [ ] **Pricing model justified:** Not just "subscription" but why subscription vs. alternatives
- [ ] **Competitive context included:** Price recommendation is relative to the market
- [ ] **Packaging targets personas:** Each tier has a named buyer persona, not just a feature list
- [ ] **Risk acknowledged:** What could make this recommendation wrong
- [ ] **Next validation step named:** Experiment, survey, or sales test to confirm
- [ ] **Output saved:** `outputs/analyses/pricing-[product]-[date].md`
