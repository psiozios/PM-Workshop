---
name: execution-plan
description: Create tracked markdown execution plans with emoji status tracking and progress percentage
disable-model-invocation: false
user-invocable: true
---

# /execution-plan - Plan the Work, Track the Work

Create a step-by-step execution plan with built-in status tracking. Every step gets an emoji, every plan gets a progress bar. Use this after exploring and before building.

## Quick Start

```
/execution-plan

Tell me:
1. What are we building? (PRD link, exploration output, or describe it)
2. Any constraints? (deadline, dependencies, team size)
3. How granular? (high-level phases or detailed subtasks)

I'll create a tracked plan with status emojis, progress percentage,
critical decisions, and time estimates.

Output: outputs/plans/plan-[project]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Exploration | `outputs/analyses/explore-*.md` | files, dependencies, complexity | Technical understanding and file map |
| PRDs | `context-library/prds/*.md`, `outputs/prds/*.md` | requirements, acceptance criteria | What needs to be built |
| CTO Consult | `outputs/analyses/cto-consult-*.md` | phases, risks, architecture | Phase structure and risk register |
| Past Plans | `outputs/plans/*.md` | plan, progress | Previous plan formats and calibration |

**Cross-Skill Links:**
- Input from → `/explore-codebase` or `/cto-consult` or `/prd-draft`
- Executed by → `/code-first-draft --from-plan`
- Tickets from → `/create-tickets` to break into engineering tickets
- Sprint assignment → `/sprint-planning`

---

## When to Use This Skill

- After `/explore-codebase` when you understand the problem and are ready to plan
- Before starting multi-step implementation work
- When coordinating complex work across multiple sessions
- When you want a visual tracker for a project

## When NOT to Use This Skill

- Single-step changes (just do it)
- You're still exploring (use `/explore-codebase` first)
- You need strategic planning, not execution planning (use `/write-prod-strategy`)

---

## Workflow

### Step 1: Gather Context

Read the inputs:
- PRD or feature description
- Exploration output (if `/explore-codebase` was run)
- CTO consultation (if `/cto-consult` was run)
- Any constraints the PM provides

### Step 2: Break Into Phases

Structure the work into logical phases:
- **Foundation:** Setup, infrastructure, data models
- **Core:** Main feature implementation
- **Polish:** Edge cases, error handling, UX refinement
- **Launch:** Testing, documentation, deployment

Not every project needs all phases. Small projects might be a single phase.

### Step 3: Break Phases Into Atomic Steps

Each step should be:
- Completable in one focused session (1-3 hours)
- Independently testable (you can verify it works before moving on)
- Clear enough that someone could start without asking questions

### Step 4: Identify Dependencies and Blockers

- Which steps depend on other steps?
- Which steps can run in parallel?
- What external dependencies exist? (APIs, design assets, approvals)
- What decisions must be made before proceeding?

### Step 5: Estimate and Deliver

- Add time estimates per step
- Calculate overall progress percentage
- Flag critical decisions that block progress
- Deliver the plan

---

## Status Emoji Legend

| Emoji | Status | Meaning |
|-------|--------|---------|
| :white_large_square: | Not started | Work hasn't begun |
| :yellow_square: | In progress | Currently being worked on |
| :green_square: | Done | Completed and verified |
| :red_square: | Blocked | Can't proceed (dependency or decision needed) |

---

## Output Template

```markdown
# Execution Plan: [Project Name]

**Overall Progress:** 0% (0/N steps complete)
**Created:** [date]
**Last Updated:** [date]
**Estimated Total Time:** [X hours]

---

## TLDR

[2-3 sentences max. What are we building and why.]

---

## Critical Decisions

Decisions that must be made before or during implementation:

- [ ] **[Decision 1]:** [Options and trade-offs] - Status: OPEN
- [ ] **[Decision 2]:** [Options and trade-offs] - Status: OPEN

---

## Phase 1: [Phase Name]

**Estimated time:** [X hours]

- [ ] :white_large_square: **Step 1: [Name]**
  - [What to do, which files to touch]
  - Depends on: nothing
  - [ ] :white_large_square: Subtask 1
  - [ ] :white_large_square: Subtask 2

- [ ] :white_large_square: **Step 2: [Name]**
  - [What to do]
  - Depends on: Step 1
  - [ ] :white_large_square: Subtask 1

---

## Phase 2: [Phase Name]

**Estimated time:** [X hours]

- [ ] :white_large_square: **Step 3: [Name]**
  - [What to do]
  - Depends on: Phase 1 complete
  - [ ] :white_large_square: Subtask 1

---

## Dependencies

| Step | Depends On | Type |
|------|-----------|------|
| Step 2 | Step 1 | Sequential |
| Step 4 | Step 3 | Sequential |
| Step 3 | Step 2 | Can run in parallel after Phase 1 |

---

## Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| [Risk 1] | [What happens if it hits] | [How to handle] |
```

---

## Behavioral Rules

- **Don't over-plan.** If the project is simple, make it one phase with a few steps. Match plan complexity to project complexity.
- **Steps must be atomic.** Each step should be small enough to complete and verify in one sitting.
- **Include the "why" in the TLDR.** Someone reading just the TLDR should understand the purpose.
- **Flag decisions, don't make them.** If a decision needs PM input, mark it OPEN and describe the trade-offs.
- **Be honest about estimates.** Round up. Things always take longer than you think.

---

## Output Quality Self-Check

- [ ] **TLDR is genuinely brief** - 2-3 sentences, not a paragraph
- [ ] **Every step is atomic** - completable in one session
- [ ] **Dependencies identified** - no step has hidden blockers
- [ ] **Critical decisions listed** - decisions that block progress are flagged
- [ ] **Time estimates provided** - per step and overall
- [ ] **Progress tracking works** - emoji statuses are correctly set
- [ ] **Output saved:** `outputs/plans/plan-[project]-[date].md`
