---
name: deprecation-plan
description: Plan and execute feature or product deprecations. Covers sunset criteria, user impact analysis, migration paths, communication sequencing, and how to close down something gracefully without burning customer trust.
disable-model-invocation: false
user-invocable: true
---

# /deprecation-plan - Sunset Gracefully

Deprecating features is harder than shipping them. This skill structures the entire shutdown: who's affected, how to help them migrate, what to communicate and when, and how to measure success.

## Quick Start

```
/deprecation-plan

Tell me:
1. What's being deprecated? (feature, API version, product tier, integration)
2. Why is it being deprecated? (low usage, maintenance cost, strategic shift, replacement ready)
3. Who uses it? (rough user count or segment — even "unknown" is okay to start)
4. Is there a replacement? (what users can do instead)
5. Timeline constraints? (must be done by X, or no hard deadline)

I'll check context-library/ for PRDs, launch docs, and metrics on the affected feature.

Output: outputs/analyses/deprecation-plan-[feature]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| PRDs | `context-library/prds/*.md` | feature name, use case | Who uses it and why |
| Metrics | `context-library/metrics/*.md` | usage, active users, engagement | Actual usage data |
| Research | `context-library/research/*.md` | customer, segment, workflow | How deeply embedded is this in user workflows |
| Launches | `context-library/launches/*.md` | feature launch, GA date | When it launched (helps estimate user expectations) |
| Meeting Notes | `context-library/meetings/*.md` | [feature name], customer, contract | Customer contracts or commitments that may block sunset |

**Cross-Skill Links:**
- Usage data → analytics MCP if connected
- Replacement feature → `/prd-lite` or `/prd-draft`
- Communication plan → `/slack-message` and `/content-marketing` (--announce mode)
- Post-deprecation review → `/feature-results`

---

## Step 1: Deprecation Readiness Assessment

Before committing to a deprecation timeline, answer:

| Question | Why it matters |
|----------|---------------|
| How many active users/accounts use this in the last 90 days? | The size of the migration problem |
| What percentage of revenue-generating accounts use this? | Enterprise or high-value accounts need special handling |
| Is there a contractual commitment to maintain this? | Legal review may be required |
| What's the replacement? Is it actually ready? | Deprecating without a path is abandonment |
| What's the cost of maintaining status quo? | The business case for deprecation |

**Deprecation signal — consider deprecating when:**
- Active usage is below [X]% of MAU for 90+ days
- The feature costs more to maintain than it generates in retention or acquisition value
- A superior replacement exists and is GA
- The feature conflicts with security, compliance, or architectural goals

---

## Step 2: Impact Analysis

Understand exactly who is affected and how badly.

### User Segments Affected

```
## Segment 1: [User type]
Active users: [N]
How they use the feature: [Workflow description]
Impact of removal: [High / Medium / Low — and why]
Migration path: [What they should do instead]
Migration complexity: [Trivial (1 step) / Moderate (requires change) / Complex (significant rework)]
```

### Dependency Mapping

- Does any other feature depend on this one?
- Do any third-party integrations reference this?
- Are there any API consumers of this functionality?
- Are there internal tools or scripts that depend on this?

---

## Step 3: Migration Path Design

The deprecation is only as good as the alternative.

```
## Migration Path: [Deprecated Feature] → [Replacement]

For [User type]:
1. [Step 1 — specific action]
2. [Step 2]
3. [Step 3]

Time to migrate: [Estimate per user]
Data migration: [Automatic / Manual / None needed]
Feature parity gaps: [What the replacement doesn't do yet that the old feature did]
Known migration friction: [What will be hardest for users]

Support resource: [Docs link / Help article / CSM support / Migration script]
```

**If feature parity gaps exist:**
Don't deprecate until those gaps are closed, or explicitly acknowledge them and get product leadership sign-off on the trade-off.

---

## Step 4: Communication Plan

The most common deprecation mistake is under-communicating. Err toward over-communication.

### Communication Timeline

| Timing | Audience | Channel | Message focus |
|--------|---------|---------|---------------|
| T-90 days | Power users / enterprise / high-impact accounts | Direct (CS, email, in-app) | "Here's what's changing and why. Here's your migration path." |
| T-60 days | All affected users | In-app banner + email | "This feature sunsets in 60 days. Here's how to migrate." |
| T-30 days | All affected users | In-app + email | "30 days left. Have you migrated? Here's help." |
| T-7 days | Still-active users of old feature | Targeted in-app + email | "One week left. Your workflow will change on [date]." |
| T-0 | All affected users | In-app notification | "This feature has been retired. Here's where to go." |
| T+30 | Migrated users | Survey or CSM check-in | "How's the new workflow? What can we improve?" |

### Communication Templates

**Initial announcement (long lead):**
```
Subject: [Feature name] is changing on [date]

We're retiring [Feature] on [Date].

[Why — be honest. "We're replacing it with something better" or "We're simplifying our product" — not vague "to improve your experience."]

Here's what this means for you:
- [Specific impact on their workflow]
- [Replacement: what they should use instead]
- [What we're doing to make migration easy]

[CTA: Link to migration guide or to schedule a migration call with CSM for enterprise]

Questions? [Contact info]
```

**In-app banner (short, visible):**
```
"[Feature] will retire on [date]. [Switch to X] to keep your workflows running."
[Learn more] [Migrate now]
```

---

## Step 5: Sunset Criteria and Go/No-Go

Define what "done" looks like before you flip the switch.

```
## Sunset Go/No-Go Checklist

□ Migration rate: [X]% of users migrated (minimum threshold)
□ High-value accounts: All enterprise/high-ARR accounts confirmed migrated or in progress
□ Support ticket volume: Migration-related support tickets trending down
□ Docs complete: Migration guide live, searchable, and validated by [N] test users
□ No contractual obligations remaining
□ Engineering prepared for sunset date (disable, not delete, initially)
□ Rollback plan: Can we re-enable if we discover a critical use case we missed?
```

**Disable, don't delete:** On sunset date, disable the feature and show a clear message. Don't delete data immediately. Keep data retained for [30-90 days] after sunset, then follow your data retention policy.

---

## Output Template

```markdown
# Deprecation Plan: [Feature Name]

**Date:** [date]
**Target sunset date:** [date]
**Owner:** [PM name]
**Status:** [Planning / In progress / Communicated / Sunset complete]

---

## Why We're Deprecating This

[2-3 sentences: usage data, cost, replacement ready, strategic reason]

---

## Impact Summary

| Segment | Active users | Impact | Migration complexity |
|---------|-------------|--------|---------------------|
| [Segment] | [N] | [H/M/L] | [Low/Med/High] |

**Revenue at risk:** [ARR from affected accounts, if known]
**Contractual review:** [Completed / Needed / N/A]

---

## Migration Path

[What users should do instead, step by step]
[Link to migration docs]
[Feature parity gaps and resolution plan]

---

## Communication Timeline

| Date | Audience | Channel | Message |
|------|---------|---------|---------|
| [date] | [who] | [channel] | [focus] |

---

## Success Metrics

- Migration rate: [X]% by sunset date
- Support ticket volume: <[N] migration tickets/week by T-7
- Post-sunset satisfaction: [CSAT or CSM check-in scores]

---

## Sunset Checklist

[Go/No-Go criteria from Step 5]

---

## Post-Sunset Plan

- Data retention: [How long, then what]
- Monitoring: [What to watch for 30 days post-sunset]
- Rollback trigger: [What would cause us to re-enable]
```

---

## Output Quality Self-Check

- [ ] **Usage data gathered:** Deprecation is based on real usage, not assumption
- [ ] **High-value accounts identified:** Enterprise or high-ARR accounts get direct treatment
- [ ] **Migration path is real:** Not "use the new feature" — step-by-step specific instructions
- [ ] **Feature parity gaps acknowledged:** If the replacement doesn't do everything, it's documented
- [ ] **Communication timeline starts 90 days out:** Not 2 weeks notice for significant changes
- [ ] **Go/No-go criteria defined:** Migration threshold set before communications begin
- [ ] **Output saved:** `outputs/analyses/deprecation-plan-[feature]-[date].md`
