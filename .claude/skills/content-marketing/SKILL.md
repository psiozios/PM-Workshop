---
name: content-marketing
description: Create product-led content assets. Covers launch announcements, changelogs, release notes, in-app messaging, onboarding copy, and case study outlines. Helps PMs write and brief content that drives activation, retention, and product awareness.
disable-model-invocation: false
user-invocable: true
---

# /content-marketing - Product Content That Moves Metrics

PMs don't always own content, but they're always accountable for how features land with users. This skill helps you write or brief every content touchpoint in the product lifecycle.

## Quick Start

```
/content-marketing

Modes:
--announce    Launch announcement (blog, email, in-app, social)
--changelog   Product changelog entry
--release     Full release notes (internal or external)
--onboarding  In-product onboarding copy (tooltips, empty states, walkthroughs)
--case-study  Customer case study outline
--brief       Content brief for a marketing or content team

Tell me:
1. Mode (or describe what you need to write)
2. Feature or product being communicated
3. Primary audience (new users / existing users / prospects / press)
4. Key message (what you most want them to understand or do)

I'll check context-library/ for product context and writing style preferences.

Output: outputs/content/[type]-[feature]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| PRDs | `context-library/prds/*.md` | feature name, user story, benefit | What the feature does and for whom |
| Writing Style | `context-library/writing-style-*.md` | tone, voice, audience | Voice and style preferences |
| Research | `context-library/research/*.md` | user quote, pain point, outcome | Real customer language to borrow |
| Launches | `context-library/launches/*.md` | messaging, positioning, GTM | Prior launch context |
| Strategy | `context-library/strategy/*.md` | positioning, value prop, North Star | Strategic framing |

**Cross-Skill Links:**
- Launch assets → `/launch-checklist` to track content deliverables
- Feature context → `/prd-draft` for the underlying feature spec
- Customer success data → `/feature-results` for metrics to include in case studies
- Messaging consistency → `/write-prod-strategy` (positioning section)

---

## Mode: --announce (Launch Announcement)

Announcements fail when they lead with what you built. Lead with what changed for the user.

### Announcement Structure

```
HEADLINE: [The outcome/benefit — not the feature name]
Bad: "Introducing Advanced Export Settings"
Good: "Finally, export exactly the data your finance team needs"

HOOK (1 sentence): [The problem it solves]
"If you've spent time manually reformatting exports before sharing with stakeholders, this is for you."

THE THING (1-2 sentences): [What you shipped — specific, concrete]
"You can now [specific action] with [specific options]. [Second capability if relevant]."

THE BENEFIT (1-2 sentences): [What changes for them]
"That means [specific outcome]. [User type] will spend [X less time] on [painful task]."

SOCIAL PROOF (optional): [Customer quote or beta stat]
"[Customer name]: '[Quote about the value]'"

CTA: [Single, specific action]
"[Button text or link text]"
```

### Announcement Channel Variations

| Channel | Length | Tone | Key focus |
|---------|--------|------|-----------|
| In-app banner | 1 sentence + CTA | Friendly, action-forward | Immediate benefit |
| Email (existing users) | 150-300 words | Conversational | What changes for you |
| Blog post | 400-800 words | Educational | Problem + solution + context |
| LinkedIn | 3-5 short paragraphs | Professional, direct | Insight + feature + proof |
| Twitter/X | 1-3 tweets | Punchy | Hook + feature + link |
| Changelog | 50-100 words | Concise, specific | What changed, for whom |

---

## Mode: --changelog (Changelog Entry)

Good changelogs are read. Bad ones are ignored.

### Changelog Entry Template

```
## [Version or date] — [Month Day, Year]

### What's new

**[Feature name]** — [1-sentence description of what it does and for whom]

[Optional: 1-2 additional sentences if the feature needs context to understand its value]

### Fixed
- [Bug description] that affected [who] when [condition]
- [Bug description]

### Improved
- [Performance or UX improvement] — [specific, quantified if possible: "40% faster" beats "faster"]

### Coming soon
- [Brief preview if appropriate]
```

**Changelog rules:**
- Lead with what the user can now do, not what engineering changed
- One entry per meaningful change — don't group unrelated items under "bug fixes"
- Version numbers should link to release notes if they exist
- "Improved performance" is not a changelog entry — say what specifically improved and by how much

---

## Mode: --release (Release Notes)

Release notes are for users who want the full picture, not just the headline.

### External Release Notes Structure

```
# Release Notes — [Version] — [Date]

## Highlights

[2-3 most important changes, 1 sentence each]

## New Features

### [Feature Name]
**Who it's for:** [User persona or segment]
**What it does:** [Clear description of the capability]
**How to access it:** [Where to find it in the product]
**Before vs. after:** [Optional — what the user had to do before and what's different now]

[Repeat for each new feature]

## Improvements

- **[Area]:** [What changed and why it's better]
- **[Area]:** [What changed]

## Bug Fixes

- Fixed: [Specific issue] that caused [specific problem] for users who [specific condition]
- Fixed: [Issue]

## Breaking Changes (if applicable)

[If anything breaks existing workflows, call it out prominently and link to migration guide]

## Coming Next

[Optional teaser of what's on the near-term roadmap]
```

---

## Mode: --onboarding (In-Product Copy)

Onboarding copy determines whether users get to value or give up.

### Copy by Component Type

**Empty state** (user hasn't created anything yet):
```
Headline: [What they'll be able to do — action-oriented]
Subtext: [1 sentence on why it matters or how to get started]
CTA: [First action — verb + noun, specific]

Example:
Headline: "Your first report lives here"
Subtext: "Connect a data source and we'll build your first dashboard in under 2 minutes."
CTA: "Connect data source"
```

**Tooltip** (explaining a feature inline):
```
Rule: Explain the benefit, not the feature. Max 2 sentences.
Bad: "This button opens the export panel where you can choose your format."
Good: "Export to CSV, PDF, or Excel — your finance team gets exactly the format they want."
```

**Onboarding checklist item:**
```
[Action verb] + [specific object] + [optional: so that outcome]
Bad: "Customize your profile"
Good: "Add your logo so reports look like they're from your team"
```

**Success message / confirmation:**
```
[Acknowledge] + [next step or benefit unlocked]
Bad: "Report created successfully."
Good: "Your report is ready. Share it with your team or schedule a weekly digest."
```

---

## Mode: --case-study (Customer Case Study Outline)

### Case Study Structure

```
TITLE: How [Customer] [achieved outcome] with [Your Product]
Subtitle: "[Direct quote from customer about the result]"

## The Customer
[2-3 sentences: who they are, their size, their market, why they matter as a reference]

## The Challenge
[3-5 sentences: specific problem they had before your product. Be specific — "spent 12 hours a week" beats "spent a lot of time"]

## Why They Chose [Your Product]
[What made them choose you over alternatives. Be honest about the alternatives they considered.]

## The Implementation
[Brief: how they got started, how long onboarding took, any challenges during rollout]

## The Results
[Quantified outcomes — revenue impact, time saved, efficiency gains, metric improvements]
- Before: [specific metric]
- After: [specific metric]
- "[Customer quote about the result]" — [Name, Title]

## What's Next
[1 sentence on where they're going — shows ongoing relationship]

## In Their Words
[Full pull-quote — most compelling thing the customer said about the value]
```

---

## Mode: --brief (Content Brief for Marketing or Content Team)

When you need to hand off the writing, give a brief that's detailed enough that the output won't need major revisions.

### Content Brief Template

```markdown
# Content Brief: [Asset Name]

**Requested by:** [PM name]
**Due by:** [Date]
**Audience:** [Primary reader — who specifically]
**Goal:** [What we want them to think, feel, or do after reading this]
**Distribution channel:** [Where this will live]
**Format:** [Blog / Email / Landing page / Social / Video script / etc.]
**Length:** [Word count range or visual format]

## The Feature / Product

[2-3 sentences on what it is and for whom]

## Key Message

[One sentence — the single most important thing the reader should take away]

## Supporting Points (in priority order)

1. [Most important supporting fact or benefit]
2. [Second most important]
3. [Third — optional]

## Tone and Style

[Reference context-library/writing-style-*.md if available, or describe]
Example: "Conversational, confident, not hype-y. No buzzwords. Contractions are fine."

## Evidence Available

- Metric: [Specific data point]
- Customer quote: "[Quote]" — [Customer name/type]
- Case study: [Reference if available]

## What NOT to Do

- [Avoid angle or framing]
- [Don't include X because Y]

## References

- [PRD link]
- [Related blog post or existing asset]
- [Competitor for awareness]
```

---

## Output Quality Self-Check

- [ ] **Leads with benefit, not feature:** "Now you can [outcome]" not "We built [feature]"
- [ ] **Channel-appropriate length:** Not a blog post in an email slot
- [ ] **Voice rules applied:** No em dashes, no "leverage/utilize/unlock," contractions used naturally
- [ ] **CTA is specific:** "Start your free export" not "Learn more"
- [ ] **Real customer language borrowed:** User research quotes inform the copy where possible
- [ ] **Measurable outcomes included:** Numbers beat adjectives
- [ ] **Output saved:** `outputs/content/[type]-[feature]-[date].md`
