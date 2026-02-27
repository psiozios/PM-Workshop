---
name: post-mortem
description: Run a structured post-mortem after an incident, failed launch, or missed goal. Establishes a blameless timeline, identifies root causes, drives prevention actions, and documents learnings for the team.
disable-model-invocation: false
user-invocable: true
---

# /post-mortem - Learn From What Went Wrong

Post-mortems only work if they're blameless, fast, and action-oriented. This skill runs you through a structured process in under 90 minutes — then produces a document teams will actually read.

## Quick Start

```
/post-mortem

Tell me:
1. What happened? (incident, failed launch, missed OKR, escalation)
2. When did it start and when was it resolved?
3. What was the user/business impact? (users affected, revenue impact, time down)
4. What's already known? (paste timeline notes, Slack threads, or incident log)

I'll check context-library/ for relevant PRDs, launch plans, and prior incidents.

Output: outputs/post-mortems/post-mortem-[incident]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Launches | `context-library/launches/*.md` | [feature/product name], launch, go-live | Launch plan context — what was expected |
| PRDs | `context-library/prds/*.md` | [feature name], risk, assumption | Design assumptions that may have broken |
| Metrics | `context-library/metrics/*.md` | alert, anomaly, incident, SLA | Baseline and incident data |
| Past Post-Mortems | `context-library/decisions/*.md` | post-mortem, incident, retrospective | Prior learnings — are we repeating patterns? |

**Cross-Skill Links:**
- Root cause investigation → `/root-cause-analysis`
- Prevention planning → `/pre-mortem` for the next launch
- Stakeholder communication → `/status-update` for incident comms
- Ticket creation for fixes → `/create-tickets`

---

## Blameless Principles (Read This First)

A good post-mortem produces system improvements, not scapegoats.

**Ground rules:**
- No names in causal chains. "The deploy script failed" not "Bob's deploy script failed."
- Assume everyone acted with the best information they had at the time.
- "Why did the system allow this?" is more useful than "Why did the person do this?"
- The goal is learning and prevention, not accountability or punishment.

**The right question:** "What made this failure mode possible?"
**Not:** "Who made the mistake?"

---

## Phase 1: Build the Timeline

Construct a chronological timeline of what happened, what people knew, and what they did.

```
## Incident Timeline

| Time (UTC) | Event | Who/What | Signal or action |
|-----------|-------|---------|-----------------|
| [time] | [Event: deploy, alert, user report, etc.] | [System/person] | [What was observed or done] |
| [time] | [Event] | | |
| [time] | Incident detected | [How: alert, user report, manual] | |
| [time] | Response initiated | [Who was paged/alerted] | |
| [time] | First mitigation attempted | | [Result] |
| [time] | Root cause identified | | |
| [time] | Fix deployed / incident mitigated | | |
| [time] | Incident resolved / all clear | | |

**Total duration:** [X hours Y minutes]
**Time to detect:** [Time from start to first detection]
**Time to mitigate:** [Time from detection to user impact ended]
**Time to resolve:** [Time from detection to fully resolved]
```

---

## Phase 2: Impact Assessment

Quantify the blast radius before digging into causes.

```
## Impact

**User impact:**
- Users affected: [N] (estimated or confirmed)
- User segments affected: [new users / enterprise / all users / specific cohort]
- Workflows affected: [What users couldn't do during the incident]

**Business impact:**
- Revenue impact: $[X] (estimated — lost transactions, SLA credits, etc.)
- SLA breach: [Yes/No — and if yes, which accounts and SLA terms]
- Support ticket spike: [N tickets during incident]

**Reputation/trust impact:**
- Public mentions: [Social, G2, user community]
- Enterprise escalations: [N accounts escalated]

**Data impact:** [Was any data lost, corrupted, or exposed? Y/N — critical to call out immediately]
```

---

## Phase 3: Root Cause Analysis

Use the 5 Whys or fishbone method (see `/root-cause-analysis` for depth). Keep it blameless.

```
## Root Cause Analysis

**Immediate cause:** [The direct trigger — "deploy introduced a null pointer exception"]

**5 Whys:**
Why 1: [Why did the immediate cause happen?]
Why 2: [Why did that happen?]
Why 3: [Why did that happen?]
Why 4: [Why did that happen?]
Why 5: [Why did that happen?]

**Root cause:** [The systemic issue — what made this failure mode possible]

**Contributing factors:**
- [Factor 1: monitoring gap, process gap, design assumption, etc.]
- [Factor 2]

**What went right:** [Always document this — what limited the blast radius or sped up recovery]
```

---

## Phase 4: Response Quality Assessment

How did the team do during the incident itself?

| Area | What happened | Assessment |
|------|--------------|-----------|
| Detection | [How was it caught? How long did it take?] | [Fast / Slow / Missed for too long] |
| Escalation | [Was the right team alerted? How fast?] | [Good / Slow / Missed escalation] |
| Communication | [Internal comms? External status page?] | [Clear / Confusing / Missing] |
| Mitigation | [What was tried? What worked?] | [Effective / Too slow / Wrong approach first] |
| Runbook | [Did a runbook exist? Was it useful?] | [Used / Outdated / Didn't exist] |

---

## Phase 5: Action Items

Every post-mortem must produce concrete, assigned, time-bound actions.

```
## Action Items

| Action | Type | Owner | Due | Ticket |
|--------|------|-------|-----|--------|
| [Add alerting for X] | Prevention | [team] | [date] | [LIN-XXX] |
| [Update runbook for Y] | Process | [team] | [date] | [LIN-XXX] |
| [Fix root cause Z] | Fix | [team] | [date] | [LIN-XXX] |
| [Add test coverage for W] | Prevention | [team] | [date] | [LIN-XXX] |

Action types:
- Prevention: Stops this from happening again
- Detection: Catches it faster if it does happen
- Mitigation: Reduces impact when it occurs
- Process: Documentation, training, runbook update
- Fix: Addresses the immediate cause
```

**Quality bar for actions:**
- Specific (what exactly will be done)
- Owned (one named person or team)
- Time-bound (specific date, not "soon")
- Trackable (link to a ticket)

---

## Output Template

```markdown
# Post-Mortem: [Incident Name]

**Date:** [post-mortem date]
**Incident date:** [when it happened]
**Severity:** [P0 (all users down) / P1 (major degradation) / P2 (partial impact)]
**Status:** [Draft / Reviewed / Action items tracked]
**Facilitator:** [PM or EM who ran the post-mortem]

---

## Summary

[3-5 sentences: what happened, impact, root cause, how resolved, key learning]

---

## Timeline

[Table from Phase 1]

---

## Impact

[Section from Phase 2]

---

## Root Cause

[Section from Phase 3]

---

## Response Quality

[Table from Phase 4]

---

## What Went Well

- [Positive 1]
- [Positive 2]
(Don't skip this — understanding what limited damage is as valuable as understanding what caused it)

---

## Action Items

[Table from Phase 5]

---

## Distribution

**Shared with:** [Engineering / Leadership / CS / Customers (if applicable)]
**Customer communication:** [Link to status page update or incident email]
```

---

## Facilitation Guide (45-90 min session)

| Time | Activity |
|------|----------|
| 0-5 min | Ground rules: blameless, collaborative, learning-focused |
| 5-20 min | Walk the timeline — everyone who was involved adds their perspective |
| 20-35 min | Impact confirmation — agree on numbers |
| 35-55 min | Root cause discussion — 5 Whys as a group |
| 55-70 min | Response quality assessment — what worked, what didn't |
| 70-85 min | Action items — assign, date, ticket |
| 85-90 min | Close — key learning, who will publish the doc |

---

## Output Quality Self-Check

- [ ] **Blameless tone maintained:** No individual named in causal chains
- [ ] **Timeline is objective:** Facts and timestamps, not interpretations
- [ ] **Root cause is systemic:** Not "human error" — what system allowed the error
- [ ] **Impact is quantified:** User count, revenue impact, time down — not vague
- [ ] **What went well is documented:** Doesn't just focus on failures
- [ ] **All actions are assigned and dated:** No "we should improve X" without an owner
- [ ] **Output saved:** `outputs/post-mortems/post-mortem-[incident]-[date].md`
