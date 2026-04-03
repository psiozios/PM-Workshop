---
name: code-review
description: Comprehensive code review checking production readiness, error handling, security, and architecture
disable-model-invocation: false
user-invocable: true
---

# /code-review - Catch It Before It Ships

Comprehensive code review across 8 dimensions. Finds real issues, skips nitpicks, and gives you a clear pass/fail recommendation with severity-tagged findings.

## Quick Start

```
/code-review

Options:
- /code-review              — review staged/changed files (git diff)
- /code-review [file/dir]   — review specific files or directory
- /code-review --focus security  — focus on one dimension
- /code-review --focus performance

I'll check error handling, types, production readiness, security, performance,
architecture, and testing. Each issue gets a severity level and suggested fix.

Output: outputs/analyses/code-review-[scope]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Changed Files | `git diff`, `git status` | staged, modified | Files to review |
| PRDs | `context-library/prds/*.md`, `outputs/prds/*.md` | requirements, acceptance | What the code should do |
| Past Reviews | `outputs/analyses/code-review-*.md` | issues, patterns | Recurring problems |
| Codebase | Project source files | patterns, conventions | What "correct" looks like here |

**Cross-Skill Links:**
- After → `/code-first-draft` to review generated code
- Pair with → `/peer-review` to verify findings from another AI
- Issues found → `/create-tickets` or `/create-tickets --quick` to log fixes
- Pre-launch → `/launch-checklist` includes review as a gate

---

## When to Use This Skill

- Before merging a PR or shipping code
- After `/code-first-draft` generates a feature
- Before a launch as a final quality gate
- When inheriting unfamiliar code you need to trust
- When you want a second opinion on your own code

---

## Review Dimensions

### 1. Error Handling and Logging
- Try-catch blocks around async operations
- Proper error propagation (not swallowed errors)
- Logging with context (not bare `console.log`)
- User-facing error messages are helpful
- Centralized error handlers used where available

### 2. Type Safety (TypeScript/typed languages)
- No `any` types without justification
- Proper interfaces and type definitions
- No `@ts-ignore` or `@ts-expect-error` without comment
- Function return types specified for non-trivial functions
- Generics used appropriately

### 3. Production Readiness
- No `console.log` debug statements left behind
- No `TODO` or `FIXME` in shipped code (unless tracked as tickets)
- No hardcoded secrets, API keys, or credentials
- Environment variables used for configuration
- No commented-out code blocks

### 4. React/Hooks (if applicable)
- Effects have proper cleanup functions
- Dependency arrays are complete and correct
- No infinite render loops
- Custom hooks follow naming conventions
- State updates are batched where possible

### 5. Performance
- No unnecessary re-renders (missing memoization)
- Expensive calculations wrapped in useMemo/useCallback
- No N+1 query patterns
- Large lists use virtualization or pagination
- Images and assets are optimized

### 6. Security
- Authentication checked on protected routes/endpoints
- Input validation on user-supplied data
- No SQL injection vectors (parameterized queries)
- No XSS vectors (sanitized output)
- RLS policies in place (if using Supabase/similar)
- No sensitive data in client-side code or logs

### 7. Architecture
- Follows existing codebase patterns and conventions
- Code is in the correct directory/module
- Separation of concerns (no business logic in UI components)
- Naming is clear and consistent
- No circular dependencies

### 8. Testing
- Critical paths have test coverage
- Tests are meaningful (not just "it renders")
- Edge cases tested (empty states, error states, boundaries)
- Test data is realistic
- No flaky test patterns

---

## Severity Levels

| Level | Meaning | Action Required |
|-------|---------|-----------------|
| **CRITICAL** | Security vulnerability, data loss risk, crash in production | Must fix before merge |
| **HIGH** | Bug, performance issue, bad user experience | Should fix before merge |
| **MEDIUM** | Code quality, maintainability concern | Fix soon, can merge with ticket |
| **LOW** | Style, minor improvement, nice-to-have | Optional, note for future |

---

## Output Template

```markdown
# Code Review: [Scope]

**Date:** [date]
**Files reviewed:** [count]
**Recommendation:** [PASS / PASS WITH NOTES / FAIL]

---

## Looks Good

- [Thing that's well done - be specific]
- [Another positive finding]

---

## Issues Found

### CRITICAL

- **[File:line]** - [Issue description]
  - Why it matters: [impact]
  - Fix: [suggested fix]

### HIGH

- **[File:line]** - [Issue description]
  - Fix: [suggested fix]

### MEDIUM

- **[File:line]** - [Issue description]
  - Fix: [suggested fix]

### LOW

- **[File:line]** - [Issue description]

---

## Summary

| Severity | Count |
|----------|-------|
| Critical | X |
| High | X |
| Medium | X |
| Low | X |
| **Total** | **X** |

**Recommendation:** [PASS / PASS WITH NOTES / FAIL]
**Rationale:** [One sentence explaining the recommendation]
```

---

## Behavioral Rules

- **Read the full context** before flagging an issue. A pattern that looks wrong in one file might be intentional given the broader architecture.
- **Be specific.** "[File:line] - missing null check on user.email before toLowerCase()" beats "potential null reference."
- **Suggest fixes, don't just complain.** Every issue should have a suggested resolution.
- **Skip nitpicks.** Style preferences that don't affect correctness or readability are not worth flagging.
- **Acknowledge what's good.** Start with what's working well. Engineers (and PMs who code) need to know what to keep doing.
- **Focus mode:** When `--focus [dimension]` is used, go deep on that dimension only. Skip the rest.

---

## Output Quality Self-Check

- [ ] **Every finding has a severity level** - CRITICAL, HIGH, MEDIUM, or LOW
- [ ] **Every finding references specific file and line** - not vague
- [ ] **Every finding includes a suggested fix** - actionable, not just a complaint
- [ ] **Positives acknowledged** - at least 2-3 things done well
- [ ] **No false positives** - each issue verified against actual code context
- [ ] **Recommendation is clear** - PASS, PASS WITH NOTES, or FAIL with rationale
- [ ] **Output saved:** `outputs/analyses/code-review-[scope]-[date].md`
