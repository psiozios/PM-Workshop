---
name: feature-request-analysis
description: Synthesize and prioritize feature requests from multiple sources. Cluster by theme, score by frequency and strategic fit, surface the real job-to-be-done beneath each request, and translate into roadmap-ready recommendations.
disable-model-invocation: false
user-invocable: true
---

# /feature-request-analysis - Turn Requests into Roadmap Signal

Feature requests are symptoms. The job is to find the underlying need — and decide if building for it is the right call.

## Quick Start

```
/feature-request-analysis

Tell me:
1. Source(s) of requests (support tickets, NPS, sales calls, user interviews, community, etc.)
2. Time period covered
3. Product area or "all" for full backlog
4. Do you want: --prioritize (what to build next) or --cluster (thematic grouping only)?

Paste raw requests, or I'll check context-library/research/ first.

Output: outputs/analyses/feature-requests-[area]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| User Research | `context-library/research/*.md` | request, want, wish, feature | Verbatim requests with context |
| Meeting Notes | `context-library/meetings/*.md` | feature, ask, sales, customer | Sales and CS feature asks |
| VoC Reports | `context-library/research/voc-*.md` | request, feature, missing | Prior themes to avoid duplication |
| Strategy | `context-library/strategy/*.md` | roadmap, priorities, bets | Filter requests by strategic fit |
| PRDs | `context-library/prds/*.md` | scope, out of scope | What's already planned |

**Cross-Skill Links:**
- Themes → `/voc-report` for cross-channel validation
- Prioritized items → `/prd-lite` or `/prd-draft`
- Competitive requests → `/competitor-analysis`
- Impact estimate → `/impact-sizing` or `/opportunity-sizing`

---

## Step 0: Context Check

Before analyzing raw requests:
1. Check `context-library/research/` for prior feature request analyses — avoid re-clustering what's already been synthesized
2. Check `context-library/strategy/` for current roadmap priorities — filter requests against strategic bets
3. Check `context-library/prds/` for in-flight work — remove duplicates

---

## Step 1: Cluster by Job-to-Be-Done

Group requests by the underlying job, not by the specific feature asked for. Users ask for solutions; your job is to find the problem.

**Clustering questions:**
- What is the user trying to accomplish when they ask for this?
- When do they need this capability?
- What are they doing today instead?
- What outcome defines success for them?

**Cluster format:**
```
## Cluster: [Job name - verb + object + context]
Example: "Export data to share with finance team"

Requests in this cluster:
- "CSV export" — [source, date]
- "Download report" — [source]
- "Send to my boss" — [source]
- "Connect to Excel" — [source]

Users affected: [rough count or %]
Current workaround: [what they do today]
Underlying job: As a [user type], when [situation], I need [outcome], so that [why it matters].
```

---

## Step 2: Score Each Cluster

| Dimension | Score (1-5) | What to measure |
|-----------|-------------|-----------------|
| **Frequency** | 5 = top 10% of all requests | How often does this come up? |
| **Intensity** | 5 = users threaten to churn | How upset are users when it's missing? |
| **Strategic fit** | 5 = directly enables a current OKR | Does this move a Key Result? |
| **Scope clarity** | 5 = clear solution path | Can engineering estimate this? |
| **Breadth** | 5 = affects all user types | Is this niche or universal? |

**Weighted score:** (Frequency × 2 + Intensity × 2 + Strategic fit × 3 + Breadth × 1) / 8

Flag anything scoring above 4.0 as P0 consideration.

---

## Step 3: Distinguish Request from Need

For each high-scoring cluster, answer:

**What they're asking for:** [Specific feature request]
**What they actually need:** [Underlying job or outcome]
**Why the gap matters:** [If we build exactly what they asked, will it solve the real problem?]
**Alternative approaches to the same need:** [Other ways to solve this without building what was requested]

---

## Step 4: Apply the Feasibility Filter

Before escalating to roadmap:

| Question | Why it matters |
|----------|---------------|
| Is this something we should own? | Or does another tool already do this well? |
| Does this pull us toward or away from our core use case? | Surface area risk |
| Can we solve 80% of the need with 20% of the scope? | Lightweight solution option |
| Is this a now-problem or a future-problem? | Timing matters |

---

## Output Template

```markdown
# Feature Request Analysis: [Area] — [Period]

**Date:** [date]
**Period covered:** [range]
**Sources analyzed:** [list with counts]
**Total requests reviewed:** [N]
**Clusters identified:** [N]

---

## Top Clusters by Score

### 1. [Cluster Name] — Score: [X.X]

**Job:** As a [user], when [situation], I need [outcome].
**Frequency:** [High/Med/Low] — [N] requests from [N] unique sources
**Intensity:** [Critical / Frustrating / Nice-to-have]
**Strategic fit:** [Direct / Adjacent / Outside] — [which OKR it maps to]

**What users are saying:**
- "[Direct quote]" — [Source, date]
- "[Direct quote]" — [Source]

**What they actually need vs. what they asked for:**
[1-2 sentences on the gap, if any]

**Recommended action:** [Build / Investigate further / Decline with reason / Defer to Q[X]]

---

[Repeat for Clusters 2, 3, 4...]

---

## Requests Explicitly Declined

| Cluster | Reason for decline | Response to users |
|---------|-------------------|-------------------|
| [Cluster] | [Strategic misfit / Niche / Already covered by X] | [What to tell users] |

---

## Requests Already In Flight

| Cluster | Related ticket/PRD | ETA |
|---------|-------------------|-----|
| [Cluster] | [Link or reference] | [Sprint / Quarter] |

---

## Recommended Next Steps

1. [Highest-scoring cluster] → `/prd-lite` to scope
2. [Second cluster] → `/impact-sizing` to quantify before committing
3. [Third cluster] → Add to research agenda — need more signal
```

---

## Output Quality Self-Check

- [ ] **Clustered by job, not by feature:** Groups reflect underlying needs, not literal requests
- [ ] **Each cluster has a JTBD statement:** "As a [user], when [situation], I need [outcome]"
- [ ] **Scores calculated:** Every cluster has a weighted score, not just subjective "high/medium/low"
- [ ] **Direct quotes included:** At least 2 verbatim requests per top cluster
- [ ] **Already-in-flight items flagged:** No recommending what's already being built
- [ ] **Declined requests explained:** Clear reasoning, not just "not prioritized"
- [ ] **Output saved:** `outputs/analyses/feature-requests-[area]-[date].md`
