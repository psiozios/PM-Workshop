---
name: explore-codebase
description: Pre-implementation codebase exploration and problem space mapping. Understand before you build.
disable-model-invocation: false
user-invocable: true
---

# /explore-codebase - Understand Before You Build

Your job is NOT to implement. It's to fully understand the problem space, map the relevant code, and surface questions before anyone writes a line of code. This is the "think before you build" checkpoint.

## Quick Start

```
/explore-codebase

Tell me:
1. What are you trying to build or fix?
2. Where in the codebase do you think it lives? (or "I have no idea")
3. Any constraints I should know about? (timeline, tech debt, specific patterns to follow)

I'll explore, map dependencies, identify edge cases, and ask you the questions
that matter before you start coding.

Output: outputs/analyses/explore-[topic]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| PRDs | `context-library/prds/*.md`, `outputs/prds/*.md` | feature, requirement, scope | What was specified vs what exists |
| Strategy | `context-library/strategy/*.md` | priority, roadmap, milestone | Strategic context for the work |
| Past Analyses | `outputs/analyses/*.md` | explore, cto-consult | Prior exploration or CTO consultations |
| Codebase | Project source files | imports, exports, function signatures | Architecture, patterns, dependencies |

**Cross-Skill Links:**
- After exploration → `/execution-plan` to create a tracked plan
- After exploration → `/code-first-draft` to implement
- Complex architecture question → `/cto-consult` for pushback
- Unknown concept encountered → `/learning-mode` to understand it

---

## When to Use This Skill

- Before `/code-first-draft` on any non-trivial feature
- When evaluating if a PRD is technically feasible
- When onboarding to an unfamiliar codebase or module
- When debugging something you don't fully understand yet
- When you need to explain technical constraints to stakeholders

## When NOT to Use This Skill

- Simple one-file changes (just read the file and do it)
- You already understand the code well
- You want to implement something (use `/code-first-draft`)

---

## Workflow

### Step 1: Understand the Request

Ask clarifying questions until the problem is clear. Don't explore blindly.

Questions to ask:
- What's the user-facing behavior you want?
- What exists today? (working feature to extend, or greenfield?)
- Are there patterns in the codebase you want me to follow?
- Any files or modules you already suspect are involved?

### Step 2: Detect and Map the Codebase

Explore the project structure:
- Framework and language detection
- Directory structure and naming conventions
- Key configuration files (package.json, tsconfig, etc.)
- Existing patterns (state management, routing, API layer, auth)

### Step 3: Trace the Relevant Code Path

For the specific problem:
- Identify entry points (routes, handlers, components)
- Follow the data flow (user action → UI → API → DB → response)
- Map file dependencies (imports/exports graph)
- Note shared utilities and helpers that could be reused

### Step 4: Identify Edge Cases and Risks

- What could break? (race conditions, null states, auth gaps)
- What's fragile? (tightly coupled code, no tests, hardcoded values)
- What's missing? (no error handling, no loading states, no validation)
- What assumptions does the existing code make?

### Step 5: Ask Follow-Up Questions

Based on what you found, surface questions that matter:
- Ambiguities in the requirements
- Architecture decisions that need to be made
- Trade-offs between approaches
- Dependencies on other work

### Step 6: Deliver Exploration Summary

Write a clear summary that someone could use to start implementing.

---

## Behavioral Rules

- **NEVER write or modify code.** This is read-only exploration.
- **Ask questions.** Don't assume requirements. If something is ambiguous, ask.
- **Map dependencies visually** when helpful (ASCII diagrams for data flow).
- **Be honest about complexity.** If something is harder than it looks, say so.
- **Note reusable code.** If a utility or pattern already exists that solves part of the problem, call it out.
- **Keep it conversational.** This is a back-and-forth, not a monologue. Explore, ask, explore more.

---

## Output Template

```markdown
# Codebase Exploration: [Topic]

**Date:** [date]
**Problem:** [What we're trying to build or fix]

---

## Codebase Overview

**Framework:** [e.g., React + TypeScript, Next.js, etc.]
**Key patterns:** [State management, API layer, component structure]
**Relevant conventions:** [Naming, file organization, test patterns]

---

## Relevant Files

| File | Purpose | Relevance |
|------|---------|-----------|
| [path] | [what it does] | [why it matters for this work] |
| [path] | [what it does] | [why it matters] |

---

## Data Flow

[How data moves through the system for this feature]
[ASCII diagram if helpful]

---

## Reusable Code

- **[utility/component name]** at [path] - [what it does, how it helps]
- **[pattern name]** - [existing pattern to follow]

---

## Edge Cases and Risks

1. [Risk] - [Why it matters] - [Mitigation]
2. [Risk] - [Why it matters] - [Mitigation]

---

## Open Questions

- [ ] [Question that needs answering before implementation]
- [ ] [Decision that needs to be made]

---

## Complexity Assessment

**Estimated complexity:** [Low / Medium / High]
**Reasoning:** [Why]
**Suggested approach:** [Brief recommendation for how to tackle this]
```

---

## Output Quality Self-Check

- [ ] **No code written or modified** - exploration only
- [ ] **Dependencies mapped** - relevant files and their relationships identified
- [ ] **Edge cases surfaced** - at least 2-3 risks or edge cases noted
- [ ] **Questions asked** - ambiguities surfaced, not assumed away
- [ ] **Reusable code identified** - existing utilities and patterns noted
- [ ] **Complexity honestly assessed** - not under- or over-estimated
- [ ] **Output saved:** `outputs/analyses/explore-[topic]-[date].md`
