---
name: design-audit
description: Structured 7-dimension design audit for existing UIs. Evaluate typography, color, layout, interactivity, content, components, and code quality. Modes for competitor audit, benchmarking, and quick review.
disable-model-invocation: true
user-invocable: true
---

# /design-audit - Structured UI Quality Assessment

Give structured, credible design feedback on existing products, competitor UIs, or shipped prototypes. Uses a 7-dimension framework so your feedback sounds like it came from a design lead, not a PM saying "I don't like it."

## Quick Start

```
/design-audit                              -> Audit a URL, screenshot, or description
/design-audit --competitor [name]          -> Audit a competitor's UI with strategic lens
/design-audit --benchmark [ours vs theirs] -> Side-by-side comparison
/design-audit --quick                      -> Top 3 issues only (for fast reviews)
```

**What to provide:** A screenshot, URL, screen recording, or detailed description of the UI to audit.

**What you get:** A scored, prioritized audit across 7 dimensions with specific issues and actionable recommendations.

**Output:** Saved to `outputs/prototypes/[target]-design-audit-[date].md`

**Time:** 15-30 minutes (full audit), 5 minutes (--quick)

---

## Context Routing Logic (Internal)

**Automatic Context Checks:**

| Source | Files/Folders | What to Extract |
|--------|---------------|-----------------|
| Brand Context | `context-library/business-info-template.md` | Brand guidelines, visual identity, product personality |
| Design Direction | `outputs/prototypes/*-design-direction.md` | Target parameters if direction was already set |
| Active PRDs | `outputs/prds/*.md`, `context-library/prds/*.md` | Design requirements, acceptance criteria |
| User Research | `context-library/research/*.md` | User expectations, accessibility requirements, pain points |
| Competitor Analysis | `context-library/research/competitive-*.md` | Competitor visual benchmarks |
| Stakeholder Profiles | `context-library/stakeholder-*.md` | Who cares about design quality and their standards |
| Second Brain | `context-library/second-brain/` | Customer insights, competitive intelligence |

**Context Priority:**
1. The UI being audited FIRST (screenshot, URL, or description)
2. Brand guidelines and design direction SECOND (what it should match)
3. User context and research THIRD (who uses this and what they expect)
4. Competitor benchmarks FOURTH (how others solve this)

---

## When to Use

- **Design review meetings:** Give structured feedback instead of opinions
- **Competitor analysis:** Audit competitor UIs to find weaknesses and opportunities
- **Pre-redesign:** Assess the current state before proposing changes
- **Post-launch check:** Evaluate whether the shipped product matches the design intent
- **Stakeholder presentations:** Back up "we need to redesign this" with scored evidence
- **Vendor evaluation:** Compare SaaS tools or design agencies

## When NOT to Use

- Reviewing an in-progress prototype (use `/prototype-feedback --design` instead)
- Setting a new visual direction (use `/design-direction` first)
- Wireframe or napkin sketch review (too early for visual audit)
- Code review (use `/code-review` for code quality)

---

## The 7-Dimension Audit Framework

Each dimension is scored 1-5 with specific issues documented.

| Score | Meaning |
|-------|---------|
| 1 | Critically broken. Actively harms usability or brand perception. |
| 2 | Below standard. Multiple clear issues that need fixing. |
| 3 | Acceptable. Works but has obvious room for improvement. |
| 4 | Good. Polished with minor refinements possible. |
| 5 | Excellent. Best-in-class execution of this dimension. |

---

### Dimension 1: Typography

**What to evaluate:**
- **Hierarchy clarity:** Can you instantly tell what's most important? Is there a clear primary, secondary, tertiary reading order?
- **Readability:** Line length (45-75 characters for body), line height (1.4-1.6 for body), font size (16px minimum for body on desktop)
- **Font selection:** Does the typeface match the product personality? Is it legible at all sizes?
- **Consistency:** How many distinct type styles? (Max 5-6 across the entire UI.) Are they applied consistently?
- **Brand alignment:** Does typography match the brand's visual identity?

**Common issues to flag:**
- Too many font sizes/weights creating visual noise
- Headlines that don't stand out from body text
- Body text too small or too light (especially light grey on white)
- Inconsistent capitalization (Title Case mixed with sentence case)
- Weak fonts that lack character (overuse of system defaults without intention)

---

### Dimension 2: Color and Surfaces

**What to evaluate:**
- **Palette coherence:** Are colors intentional and limited? (Max 3-4 primary colors)
- **Contrast and accessibility:** WCAG AA compliance (4.5:1 for normal text, 3:1 for large text)
- **Emotional tone:** Does the color palette match the product's personality and audience?
- **Semantic meaning:** Do colors communicate state? (Red = error, green = success, etc.)
- **Surface depth:** Are backgrounds, cards, and layers visually differentiated?

