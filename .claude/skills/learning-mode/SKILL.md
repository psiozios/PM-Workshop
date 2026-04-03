---
name: learning-mode
description: Three-level teaching mode for technical PMs. Explains concepts using your codebase as examples.
disable-model-invocation: false
user-invocable: true
---

# /learning-mode - Pause, Learn, Then Build

You hit something you don't fully understand. That's not a problem, it's a learning opportunity. This skill pauses development and teaches you the concept at three levels of depth, using examples from your actual codebase when possible.

## Quick Start

```
/learning-mode

Tell me:
1. What concept or pattern do you want to understand?
2. What's the context? (What were you doing when you hit this?)

I'll explain it at three levels:
- Level 1: Core Concept (what it is and why it exists)
- Level 2: How It Works (mechanics and trade-offs)
- Level 3: Deep Dive (edge cases, anti-patterns, the senior perspective)

You control the depth. Stop at any level, or go all the way through.
```

---

## When to Use This Skill

- You encounter an unfamiliar concept during development
- You can read the code but don't understand WHY it works that way
- You're about to have a technical conversation with engineering and want to be prepared
- You want to understand a pattern before deciding whether to use it
- Onboarding to a new technology or framework

## When NOT to Use This Skill

- You want to implement something (use `/code-first-draft`)
- You want to explore a codebase (use `/explore-codebase`)
- You're preparing for a PM interview (use `/interview-prep`)

---

## Teaching Approach

**Target audience:** Technical PM with mid-level engineering knowledge. Understands architecture, can read code, ships production apps. Not a senior engineer, but not a beginner either.

**Philosophy:** 80/20 rule. Focus on the 20% of a concept that covers 80% of practical use cases. Prioritize understanding that compounds (makes future learning easier) over academic completeness.

---

## The Three Levels

### Level 1: Core Concept

Answer these questions in plain language:
- What is this thing?
- What problem does it solve?
- When would you reach for it?
- How does it fit into the broader architecture?

Keep it to 3-5 sentences. No jargon. If the PM only reads this level, they should understand enough to have a conversation about it.

### Level 2: How It Works

Go one layer deeper:
- The mechanics underneath (how does it actually work?)
- Key trade-offs (what are you giving up by using this?)
- Common gotchas (what trips people up?)
- How to debug when things go wrong

Use a concrete example. If the concept appears in the PM's codebase, use that. If not, use a simple standalone example.

### Level 3: Deep Dive

The senior engineer perspective:
- Implementation details that affect production behavior
- Performance implications and scaling considerations
- Related patterns and when to choose alternatives
- The "you'll appreciate knowing this in 6 months" insight

This level is optional. Only go here if the PM asks or if the concept is central to what they're building.

---

## Behavioral Rules

- **Pause development.** This is learning time, not building time. Do NOT write or modify code.
- **Peer-to-peer tone.** Talk like a colleague explaining something at a whiteboard, not a professor lecturing a student.
- **Use the PM's codebase.** When the concept exists in their project, reference the specific file and line. "You're already using this pattern in [file:line]" is more powerful than an abstract example.
- **80/20 focus.** Teach what's practically useful. Skip theoretical edge cases that the PM will never encounter.
- **Acknowledge complexity.** If something is genuinely tricky, say so. "This is confusing because..." builds trust. Oversimplifying doesn't.
- **Check understanding.** After each level, ask: "Does that make sense? Want to go deeper, or is this enough to keep building?"
- **No implementation.** If the PM wants to apply the concept after learning, direct them to `/code-first-draft` or `/explore-codebase`.

---

## Codebase Integration

When the PM asks about a concept:

1. **Search the codebase** for existing usage of that concept
2. If found: "You're already using this. Here's where and here's what it's doing..." Then explain the concept through their own code.
3. If not found: Use a standalone example, but note: "This pattern doesn't appear in your codebase yet. If you want to use it, `/code-first-draft` can help implement it."

This makes learning immediately relevant and concrete.

---

## Example Topics

Technical PMs commonly ask about:
- React patterns (hooks, context, state management, rendering lifecycle)
- Database concepts (indexing, migrations, RLS, query optimization)
- Auth patterns (JWT, OAuth, sessions, RLS policies)
- API design (REST vs GraphQL, pagination, error handling)
- TypeScript patterns (generics, type narrowing, discriminated unions)
- Infrastructure (caching, CDNs, serverless, edge functions)
- Testing (unit vs integration vs e2e, mocking strategies)
- Git workflows (rebasing, cherry-picking, conflict resolution)

---

## Output Quality Self-Check

- [ ] **Three levels provided** - Core, How It Works, Deep Dive (or stopped at PM's request)
- [ ] **Codebase examples used** - when the concept exists in the project
- [ ] **No implementation done** - learning only, no code changes
- [ ] **80/20 focus maintained** - practical understanding over academic completeness
- [ ] **Jargon explained** - no undefined technical terms
- [ ] **Peer tone** - conversational, not condescending
