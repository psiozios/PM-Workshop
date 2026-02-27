---
name: root-cause-analysis
description: Structured problem investigation for PMs. When metrics drop, bugs spike, users churn, or features underperform — diagnose what's actually broken before deciding what to fix. Uses 5 Whys, fishbone diagrams, and data-driven hypothesis testing.
disable-model-invocation: false
user-invocable: true
---

# /root-cause-analysis - Diagnose Before You Prescribe

When something goes wrong, the instinct is to jump to solutions. This skill slows you down long enough to understand the problem correctly — so the fix actually works.

## Quick Start

```
/root-cause-analysis

Tell me:
1. What's the symptom? (metric dropped, feature underperforming, user complaints spiked, etc.)
2. When did it start? (specific date, release, or event)
3. What data do you have? (paste numbers, error logs, user quotes, etc.)
4. What have you already ruled out?

Output: outputs/analyses/rca-[issue]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Metrics | `context-library/metrics/*.md` | drop, decline, regression, baseline | Quantitative baseline and trend data |
| Recent Launches | `context-library/launches/*.md` | released, shipped, deployed, change | Recent changes that could explain timing |
| User Research | `context-library/research/*.md` | friction, problem, confused, broken | Qualitative signals that match symptoms |
| Meeting Notes | `context-library/meetings/*.md` | bug, issue, complaint, escalation | Stakeholder-reported problems |
| PRDs | `context-library/prds/*.md` | implementation, edge case, assumption | Design intent vs. what shipped |

**Cross-Skill Links:**
- Metrics baseline → analytics MCP query if connected
- User behavior signal → `/activation-analysis` or `/retention-analysis`
- Root cause identified → `/decision-doc` to document the fix
- Future prevention → `/pre-mortem` for next launch

---

## Step 0: Frame the Problem Correctly

A good RCA starts with a well-defined symptom statement:

**Weak:** "Activation is down"
**Strong:** "D7 activation rate dropped from 42% to 31% between Jan 15 and Feb 1. The drop correlates with a UI change in the onboarding flow deployed Jan 14. New users who completed onboarding before the change activated at 43%; after the change, 29%."

Get specific before investigating:
- What is the metric? (exact name and definition)
- What is the baseline? (what was normal before)
- When exactly did it change? (can you isolate a date or event?)
- Who is affected? (all users? new users? a specific cohort or segment?)
- What didn't change? (what's working normally — helps isolate the variable)

---

## Method 1: The 5 Whys

Best for: Simple causal chains, human or process failures

Start with the symptom and ask "Why?" recursively until you hit a root cause (usually 3-7 levels deep). Stop when you reach something you can actually fix.

```
Symptom: [State the problem]

Why 1: [First-order cause]
Why 2: [Why did that happen?]
Why 3: [Why did that happen?]
Why 4: [Why did that happen?]
Why 5: [Why did that happen?]

Root cause: [What you can actually fix]
```

**5 Whys trap:** Don't stop at the first convenient answer. If "Why 1" is "the button was broken," that's not a root cause — that's still a symptom. Keep going.

---

## Method 2: Fishbone Diagram (Ishikawa)

Best for: Complex problems with multiple potential contributing factors

Organize possible causes into 6 categories:

```
## Fishbone Analysis: [Problem Statement]

### Product / UX
- [Possible cause]
- [Possible cause]

### Process / Workflow
- [Possible cause]
- [Possible cause]

### Data / Measurement
- [Possible cause — could the data itself be wrong?]
- [Possible cause]

### People / Users
- [User behavior or segment shift]
- [Possible cause]

### Technology / Infrastructure
- [Backend, latency, reliability issue]
- [Possible cause]

### External / Environment
- [Market change, competitor, seasonality]
- [Possible cause]
```

After listing causes, vote on highest-probability candidates and test those first.

---

## Method 3: Hypothesis-Driven Investigation

Best for: Data-rich environments, ambiguous problems

Build a hypothesis tree — a structured set of "if true, then X explains the symptom" statements.

```
## Problem: [Metric drop or issue]

### Hypothesis A: [Most probable cause]
Evidence that supports it: [data]
Evidence against it: [data]
Test to confirm/reject: [What data point would prove or disprove this?]
Status: [Confirmed / Rejected / Inconclusive]

### Hypothesis B: [Second-most probable]
Evidence that supports it: [data]
Evidence against it: [data]
Test to confirm/reject: [What data point would prove or disprove this?]
Status: [Confirmed / Rejected / Inconclusive]

### Hypothesis C: [Third]
[Same format]
```

Work hypotheses in order of probability and testability. Rule out fast what you can rule out fast.

---

## Step 3: Isolate the Variable

Before concluding, answer:
- **What changed?** (code deploy, configuration, pricing, copy, competitor action, external event)
- **When exactly?** (can you correlate the metric drop to a timestamp?)
- **Who is affected?** (specific cohort, device, geography, plan tier)
- **What's NOT affected?** (control group behavior helps isolate the cause)

**Correlation vs. causation check:**
"X happened around the same time as Y" is not a root cause. To confirm causation, you need:
1. Temporal order (X happened before Y)
2. Co-variation (when X changes, Y changes consistently)
3. Elimination of alternatives (no other variable explains Y)

---

## Output Template

```markdown
# Root Cause Analysis: [Issue Name]

**Date:** [date]
**Analyst:** [name]
**Problem statement:** [Specific, quantified symptom with baseline and current state]
**Impact:** [Who is affected, how many users, revenue impact if known]
**Urgency:** [P0 - immediate fix / P1 - fix this sprint / P2 - backlog]

---

## Timeline

| Date | Event | Impact |
|------|-------|--------|
| [date] | [change, release, event] | [observed effect] |
| [date] | [change] | [effect] |
| [date] | [symptom first noticed] | [metric value] |

---

## Investigation Summary

**Method used:** [5 Whys / Fishbone / Hypothesis-driven]

**Hypotheses tested:**
1. [Hypothesis] — [Confirmed / Rejected] — [Evidence]
2. [Hypothesis] — [Confirmed / Rejected] — [Evidence]
3. [Hypothesis] — [Confirmed / Rejected] — [Evidence]

---

## Root Cause

**Primary root cause:** [One sentence — the specific, actionable thing that caused the problem]

**Contributing factors:** [Other factors that made it worse or harder to catch]

**Why we didn't catch it sooner:** [Monitoring gap, missing alert, or process gap]

---

## Fix

**Immediate action (stop the bleeding):** [What to do right now]
**Permanent fix:** [What to build or change to prevent recurrence]
**Validation:** [How will we know the fix worked? What metric returns to baseline?]

---

## Prevention

| What failed | Process improvement | Owner | By when |
|-------------|--------------------|--------------------|---------|
| [Monitoring gap] | [Add alert on X] | [name] | [date] |
| [Testing gap] | [Add test case for Y] | [name] | [date] |
| [Process gap] | [Update runbook / add step] | [name] | [date] |

---

## Open Questions

- [Question that still needs answering]
- [Data point we couldn't get]
```

---

## Output Quality Self-Check

- [ ] **Problem quantified:** Symptom has a baseline and a current number — not vague ("dropped" vs "dropped from 42% to 31%")
- [ ] **Timeline constructed:** Key events dated and correlated to symptom onset
- [ ] **Multiple hypotheses tested:** Did not stop at the first plausible explanation
- [ ] **Root cause is fixable:** Not a symptom restated as a cause
- [ ] **Prevention addressed:** Fix and process improvement documented with owners
- [ ] **Validation defined:** Clear signal that confirms the fix worked
- [ ] **Output saved:** `outputs/analyses/rca-[issue]-[date].md`