**Common issues to flag:**
- Oversaturated accents that compete for attention
- Flat surfaces with no depth (everything on the same visual plane)
- Color used purely for decoration, not meaning
- Insufficient contrast on text or interactive elements
- Too many accent colors creating a carnival effect
- Pure black backgrounds (#000) instead of near-black (#18181B)

---

### Dimension 3: Layout

**What to evaluate:**
- **Scanning patterns:** Does the layout support natural eye movement (F-pattern for text, Z-pattern for landing pages)?
- **Information density:** Is the density appropriate for the audience? (Power users tolerate more density than casual users.)
- **Grid consistency:** Are elements aligned to a visible or implied grid system?
- **Responsive behavior:** How does the layout adapt across breakpoints?
- **Whitespace:** Is there breathing room? Or is content cramped?

**Common issues to flag:**
- Cookie-cutter grid layouts (3-column icon grid, 4-column feature cards) that feel generic
- No clear visual flow. The eye doesn't know where to go.
- Elements that float outside the grid alignment
- Desktop-only thinking (tables, hover interactions, wide layouts)
- Wasted space vs. productive whitespace

---

### Dimension 4: Interactivity

**What to evaluate:**
- **Feedback loops:** Does every action produce visible feedback? (Click, submit, toggle, delete)
- **State management:** Are all interactive states designed? (Default, hover, active, focus, disabled, loading)
- **Animation purpose:** Does motion guide attention, show relationships, or confirm actions? Or is it decorative noise?
- **Error handling:** What happens when things go wrong? Is recovery clear?
- **Loading behavior:** How does the UI handle slow operations? (Skeleton screens, progress bars, spinners)

**Common issues to flag:**
- Buttons with no hover/focus states
- No loading indicators for async operations
- Error messages that don't explain how to fix the problem
- Animations that shift layout (janky, non-hardware-accelerated)
- Missing empty states ("what does this look like with zero data?")
- No confirmation for destructive actions (delete without undo)

---

### Dimension 5: Content

**What to evaluate:**
- **Copy quality:** Is the writing clear, concise, and action-oriented?
- **Microcopy:** Are labels, tooltips, placeholders, and error messages helpful?
- **Empty states:** What does the user see before there's data? Is it helpful or just blank?
- **Error messages:** Do they explain what went wrong AND how to fix it?
- **Tone consistency:** Does the voice match across all touchpoints?

**Common issues to flag:**
- Placeholder text or Lorem ipsum in production
- AI cliches ("Unlock your potential," "Harness the power of")
- Generic error messages ("Something went wrong")
- Missing empty states (blank screens with no guidance)
- Inconsistent terminology (using "project" and "workspace" interchangeably)
- CTAs that don't describe the action ("Submit" instead of "Create account")

---

### Dimension 6: Components

**What to evaluate:**
- **Consistency:** Are similar components styled the same way throughout? (All buttons look like buttons, all cards look like cards)
- **Design system adherence:** Does the UI follow an established pattern library?
- **Reusability:** Are components modular, or are there one-off custom elements everywhere?
- **Appropriate patterns:** Are the right components used for the right purpose? (Dropdown vs. radio buttons, modal vs. inline expansion)
- **State coverage:** Do components handle all necessary states?

**Common issues to flag:**
- Multiple button styles for the same action level
- Custom components where standard patterns would work
- Overuse of modals (when inline expansion or a new page would be better)
- Testimonial carousels, generic avatar grids, and other cliched patterns
- Components that look different in different parts of the app
- Missing states (what does this table look like with 0 rows? 1000 rows?)

---

### Dimension 7: Code Quality (Visual Impact)

**What to evaluate:**
- **Performance:** Does the page feel fast? Are images optimized? Do animations jank?
- **Semantic HTML:** Are headings, landmarks, and ARIA labels correct?
- **Accessibility markup:** Can a screen reader user navigate this?
- **CSS quality:** Are styles consistent, or is there visual drift from inline overrides?
- **Responsive implementation:** Do breakpoints work correctly, or do things break?

**Common issues to flag:**
- Large unoptimized images causing slow loads
- Animations that cause layout shifts or dropped frames
- Missing semantic HTML (divs instead of buttons, no heading hierarchy)
- No focus indicators on interactive elements
- Broken layout at certain viewport widths
- Font loading causing visible text shift (FOUT/FOIT)

---

## Output Template

```markdown
# Design Audit: [Target Name]
**Date:** [Date]
**Audited by:** PM + Claude (7-dimension framework)
**Target:** [URL, screenshot, or description]
**Context:** [What this is and why we're auditing it]

## Overall Score: [X.X / 5.0]

| Dimension | Score | Key Issue |
|-----------|-------|-----------|
| Typography | X/5 | [One-line summary] |
| Color & Surfaces | X/5 | [One-line summary] |
| Layout | X/5 | [One-line summary] |
| Interactivity | X/5 | [One-line summary] |
| Content | X/5 | [One-line summary] |
| Components | X/5 | [One-line summary] |
| Code Quality | X/5 | [One-line summary] |

## Strengths
[2-3 things this UI does well. Be specific.]

## Issues by Priority

### Must-Fix (blocks launch or damages brand)
1. **[Issue]** (Dimension: [X]) -- [What's wrong] -> [How to fix it]
2. ...

### Should-Fix (noticeable quality gap)
1. **[Issue]** (Dimension: [X]) -- [What's wrong] -> [How to fix it]
2. ...

### Nice-to-Have (polish and refinement)
1. **[Issue]** (Dimension: [X]) -- [What's wrong] -> [How to fix it]
2. ...

## Accessibility Flags
[Any WCAG violations or accessibility concerns, listed separately for visibility]

## Recommendations
[2-3 high-level recommendations for improving the overall design quality]

## Next Steps
- [ ] Share audit with design team
- [ ] Prioritize must-fix items in next sprint
- [ ] Run `/design-direction` to set target direction for improvements
- [ ] Re-audit after changes with `/design-audit`
```

---

## Mode: --competitor [name]

Audits a competitor's UI with a strategic lens. Same 7-dimension framework, but adds:

**Additional sections:**

```markdown
## Strategic Implications

### Where they're stronger than us
[Specific dimensions where the competitor scores higher]

### Where we're stronger
[Specific dimensions where our product scores higher]

### Differentiation opportunities
[Visual or UX approaches they're NOT taking that we could own]

### What to steal (ethically)
[Specific patterns or approaches worth adopting]
```

**Cross-reference with:** `context-library/research/competitive-*.md` and `/competitor-analysis` output.

---

## Mode: --benchmark [ours vs theirs]

Side-by-side comparison of two UIs (yours and a competitor, or two versions of your product).

**Output format:**

```markdown
## Benchmark: [Our Product] vs [Their Product]

| Dimension | Ours (Score) | Theirs (Score) | Winner | Notes |
|-----------|-------------|----------------|--------|-------|
| Typography | X/5 | X/5 | [Name] | [Why] |
| Color & Surfaces | X/5 | X/5 | [Name] | [Why] |
| Layout | X/5 | X/5 | [Name] | [Why] |
| Interactivity | X/5 | X/5 | [Name] | [Why] |
| Content | X/5 | X/5 | [Name] | [Why] |
| Components | X/5 | X/5 | [Name] | [Why] |
| Code Quality | X/5 | X/5 | [Name] | [Why] |
| **Overall** | **X.X** | **X.X** | | |

## Key Takeaways
[3-5 actionable insights from the comparison]
```

---

## Mode: --quick

Top 3 issues only. For fast design review meetings or Slack feedback.

**Output:**

```markdown
## Quick Design Review: [Target]

**Overall impression:** [One sentence]

**Top 3 issues:**
1. **[Issue]** -- [What and why] -> [Fix]
2. **[Issue]** -- [What and why] -> [Fix]
3. **[Issue]** -- [What and why] -> [Fix]

**Strongest element:** [One thing that works well]
```

---

## Anti-Patterns to Avoid

- **Opinion masquerading as analysis.** "I don't like the colors" is not a design audit. Score each dimension, cite specific issues, and recommend fixes.
- **Prescribing pixel-level solutions.** The audit identifies problems and direction. The designer decides the exact implementation.
- **Ignoring context.** A developer tool dashboard has different quality standards than a consumer onboarding flow. Score relative to the audience.
- **Auditing wireframes.** This framework is for visual/shipped UIs. Wireframes are too early for this level of critique.
- **Skipping accessibility.** Every audit must include accessibility flags. It's not optional polish.
- **Comparing apples to oranges.** Don't benchmark a startup's MVP against Apple.com. Compare within reasonable competitive sets.

---

## Output Quality Self-Check

Before delivering the audit, verify:

- [ ] **All 7 dimensions scored** -- No dimension skipped, even if it scores well
- [ ] **At least 3 specific actionable items** -- Not just scores, but concrete issues with recommended fixes
- [ ] **Issues tied to user impact** -- "This hurts conversion" or "Users will miss this" not just "This looks off"
- [ ] **Accessibility flagged separately** -- WCAG violations are visible, not buried in other dimensions
- [ ] **Context respected** -- Scores account for the product's audience and category
- [ ] **Strengths acknowledged** -- The audit isn't only criticism. What's working well?
- [ ] **Recommendations are prioritized** -- Must-fix, should-fix, nice-to-have. Not a flat list.
- [ ] **Brand context checked** -- Audit references existing brand guidelines if available
- [ ] **Next steps are clear** -- PM knows what to do with this audit

---

## Integration with Other Skills

**Before:**
- `/competitor-analysis` -- Gather competitive context before auditing
- `/user-research-synthesis` -- Understand user expectations before judging the UI

**After:**
- `/design-direction` -- Set target parameters for the redesign
- `/prototype` -- Build improved version based on audit findings
- `/prototype-feedback --design` -- Evaluate the new prototype against audit issues
- `/create-tickets` -- Turn must-fix items into engineering tickets

**Related:**
- `/prd-review-panel` -- Designer sub-agent reviews PRDs, this skill reviews built UIs
- `/napkin-sketch` -- Quick wireframe after identifying layout issues
- `/second-brain ingest` -- File audit findings into competitive-intelligence

---

## Related Skills

- `/design-direction` -- Set the target aesthetic after auditing current state
- `/prototype-feedback --design` -- For in-progress prototypes, not shipped UIs
- `/competitor-analysis` -- Broader competitive intelligence beyond UI
- `/prototype` -- Build the improved version
