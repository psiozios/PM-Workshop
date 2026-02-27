---
name: ai-quality-debug
description: Evaluate and debug AI/ML feature quality from a PM perspective. Design evaluation frameworks, interpret model performance metrics, diagnose quality regressions, and make go/no-go decisions on AI feature readiness without needing to read the code.
disable-model-invocation: false
user-invocable: true
---

# /ai-quality-debug - Evaluate AI Features Like a PM

AI features fail differently than traditional software. The bug isn't a crash — it's a bad output. This skill gives PMs the framework to define quality, catch regressions, and make informed shipping decisions without being an ML engineer.

## Quick Start

```
/ai-quality-debug

Modes:
--define      Define quality criteria for an AI feature before it ships
--evaluate    Evaluate current AI output quality using structured assessment
--diagnose    Investigate a quality regression or user complaint pattern
--decide      Go/no-go framework for launching an AI feature

Tell me:
1. Mode (or describe your AI quality situation)
2. What the AI feature does (1-2 sentences)
3. What quality problem you're observing (or are trying to prevent)

I'll check context-library/prds/ for the feature spec and context-library/metrics/ for baseline data.

Output: outputs/analyses/ai-quality-[feature]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| PRDs | `context-library/prds/*.md` | AI, ML, model, generate, predict | Intended behavior and quality expectations |
| Metrics | `context-library/metrics/*.md` | accuracy, precision, recall, hallucination, quality score | Baseline and current performance |
| Research | `context-library/research/*.md` | user, feedback, AI, wrong, mistake | User complaints about AI quality |
| Feature Results | `context-library/prds/*.md` | post-launch, A/B, accuracy, satisfaction | Prior AI quality assessments |

**Cross-Skill Links:**
- Define metrics first → `/feature-metrics` (STEDII framework for AI metrics)
- Design evaluation experiment → `/experiment-metrics`
- Post-launch diagnosis → `/root-cause-analysis` for quality regressions
- Ship decision → `/experiment-decision`

---

## AI Quality Framework for PMs

AI quality has two dimensions traditional software doesn't:

1. **Correctness:** Is the output right? (hard — needs ground truth)
2. **Usefulness:** Does the output help the user accomplish their goal? (more measurable via behavioral data)

PMs typically can't evaluate correctness directly. But you can:
- Define what "good" looks like with examples
- Design evaluations that surface quality at scale
- Track proxy metrics that correlate with correctness
- Build feedback loops that catch regressions early

---

## Mode: --define (Quality Criteria Before Launch)

### Step 1: Define the Output Space

What does the AI feature produce?

```
Feature: [Name]
Input: [What the user provides]
Output: [What the AI produces — text / classification / ranking / recommendation / summary]
Success state: [What a good output looks like — describe with examples]
Failure states: [What a bad output looks like — be specific]
```

**Failure taxonomy (customize for your feature):**

| Failure type | Description | Severity |
|-------------|-------------|----------|
| Hallucination | Confident output that is factually wrong | High |
| Irrelevance | Output is correct but doesn't help the user | High |
| Incomplete | Output misses key information | Medium |
| Format error | Right content, wrong format or length | Low |
| Bias | Output systematically favors certain groups or topics | High |
| Unsafe | Output could cause harm or embarrassment | Critical |

### Step 2: Define Quality Thresholds

Before launching, set the minimum acceptable bar:

```
## Quality Thresholds: [Feature Name]

Primary quality metric: [What you'll measure — user satisfaction score, task completion rate, accuracy on eval set]
Launch bar: [X]% — [describe what this means]
Watch threshold: [Y]% — trigger investigation if drops below this
Kill threshold: [Z]% — roll back if this is reached

Secondary guardrails:
- Hallucination rate: <[X]% (if applicable)
- User override / rejection rate: <[X]% (user chose not to use the output)
- Negative feedback rate: <[X]%
```

### Step 3: Build a Golden Set

A golden set is a curated collection of test inputs with known-good outputs. It's the ground truth you use to catch regressions.

```
Golden set guidelines:
- 50-200 examples for an initial MVP eval set
- Cover the full range of inputs (easy / ambiguous / edge cases / adversarial)
- Include examples that represent your highest-value user segments
- Label "good" and "bad" outputs with explicit rationale — not just intuition
- Update the set every time you encounter a new failure mode

Storage: context-library/metrics/ai-eval-[feature]-golden-set.md (or equivalent)
```

---

## Mode: --evaluate (Current Quality Assessment)

### Structured Evaluation Rubric

For each AI output, score on these dimensions:

```
## Output Evaluation Rubric

Feature: [Name]
Evaluator: [Human reviewer / Automated / LLM-as-judge]
Sample size: [N outputs reviewed]

| Dimension | Weight | Score (1-5) | Notes |
|-----------|--------|-------------|-------|
| Accuracy / Correctness | [X]% | [1-5] | Is the core claim correct? |
| Helpfulness | [X]% | [1-5] | Does it advance the user's goal? |
| Completeness | [X]% | [1-5] | Does it cover what's needed? |
| Format / Readability | [X]% | [1-5] | Is it in the right form? |
| Safety / Appropriateness | [X]% | [1-5] | Would you show this to any user? |

Weighted quality score: [Calculate]
Pass threshold: [X.X]
```

### LLM-as-Judge (fast evaluation at scale)

When you have many outputs to evaluate, use an LLM to score them against your rubric. This is approximate but fast.

**Prompt template:**
```
You are evaluating the quality of an AI-generated output.

Feature context: [What the AI feature does]
User input: [The input provided]
AI output: [The output to evaluate]
Expected quality criteria: [Your rubric dimensions]

Rate this output 1-5 on each dimension. Explain your reasoning in one sentence per dimension. Flag any critical failures (hallucination, safety, bias).
```

**Caveat:** LLM-as-judge has its own biases. Use it for ranking and relative comparison, not as absolute ground truth.

---

## Mode: --diagnose (Quality Regression Investigation)

When quality drops, investigate systematically.

### Regression Diagnostic Checklist

```
□ When did the regression start? (deployment date, model update, data change)
□ Is it affecting all users or a subset? (segment, use case, input type)
□ What specifically got worse? (which failure type from taxonomy)
□ Did any model, prompt, or data change happen at the same time?
□ Has the input distribution shifted? (users sending different queries than before)
□ Is it a new failure mode or an existing one getting worse?
```

### Common Regression Causes

| Cause | Signal | Investigation |
|-------|--------|--------------|
| Model change / rollout | Sudden drop on specific date | Check deployment log |
| Prompt change | Outputs follow a new wrong pattern | A/B compare prompt versions |
| Data drift | Gradually worsening over time | Compare recent vs. historical input distribution |
| Edge case exposure | Affects new user segment | Segment the failure by user type or input type |
| Threshold calibration | Classification decisions getting worse | Check confidence score distribution |

---

## Mode: --decide (Go/No-Go for AI Feature Launch)

AI features need different launch criteria than traditional features. Use this checklist:

### AI Feature Go/No-Go Checklist

```
## Functional Readiness
□ Quality threshold met: [Feature] scores [X]% on primary quality metric (threshold: [Y]%)
□ Critical failure rate below threshold: Hallucination/unsafe outputs <[Z]%
□ Golden set regression test passing: [X]/[N] test cases pass
□ Edge cases handled: Empty input, adversarial input, maximum length input all handled gracefully

## User Experience
□ Loading state exists: Users know the AI is working
□ Error state exists: Users know when it fails and what to do
□ Override/dismiss is available: Users can ignore or replace the output
□ Feedback mechanism exists: Users can flag bad outputs

## Trust and Safety
□ Hallucination risk assessed: [Low / Medium — mitigated by / High — do not ship]
□ Bias assessment completed: Outputs don't systematically disadvantage any user group
□ Privacy review: No PII or sensitive data leaking into outputs
□ Legal review: No IP or copyright issues with generated content (if applicable)

## Monitoring
□ Quality dashboard exists: Real-time visibility into output quality metrics
□ Alerts configured: Automated alert if quality drops below [threshold]
□ Feedback loop live: User thumbs-up/down or equivalent captured and reviewed
□ Human review queue: Mechanism to review flagged or low-confidence outputs

## Rollback Plan
□ Feature flag in place: Can disable without a deployment
□ Rollback criteria defined: [What metric triggers rollback?]
□ Rollback tested: Have verified that disabling works cleanly
```

**Ship when:** All P0 items green, P1 items mitigated or accepted
**Don't ship when:** Any critical failure or safety issue unresolved, quality below launch threshold

---

## Output Template

```markdown
# AI Quality Assessment: [Feature Name]

**Date:** [date]
**Mode:** [Define / Evaluate / Diagnose / Decide]
**Feature:** [1-sentence description]

---

## Quality Definition

**Primary metric:** [What we measure]
**Launch threshold:** [Minimum acceptable score]
**Current score:** [If evaluating]

---

## Evaluation Results (if --evaluate or --diagnose)

**Sample size:** [N outputs]
**Evaluation method:** [Human review / LLM-as-judge / Automated]
**Overall score:** [X.X / 5]

| Dimension | Score | Pass/Fail | Key finding |
|-----------|-------|-----------|-------------|
| Accuracy | [X.X] | [Pass/Fail] | [Finding] |
| Helpfulness | [X.X] | [Pass/Fail] | [Finding] |

**Most common failure type:** [From taxonomy]
**Example failures:** [2-3 specific examples]

---

## Root Cause (if --diagnose)

[Timeline and diagnosis of what changed]

---

## Go/No-Go (if --decide)

**Recommendation:** [Go / No-Go / Go with conditions]
**Conditions (if any):** [What must be addressed before or after launch]
**Monitoring plan:** [What will be watched post-launch]

---

## Action Items

| Action | Owner | Due | Priority |
|--------|-------|-----|----------|
| [Action] | [owner] | [date] | [P0/P1] |
```

---

## Output Quality Self-Check

- [ ] **Quality defined concretely:** Not "outputs should be good" — specific rubric with thresholds
- [ ] **Failure taxonomy created:** Named failure modes, not just "bad output"
- [ ] **Golden set referenced:** Evaluation grounded in consistent test cases
- [ ] **Regression investigation systematic:** Compared before/after, checked all likely causes
- [ ] **Go/no-go is binary:** Clear recommendation, not "we should keep monitoring"
- [ ] **Monitoring plan defined:** Ongoing visibility after launch, not just at launch
- [ ] **Output saved:** `outputs/analyses/ai-quality-[feature]-[date].md`
