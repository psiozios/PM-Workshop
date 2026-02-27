---
name: survey-builder
description: Design product surveys using validated PM research methodologies. Covers PMF, NPS, CSAT, CES, JTBD, and Van Westendorp pricing surveys with exact question wording and interpretation guides.
disable-model-invocation: false
user-invocable: true
---

# /survey-builder - Design Product Surveys

Build research-grade product surveys using established PM methodologies. Every question template comes from validated frameworks, not gut instinct.

## Quick Start

```
/survey-builder

Planning a survey? I'll help you pick the right methodology and build it correctly.

Tell me:
1. What are you trying to measure? (e.g., "whether we've hit product-market fit", "how easy our checkout is", "pricing sensitivity for our new plan")
2. Who will take this survey? (existing users, churned users, prospects, a specific segment)
3. How many responses do you expect? (helps calibrate methodology choice)

I'll recommend the right survey type, give you the exact questions with correct wording, and include an interpretation guide so you know what to do with the results.

Output: outputs/surveys/survey-[type]-[topic]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

**Automatic Context Checks:**
When this skill is invoked, immediately check:

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Existing Research | `context-library/research/*.md` | survey, NPS, PMF, satisfaction | Past survey results to avoid duplication |
| Metrics | `context-library/metrics/*.md` | NPS score, CSAT, satisfaction baseline | Current benchmark to compare against |
| PRDs | `context-library/prds/*.md` | feature name, problem statement | Feature context for CES or JTBD surveys |
| Strategy | `context-library/strategy/*.md` | pricing, segments, target users | Segment targeting and pricing context |

**Context Priority:**
1. Understand what's already been measured FIRST (avoid re-running identical surveys)
2. Match survey type to the specific measurement goal SECOND
3. Customize question wording for the product/feature THIRD

**Cross-Skill Links:**
- After survey runs → Link to `/user-research-synthesis` to synthesize results
- Survey reveals activation issue → Link to `/activation-analysis`
- Survey reveals churn risk → Link to `/retention-analysis`
- PMF survey results → Link to `/write-prod-strategy` if results inform direction

---

## Step 0: Understanding What You Need to Measure

Before picking a survey type, let me check what you already know...

**Checking:**
- `context-library/metrics/` for existing survey baselines (NPS, CSAT)
- `context-library/research/` for past surveys on this topic
- `context-library/prds/` for the feature or problem context (if relevant)

**Based on what I find, I'll surface:**

### What We Already Know
- [Prior survey results on this topic, if any]
- [Current NPS/CSAT benchmarks, if tracked]
- [Segments already measured vs. segments with gaps]

### PM Diagnosis Questions

1. **Measurement goal:** Are you validating PMF, measuring satisfaction, testing pricing, or understanding job completion?
2. **Timing:** Is this pre-launch, post-launch, or ongoing tracking?
3. **Audience:** Current users, churned users, prospects, or a specific cohort?
4. **Action trigger:** What decision will these results inform?

---

## Survey Type Selector

Choose based on what you need to measure:

| Goal | Survey Type | Key Signal |
|------|-------------|------------|
| "Do we have product-market fit?" | PMF Survey | 40%+ "very disappointed" = strong PMF |
| "How loyal are our users?" | NPS | Promoters (9-10) minus Detractors (0-6) |
| "How satisfied are users overall?" | CSAT | Average satisfaction score |
| "How easy was this to use?" | CES (Customer Effort Score) | Lower effort = better experience |
| "Are users getting value from this job step?" | JTBD Satisfaction | Gap between importance and satisfaction |
| "What should we charge for this?" | Van Westendorp Pricing | Acceptable price range |

---

## The Six Survey Templates

### 1. PMF Survey (Product-Market Fit)

**Use when:** You want to know if you've built something people would miss.

**Question:**

> How would you feel if you could no longer use [product]?
> - Very disappointed
> - Somewhat disappointed
> - Not disappointed

**Interpretation guide:**
- 40%+ "very disappointed" → Strong PMF signal. Move fast on growth.
- 25-40% "very disappointed" → Partial PMF. Experiment with messaging or product tweaks.
- Under 25% "very disappointed" → Weak PMF. Significant product development needed before scaling.

**Pro tip:** Follow up with "Why?" as an open text field for each response. The qualitative answers reveal what to say in marketing (from the "very disappointed" group) and what to fix (from the "not disappointed" group).

**Sample size:** 40+ responses minimum for statistical reliability. 100+ for segment analysis.

---

### 2. NPS (Net Promoter Score)

**Use when:** You want a single loyalty metric to track over time.

**Question:**

> On a scale of 0-10, how likely are you to recommend [product/company] to a friend or colleague?
> *(Numeric scale: 0-10)*

**Interpretation guide:**
- Promoters: 9-10 (love it, will spread the word)
- Passives: 7-8 (satisfied but not enthusiastic)
- Detractors: 0-6 (unhappy, risk of churn and negative word-of-mouth)
- NPS Score = % Promoters - % Detractors

**Benchmarks by industry:**
- SaaS: 30+ is good, 50+ is excellent
- Consumer apps: 40+ is good, 60+ is excellent
- B2B enterprise: 20+ is good, 40+ is excellent

**Warning:** NPS measures attitude, not behavior. A high NPS doesn't guarantee retention. Pair with churn data.

**Follow-up:** "What is the primary reason for your score?" (open text). This is where the actionable insight lives.

**Sample size:** 100+ for reliable tracking. Run quarterly or after major releases.

---

### 3. CSAT (Customer Satisfaction Score)

**Use when:** You want to measure satisfaction with a specific interaction, feature, or the product overall.

**CSAT offers two question variants:**

**Overall satisfaction:**
> Please rate your overall satisfaction with [product/feature].
> - Very satisfied
> - Somewhat satisfied
> - Neither satisfied nor dissatisfied
> - Somewhat dissatisfied
> - Very dissatisfied
> - [Non-applicable] *(include if users might not have used the feature)*

**Expectation alignment:**
> How did [product/feature] meet your expectations?
> - Extremely well
> - Very well
> - Moderately well
> - Slightly well
> - Not at all well
> - [Non-applicable]

**Interpretation guide:**
- CSAT Score = (# Satisfied responses / Total responses) × 100
- 80%+ CSAT is generally strong
- Compare trends over time, not just absolute scores

**When to add a follow-up:** "How can we improve?" after any response below "somewhat satisfied."

---

### 4. CES (Customer Effort Score)

**Use when:** You want to know how easy it is to use a specific feature or complete a specific task.

**CES offers two question variants:**

**Agreement-based (recommended for feature satisfaction):**
> To what extent do you agree or disagree with the following statement? [Product/feature] made it easy for me to [complete task/solve problem].
> - Strongly agree
> - Somewhat agree
> - Neither agree nor disagree
> - Somewhat disagree
> - Strongly disagree

**Effort-based (recommended for task completion):**
> When using [product/feature] to [complete task/solve problem], how much effort did you personally have to put forth?
> - Very low effort
> - Low effort
> - Moderate effort
> - High effort
> - Very high effort

**Interpretation guide:**
- High "strongly agree" or "very low effort" = frictionless experience
- High "strongly disagree" or "high effort" = UX problem worth fixing
- CES is the strongest predictor of customer loyalty for service interactions

**Note:** Don't add "non-applicable" to CES. You're asking about a task you know they performed.

---

### 5. JTBD Survey (Jobs-to-be-Done Satisfaction)

**Use when:** You want to understand how well your product helps users complete specific jobs, and whether those jobs matter.

**Two-question set (always use together):**

**Importance question:**
> When [job step], how important is it to you that you can [desired outcome]?
> - Not at all important
> - Somewhat important
> - Important
> - Very important
> - Extremely important
> - [Non-applicable] *(include if not all users perform this job)*

**Satisfaction question:**
> When [job step], how satisfied are you with your ability to [desired outcome]?
> - Not at all satisfied
> - Somewhat satisfied
> - Satisfied
> - Very satisfied
> - Extremely satisfied
> - [Non-applicable]

**Interpretation guide:**
- High importance + low satisfaction = underserved opportunity (the sweet spot for new features)
- High importance + high satisfaction = table stakes (don't break this)
- Low importance + high satisfaction = over-engineered (deprioritize investment)
- Low importance + low satisfaction = ignore

**Opportunity Score formula (Dan Olsen):**
Opportunity = Importance + (Importance - Satisfaction)

Higher score = bigger opportunity to address.

---

### 6. Van Westendorp Pricing Survey

**Use when:** You need to find the acceptable price range for a new product, tier, or feature.

**Four-question set (always use all four):**

> 1. At what price would you consider [product/feature] so expensive that you would not consider buying it?
> *(Provide a numeric scale with 5-7 price point options based on preliminary research)*

> 2. At what price would you consider [product/feature] to be priced so low that you would feel the quality couldn't be very good?

> 3. At what price would you consider [product/feature] starting to get expensive, so that it is not out of the question, but you would have to give some thought to buying it?

> 4. At what price would you consider [product/feature] a bargain — a great buy for the money?

**Interpretation guide:**
- Plot the four price curves on a chart
- "Acceptable range" = where "too cheap" and "too expensive" curves intersect
- "Optimal price point" = where "getting expensive" and "bargain" curves intersect
- Use this range to inform pricing tiers, not a single price point

**Note:** Run qualitative research before this survey to establish reasonable price anchor points for the scale options.

---

## Survey Design Best Practices

**Scale consistency:** Use 5-point scales for almost everything. 11-point scales (NPS) are the exception, not the rule. More options don't improve accuracy — they create confusion.

**Non-applicable opt-outs:** Include "non-applicable" when there's a real chance respondents haven't used the feature. Skip it when you know they have.

**Follow-up fields:** Every survey should end with one open text question: "Any additional feedback?" The qualitative insights often matter more than the scores.

**Survey length:** Completion rates drop sharply after 5 questions. If you need more data, run multiple focused surveys rather than one exhaustive one.

**Timing matters:** Send within 24 hours of the experience you're measuring. Delayed surveys measure memory, not experience.

**Avoid leading questions:** "How much did you love our new feature?" is not a survey question. Neutral wording only.

---

## Output Template

```markdown
# Survey: [Type] - [Topic]

**Date designed:** [date]
**Survey type:** [PMF / NPS / CSAT / CES / JTBD / Van Westendorp]
**Measurement goal:** [what decision this informs]
**Target audience:** [who takes this survey and when]
**Expected sample size:** [minimum / target]
**Distribution method:** [in-app / email / Typeform link / etc.]

---

## Questions

[Paste the appropriate questions from the templates above, customized for your product]

---

## Interpretation Guide

**When results come in, look for:**
- [Specific threshold or pattern to watch for]
- [What action to take if result is X]
- [What action to take if result is Y]

---

## Follow-up Actions

Based on results:
- If [condition]: → [action]
- If [condition]: → [action]
- After results: Run `/user-research-synthesis` to synthesize qualitative responses
```

---

## Output Integration

### Where Files Go

**Survey designs:**
- Active work: `outputs/surveys/survey-[type]-[topic]-[date].md`
- After results come in: Archive to `context-library/metrics/` for baseline tracking
- Reference in PRDs: Link to survey in "Research & Validation" section

### Cross-Skill Integration

**Feeds into:**
- `/user-research-synthesis` - Synthesize open-text survey responses
- `/activation-analysis` - PMF and CES results inform activation work
- `/retention-analysis` - NPS and CSAT trends inform retention work
- `/feature-metrics` - Survey results help set success metric targets
- `/impact-sizing` - PMF strength informs market opportunity sizing

**Pulls from:**
- `context-library/metrics/` - Existing survey baselines to compare against
- `context-library/prds/` - Feature context for CES and JTBD questions
- `context-library/strategy/` - Pricing context for Van Westendorp surveys

---

## Output Quality Self-Check

Before delivering a survey design, verify:

- [ ] **Right methodology selected:** Survey type matches the measurement goal, not just "what we've done before"
- [ ] **Exact question wording used:** Questions match the validated templates above (not paraphrased or improvised)
- [ ] **Response scale is consistent:** 5-point scales used throughout (except NPS at 11 points)
- [ ] **Non-applicable included where needed:** Added only when users might genuinely not have experienced the feature
- [ ] **Interpretation guide included:** PM knows what to do with results before the survey runs
- [ ] **Sample size guidance given:** Minimum sample size stated for statistical reliability
- [ ] **Follow-up text field included:** Survey ends with an open-text question for qualitative depth
- [ ] **Past surveys checked:** Confirmed we're not re-running a survey we already have results for
- [ ] **Output file saved:** Survey saved to `outputs/surveys/survey-[type]-[topic]-[date].md`
