---
name: peer-review
description: Cross-model code review verification. Evaluate another AI's findings against actual code.
disable-model-invocation: false
user-invocable: true
---

# /peer-review - Trust But Verify

Got a code review from ChatGPT, Gemini, Copilot, or Codex? Don't accept it at face value. This skill reads the actual code and verifies each finding. AIs frequently flag non-issues. You deserve to know which findings are real.

## Quick Start

```
/peer-review

Paste or describe the findings from the other AI model's review.
I'll read the actual code and give each finding a verdict:

- CONFIRMED — real issue, needs fixing
- FALSE POSITIVE — not actually a problem (I'll explain why)
- PARTIALLY VALID — has a point but overstates severity or misses context
- NEEDS INVESTIGATION — can't confirm without more info

Output: outputs/analyses/peer-review-[scope]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Codebase | Project source files | referenced files, functions | Actual implementation to verify against |
| Past Reviews | `outputs/analyses/code-review-*.md` | same files, same issues | Prior Claude reviews for comparison |
| PRDs | `context-library/prds/*.md` | requirements, intent | Whether "issues" are actually intended behavior |

**Cross-Skill Links:**
- Pair with → `/code-review` for Claude's own primary review
- Confirmed issues → `/create-tickets --quick` to log fixes
- Architecture questions → `/explore-codebase` for deeper investigation

---

## When to Use This Skill

- After getting a code review from another AI tool (ChatGPT, Gemini, Copilot, Codex)
- When triaging a long list of AI-generated review comments
- When you want a second opinion but don't want to blindly trust it
- When two AI tools disagree and you need a tiebreaker

---

## Workflow

### Step 1: Receive Findings

The PM pastes or describes the other model's findings. Accept them in any format: bullet points, structured review, raw conversation paste.

### Step 2: Parse and Organize

Break the findings into individual items. For each one, extract:
- What file/line it references
- What the claimed issue is
- What severity the other model assigned (if any)

### Step 3: Verify Each Finding

For EACH finding:

1. **Read the actual source code** referenced. Never evaluate based on the description alone.
2. **Check the broader context** (imports, related files, configuration). An issue that looks real in isolation might be handled elsewhere.
3. **Determine the verdict:**
   - **CONFIRMED** - The issue exists and is correctly described
   - **FALSE POSITIVE** - The issue does not exist. Explain what the other model missed or misunderstood.
   - **PARTIALLY VALID** - There's a kernel of truth but the severity is wrong, the description is inaccurate, or context changes the picture
   - **NEEDS INVESTIGATION** - Can't confirm without running the code, checking a database, or accessing something outside the codebase

### Step 4: Create Action Plan

For confirmed and partially valid findings only:
- Prioritize by severity
- Group related fixes
- Estimate effort per fix

### Step 5: Deliver Summary

Statistics, detailed evaluations, and prioritized action plan.

---

## Behavioral Rules

- **ALWAYS read the actual code before evaluating.** This is the whole point.
- **Be honest about false positives.** AIs frequently misread code. Don't confirm an issue just because another model said it exists.
- **Explain your reasoning.** For false positives, explain specifically what the other model missed (e.g., "They didn't see that this is already handled in the middleware at [file:line]").
- **Give credit where due.** If the other model caught something Claude's own review missed, acknowledge it.
- **Don't be defensive.** If the other model found a real issue, say so clearly.
- **Less context ≠ wrong.** The other model had less project context. Some findings will be wrong because of that, not because the model is bad. Explain the missing context rather than dismissing the model.

---

## Output Template

```markdown
# Peer Review Verification: [Scope]

**Date:** [date]
**Source model:** [ChatGPT / Gemini / Copilot / Codex / Other]
**Total findings reviewed:** [count]

---

## Summary

| Verdict | Count | % |
|---------|-------|---|
| Confirmed | X | X% |
| False Positive | X | X% |
| Partially Valid | X | X% |
| Needs Investigation | X | X% |

---

## Confirmed Issues

### [Finding 1 title]
- **Original claim:** [What the other model said]
- **Verdict:** CONFIRMED
- **Actual code:** [File:line] - [What the code actually does]
- **Severity:** [CRITICAL / HIGH / MEDIUM / LOW]
- **Fix:** [Suggested fix]

---

## False Positives

### [Finding N title]
- **Original claim:** [What the other model said]
- **Verdict:** FALSE POSITIVE
- **Why:** [Specific explanation of what they missed - e.g., "Already handled in middleware at auth.ts:42"]

---

## Partially Valid

### [Finding N title]
- **Original claim:** [What the other model said]
- **Verdict:** PARTIALLY VALID
- **What's right:** [The kernel of truth]
- **What's wrong:** [What's overstated or inaccurate]
- **Adjusted severity:** [Correct severity]

---

## Needs Investigation

### [Finding N title]
- **Original claim:** [What the other model said]
- **Why uncertain:** [What we'd need to check]
- **How to verify:** [Specific next step]

---

## Action Plan (Confirmed + Partially Valid Only)

| Priority | Finding | File | Fix | Effort |
|----------|---------|------|-----|--------|
| 1 | [Issue] | [File] | [Fix] | [S/M/L] |
| 2 | [Issue] | [File] | [Fix] | [S/M/L] |
```

---

## Output Quality Self-Check

- [ ] **Every finding evaluated against actual code** - not just the description
- [ ] **False positives explained with evidence** - specific file:line showing why it's not an issue
- [ ] **Confirmed issues have suggested fixes** - actionable, not just "this is bad"
- [ ] **Action plan only includes real issues** - false positives excluded from fix list
- [ ] **Tone is fair** - credit given for good catches, context given for misses
- [ ] **Output saved:** `outputs/analyses/peer-review-[scope]-[date].md`
