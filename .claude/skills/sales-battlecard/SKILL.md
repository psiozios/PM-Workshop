---
name: sales-battlecard
description: Create competitive battle cards for the sales team. Turns product knowledge, win/loss intel, and competitive research into concise sales-ready cards — objection handling, differentiation points, landmine questions, and proof points.
disable-model-invocable: false
user-invocable: true
---

# /sales-battlecard - Arm Sales to Win More Deals

A battle card is what your AE pulls up when a prospect says "[Competitor] does the same thing." If it's not fast and specific, it won't get used.

## Quick Start

```
/sales-battlecard

Tell me:
1. Which competitor? (one card per competitor works best)
2. Your product name and 1-sentence positioning
3. What deals have you won/lost against this competitor recently?
4. What objections come up most often?

I'll check context-library/research/ for competitive intel and win-loss data first.

Output: outputs/analyses/battlecard-vs-[competitor]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Competitive Research | `context-library/research/competitive*.md` | competitor name, features, pricing | Existing competitive intel |
| Win/Loss Analysis | `context-library/research/win-loss*.md` | win, lost, deal, reason | Why we win and lose against this competitor |
| Meeting Notes | `context-library/meetings/*.md` | [competitor name], objection, comparison | Sales call intel |
| PRDs | `context-library/prds/*.md` | feature, capability, roadmap | Our actual capabilities to reference |

**Cross-Skill Links:**
- Deeper competitive analysis → `/competitor-analysis`
- Win/loss patterns → `/win-loss-analysis`
- Positioning document → `/write-prod-strategy` (messaging section)
- Customer proof points → `/feature-results` or `/user-research-synthesis`

---

## Battle Card Structure

### Section 1: The 30-Second Summary

```
## Competing against [Competitor Name]

When a prospect brings them up, the core message is:

"[Competitor] is [honest assessment of their strength]. We're better for [your target customer] because [your most defensible differentiator]."

Best for: [Which customer types you typically beat them with]
Avoid: [Which customer types you typically lose with — know when to walk away]
```

### Section 2: At a Glance

| | [Your Product] | [Competitor] |
|--|----------------|--------------|
| Primary use case | [What you're built for] | [What they're built for] |
| Target customer | [Your ICP] | [Their ICP] |
| Pricing model | [Model] | [Model] |
| Biggest strength | [Your #1] | [Their #1] |
| Biggest weakness | [Their #1 weakness] | [Your honest gap] |

### Section 3: Differentiation Points

**Where we win:**

1. **[Differentiator 1]**
   Claim: [What we do better]
   Proof: [Specific metric, customer quote, or case study]
   When to use: [Which deals or personas this resonates with]

2. **[Differentiator 2]**
   Claim: [What we do better]
   Proof: [Specific proof]
   When to use: [Context]

3. **[Differentiator 3]**
   Claim: [What we do better]
   Proof: [Specific proof]
   When to use: [Context]

**Where they have an edge (be honest):**
- [Area where they're stronger] — [What to say when it comes up]
- [Area where they're stronger] — [What to say when it comes up]

---

### Section 4: Handling Common Objections

```
Objection: "We're already using [Competitor] and it's fine."

Don't say: "They're actually inferior because..."
Do say: "Totally makes sense — they're good at [X]. The reason customers switch to us is usually [specific scenario]. Has [scenario] come up for you?"

---

Objection: "[Competitor] has [Feature] and you don't."

Don't say: "We're building that."
Do say: "That's fair — they do that differently. Here's the trade-off: [Competitor's approach] means [consequence], which is great if [use case]. Most of our customers care more about [alternative value]. Where are you on that?"

---

Objection: "[Competitor] is cheaper."

Don't say: "We're worth the extra cost."
Do say: "Depends on what you're optimizing for. If [use case], they're probably the right call at that price. If [your use case], the math changes — [ROI proof point or customer example]."

---

Objection: "We evaluated you both and [Competitor] won our last review."

Don't say: [Anything defensive]
Do say: "I appreciate you telling me. What drove that decision? I want to understand if anything has changed or if this is the wrong fit for us."
```

---

### Section 5: Landmine Questions

Questions to plant that expose the competitor's weaknesses without attacking directly.

**Landmine questions for [Competitor]:**

1. "[Question that naturally surfaces their weakness]"
   Why it works: [What this exposes about their limitation]

2. "[Question about a use case they struggle with]"
   Why it works: [What this reveals]

3. "[Question about migration, support, or future roadmap]"
   Why it works: [What this surfaces]

**Example landmines (customize for your competitor):**
- "How important is it to you that the data lives in your own environment vs. [Competitor]'s cloud?"
- "What happens to your workflows if [Competitor] raises prices next year?"
- "How does [Competitor] handle [edge case your product handles well]?"

---

### Section 6: Proof Points and References

```
Customer wins against [Competitor]:
- [Company type or name if sharable]: "[Quote or metric about switching from Competitor]" — [Role, Company]
- [Company type]: Switched from [Competitor] after [specific reason]. Now achieves [outcome].

Case study or reference available? [Yes — ask your manager / No — use the quote above]

G2/Capterra positioning: [Link or summary of how we compare publicly]
```

---

### Section 7: Know When to Walk Away

Some deals are the wrong fit. Recognize them early.

**Red flags that [Competitor] is the right choice for this prospect:**
- They need [specific feature/capability we don't have]
- Their budget is [range we can't match]
- Their primary use case is [their strength, not ours]
- [Other signals]

It's better to qualify out early than to win a bad-fit customer.

---

## Output Template

```markdown
# Battle Card: vs. [Competitor Name]
**Last updated:** [date]
**Maintained by:** [PM name]
**Use for:** [AEs / SEs / SDRs / CS — who this is for]

---

## 30-Second Summary

[When a prospect mentions [Competitor], say:]
"[2-3 sentence positioning message]"

Win profile: [Describe the deals we typically win]
Walk-away profile: [Describe the deals we typically lose]

---

## At a Glance Table

[Comparison table]

---

## Top 3 Differentiators

[Differentiator 1, 2, 3 with proofs]

---

## Objection Handling

[4-6 objections with do/don't responses]

---

## Landmine Questions

[3-5 questions to plant]

---

## Proof Points

[Customer wins, quotes, metrics]

---

## When to Walk Away

[Red flag list]
```

---

## Output Quality Self-Check

- [ ] **30-second summary written:** Can a new AE say it in one breath
- [ ] **Honest about gaps:** Includes where competitor has a genuine edge
- [ ] **Objection responses use "do/don't":** Specific language, not just principles
- [ ] **Proof points are specific:** Named metrics or direct customer quotes, not "customers love it"
- [ ] **Landmines are non-obvious:** Questions that sound natural, not like attacks
- [ ] **Walk-away criteria defined:** Saves everyone time when the fit is wrong
- [ ] **Output saved:** `outputs/analyses/battlecard-vs-[competitor]-[date].md`
