---
name: analytics-instrumentation
description: Design tracking plans for product features. Define events, properties, user identification, and measurement strategy before engineering ships. Ensures the data you need to evaluate features actually gets captured.
disable-model-invocation: false
user-invocable: true
---

# /analytics-instrumentation - Measure Before You Ship

Features without tracking are guesses. This skill designs the measurement layer before engineering touches a line of code.

## Quick Start

```
/analytics-instrumentation

Tell me:
1. Feature name and brief description
2. What questions you need the data to answer (success metrics or hypotheses)
3. Your analytics tool (Amplitude, Mixpanel, Posthog, Segment, GA4, or unknown)
4. Any existing events or user properties already tracked (or "I don't know")

I'll check context-library/prds/ for the feature spec and context-library/metrics/ for existing tracking patterns.

Output: outputs/analyses/tracking-plan-[feature]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| PRDs | `context-library/prds/*.md` | feature, user story, success metric | What behavior to measure |
| Metrics | `context-library/metrics/*.md` | event, property, funnel, tracking | Existing events and naming conventions |
| Feature Metrics | `context-library/prds/*.md` | STEDII, primary metric, guardrail | Metrics the PM already defined |
| Strategy | `context-library/strategy/*.md` | North Star, KPI | How events ladder to business metrics |

**Cross-Skill Links:**
- Define what to measure → `/feature-metrics` (STEDII framework)
- Experiment design → `/experiment-metrics` for trustworthy experiment metrics
- Post-launch analysis → `/feature-results` using the events you define here
- Activation funnel → `/activation-analysis` using funnel events

---

## Step 1: Define Measurement Goals

Before writing a single event name, answer:

| Question | Why it matters |
|----------|---------------|
| What is the primary success metric for this feature? | Determines what events are critical vs. nice-to-have |
| What user behaviors indicate success? | These become your key events |
| What behaviors indicate failure or friction? | These become your error and abandonment events |
| What segmentation will you need? | Determines which user properties to capture |
| When will you review the data? (sprint / 30 days / 60 days) | Determines how fast events need to be in place |

---

## Step 2: Design the Event Schema

### Event Naming Convention

Use a consistent verb-object-context pattern:
- `feature_viewed` — user saw the feature
- `feature_started` — user initiated an action
- `feature_completed` — user finished the action
- `feature_abandoned` — user left without completing
- `feature_errored` — something went wrong
- `feature_upgraded` — user hit a paywall or limit

**Naming rules:**
- Snake_case, lowercase always
- Verb first (action-oriented)
- Specific over generic: `export_csv_completed` beats `export_completed`
- Consistent with existing events in your schema

### Event Taxonomy

| Event type | When to fire | Priority |
|-----------|-------------|----------|
| Entry / view | User lands on the feature | P0 |
| Initiation | User starts the primary action | P0 |
| Completion | User successfully completes the action | P0 |
| Abandonment | User exits before completing | P1 |
| Error | Action fails or user hits an error state | P1 |
| Secondary actions | Additional interactions within the feature | P2 |
| Discovery | How users found or learned about the feature | P2 |

---

## Step 3: Write the Tracking Plan

### Tracking Plan Template (per event)

```
## Event: [event_name]

**Trigger:** [Exact user action that fires this event — be specific]
Example: "User clicks 'Export CSV' button on the Reports page"

**When:** [Timing — on click, on completion, on error, on load]

**Properties:**

| Property name | Type | Values | Required | Notes |
|---------------|------|--------|----------|-------|
| user_id | string | unique ID | Yes | Standard — already tracked |
| feature_name | string | "csv_export" | Yes | Hardcoded |
| report_type | enum | "summary" / "detail" / "raw" | Yes | Which report type |
| row_count | integer | number | No | Only available on completion |
| time_to_complete_ms | integer | milliseconds | No | Only on completion |
| error_code | string | "timeout" / "permission" | No | Only on error events |

**Success criteria this event measures:** [Which metric this feeds into]
**Owner:** [Who writes the tracking code — frontend, backend, or both]
```

---

## Step 4: User Identification and Properties

### User-Level Properties to Capture or Confirm

For any feature with segmentation needs, confirm these are being tracked:

| Property | Type | Example values | Priority |
|----------|------|---------------|----------|
| user_id | string | UUID | P0 |
| account_id | string | UUID | P0 |
| plan_tier | enum | free / pro / enterprise | P0 |
| account_age_days | integer | days since signup | P1 |
| user_role | enum | admin / member / viewer | P1 |
| is_paying | boolean | true / false | P1 |
| company_size | enum | 1-10 / 11-50 / 51-200 / 200+ | P2 |

**Don't create new user properties if existing ones cover the need.** Check with your analytics or data engineering team for the current schema.

---

## Step 5: Funnels and Derived Metrics

Define the funnels and metrics you expect to calculate from your events:

### Key Funnel (adjust for your feature)

```
Step 1: [feature]_viewed
Step 2: [feature]_started
Step 3: [intermediate step if applicable]
Step 4: [feature]_completed

Conversion rate: Step 1 → Step 4
Drop-off analysis: Which step has the highest abandonment?
Time-to-complete: Delta between Step 1 timestamp and Step 4 timestamp
```

### Derived Metrics Table

| Metric name | Formula | Target | Used in |
|-------------|---------|--------|---------|
| Feature adoption rate | unique users who completed / total active users | [X]% in 30 days | Dashboard |
| Completion rate | completed events / started events | >[X]% | Sprint review |
| Error rate | error events / started events | <[X]% | Alert |
| Time to value | avg time from feature_viewed to feature_completed | <[X] seconds | Activation analysis |

---

## Step 6: QA and Validation Checklist

Before calling tracking done, verify:

```
Tracking Plan QA Checklist

□ Events fire in the correct sequence (view → start → complete)
□ No duplicate events fire for the same user action
□ Events fire on all supported platforms (web / mobile / API if applicable)
□ Required properties are never null when event fires
□ Optional properties are null when not available (not empty string or "undefined")
□ User ID is always resolved before events fire (no anonymous events for logged-in users)
□ Events appear in analytics tool within expected latency (usually <1 minute for realtime)
□ Funnel can be built in analytics tool using the events defined
□ A/B test variant property included if this feature is in an experiment
```

---

## Output Template

```markdown
# Tracking Plan: [Feature Name]

**Date:** [date]
**Feature owner:** [PM name]
**Analytics tool:** [Amplitude / Mixpanel / Posthog / GA4 / Segment / Other]
**Review date:** [When to review data after launch]

---

## Measurement Goals

**Primary question this tracking answers:** [One sentence]
**Primary success metric:** [Metric name, definition, target]
**Guardrail metrics:** [What we're watching to make sure we don't break something]

---

## Events

### Critical Events (P0 — must be in place before launch)

| Event name | Trigger | Key properties |
|-----------|---------|---------------|
| [event] | [when fired] | [prop1, prop2] |

### Secondary Events (P1 — ship with feature or within 1 week)

| Event name | Trigger | Key properties |
|-----------|---------|---------------|
| [event] | [when fired] | [prop1, prop2] |

---

## Key Funnel

[event_1] → [event_2] → [event_3] → [event_4]

---

## User Properties Required

[Table of user properties needed for segmentation]

---

## Dashboard / Monitoring Plan

- Where will this be monitored: [Dashboard name or analytics board]
- Alert threshold: [If error rate > X% or adoption < Y% by Z days]
- Review cadence: [Weekly / Per sprint / 30 days post-launch]

---

## Handoff Notes

- **Frontend tracking owner:** [Name]
- **Backend tracking owner:** [Name if applicable]
- **QA validation:** [Who will verify events are firing correctly]
- **Due before launch:** [Date]
```

---

## Output Quality Self-Check

- [ ] **Events cover the full funnel:** View, start, complete, and error states all defined
- [ ] **Naming convention consistent:** Matches existing schema or new convention is documented
- [ ] **Properties typed correctly:** No ambiguity on what each property contains
- [ ] **User properties confirmed:** Not creating duplicate properties that already exist
- [ ] **Funnels derivable:** The events defined make the funnels you need buildable
- [ ] **QA checklist included:** Engineering knows what to validate before launch
- [ ] **Output saved:** `outputs/analyses/tracking-plan-[feature]-[date].md`
