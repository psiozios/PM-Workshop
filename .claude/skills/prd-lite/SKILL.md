---
name: prd-lite
description: Generate lightweight PRD Lite feature proposals — decision-grade prioritization docs, not engineering-ready specs. Fast, structured, opinion-strong. Feeds into /prd-draft when the feature gets slated.
disable-model-invocation: false
user-invocable: true
---

# /prd-lite - Lightweight Feature Proposals

A PRD Lite is a decision doc, not a spec. It's what you write when you need alignment on whether to build something — not how to build it. One page, strong opinion, ready in 30 minutes.

**Use PRD Lite when:** You're pitching a feature, asking for prioritization, or proposing something that hasn't been fully scoped yet.
**Use /prd-draft when:** Engineering is ready to build, you need full spec, or the feature is already prioritized.

## Quick Start

```
/prd-lite

Fast feature proposal. I'll ask you 5 questions and generate a decision-ready one-pager.

1. Feature name and one-sentence description
2. What user problem does this solve? (be specific)
3. What's your hypothesis? (If we build X, Y will happen because Z)
4. What's the rough business case? (impact on key metric, revenue, retention, etc.)
5. How confident are you? (lots of evidence / educated guess / pure bet)

Output: outputs/prds/prd-lite-[feature]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

**Automatic Context Checks:**

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| North Star | `context-library/strategy/*.md` | north star, NSM, primary metric | Alignment check |
| Strategy | `context-library/strategy/*.md` | OKR, priority, pillar | Strategic fit |
| Research | `context-library/research/*.md` | user pain, problem, segment | Evidence for the claim |
| PRDs | `context-library/prds/*.md` | similar feature, related | Prior work to reference |
| Metrics | `context-library/metrics/*.md` | baseline, current rate | Numbers for the business case |

**Cross-Skill Links:**
- If the feature gets prioritized → Escalate to `/prd-draft` for full spec
- If the sizing is unclear → Run `/impact-sizing` first
- If research is thin → Run `/interview-guide` to validate
- For multi-perspective review → `/prd-review-panel`

---

## The PRD Lite Format

**Target length:** 300-400 words max. If it's longer, it's not a Lite.

**Target audience:** Product leadership, engineering leads, or anyone who needs to decide if this is worth investing in.

**Tone:** Direct, confident, opinionated. This is a pitch, not a report.

---

## Output Template

```markdown
# PRD Lite: [Feature Name]

**Status:** Proposal | **Stage:** Discovery
**Owner:** [PM Name] | **Date:** [Date]
**Confidence:** High (validated) / Medium (educated guess) / Low (bet)

---

## The Problem

[1-2 sentences. Specific pain, specific user, specific context. Not "users struggle with X" — "SMB users who onboard without a CSM abandon setup at step 3 at a 67% rate because they can't find the API key."]

## The Hypothesis

If we [build/change/remove X], then [specific users] will [specific behavior change], which will improve [metric] by approximately [range].

*Why we believe this:* [1-2 sentences of evidence — interview quote, data point, competitor signal, or honest "this is a bet."]

## Why Now

[Why does this matter in the current quarter? Competitive pressure, strategic bet, upcoming renewal season, recent churn pattern? 1-2 sentences.]

## The Rough Idea

[2-4 sentences describing the solution concept. No wireframes yet — just enough to evaluate scope. "A guided checklist on first login that adapts based on user type and auto-checks steps as they complete them."]

## Business Case (Back of Envelope)

- **Users affected:** [N] users or [X%] of [segment]
- **Expected impact:** [metric] from [baseline] to [target] → [business value]
- **Effort signal:** [Small (days) / Medium (weeks) / Large (months)] — [why]
- **Confidence:** [High / Medium / Low] — [what would increase confidence?]

## Biggest Risks

1. [Risk] — Mitigation: [one sentence]
2. [Risk] — Mitigation: [one sentence]

## Recommended Next Steps

- [ ] [Validate assumption: how and by when]
- [ ] [If green: escalate to /prd-draft]
- [ ] [Stakeholders to align before proceeding: names]

---

*Decision needed: Prioritize for [Q?] / Deprioritize / Need more validation*
```

---

## Guiding Principles for PRD Lite

**Lead with the problem, not the solution.** If you can't explain the user pain in one sentence, you don't understand it well enough to build for it.

**Make the hypothesis falsifiable.** "Improve engagement" is not a hypothesis. "Increase D7 retention by 8 points for users who complete the checklist" is.

**Show confidence honestly.** A low-confidence bet that you call out explicitly is better than a medium-confidence claim you can't defend in review.

**Include "why now."** Without urgency, features die in the backlog. Connect to a current OKR, deadline, or strategic window.

**Name the ask explicitly.** Does this need a decision? More research? Engineering estimate? End with a clear action.

---

## What Makes a Strong PRD Lite vs. a Weak One

| Strong | Weak |
|--------|------|
| "67% of SMB users abandon at step 3" | "Many users struggle with onboarding" |
| "If we add inline help, we expect 20pt activation gain" | "This should improve activation" |
| "Low confidence — needs 5 more interviews" | [Unstated confidence, assumed high] |
| "Effort: Medium (2-3 weeks, 2 engineers)" | "Effort: TBD" |
| "Decision needed by [date] for Q2 roadmap" | [No urgency stated] |

---

## Output Integration

**Files:** `outputs/prds/prd-lite-[feature]-[date].md`

**When to escalate to `/prd-draft`:**
- Feature is prioritized for an upcoming sprint
- Engineering is asking for spec
- You've validated the hypothesis and are ready to commit

**Cross-Skill Integration:**
- Feeds into `/prd-draft` — provide PRD Lite as context when drafting
- Feeds into `/impact-sizing` — use for business case backing
- Feeds into `/create-tickets` — high-level tickets can be created from PRD Lite
- Informed by `/interview-guide` and `/user-research-synthesis`

---

## Output Quality Self-Check

- [ ] **One page or less:** If it's longer, cut it. The value is in the discipline.
- [ ] **Problem is specific:** Names a specific user type, specific context, specific pain — not a category of problems
- [ ] **Hypothesis is falsifiable:** Has a specific metric, direction, and magnitude
- [ ] **Business case has numbers:** Even rough estimates with confidence levels — not "TBD"
- [ ] **Confidence level stated:** High / Medium / Low with the honest reasoning
- [ ] **Next steps are clear:** Specific action with owner, not "follow up"
- [ ] **Decision ask is explicit:** Does this need approval, more research, or an estimate?
- [ ] **Output saved:** `outputs/prds/prd-lite-[feature]-[date].md`
