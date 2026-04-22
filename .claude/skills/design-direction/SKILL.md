---
name: design-direction
description: Set and communicate design direction using a 3-parameter system (variance, motion, density). Style presets (soft, minimal, brutalist). Output briefs for designers or AI tools. Use --strict for engineering-ready specs.
disable-model-invocation: true
user-invocable: true
---

# /design-direction - Set Visual Tone for Features

Help PMs articulate "how it should feel" and communicate that direction to designers, AI prototyping tools, and engineering. Replaces vague feedback like "make it modern" with specific, actionable parameters.

## Quick Start

```
/design-direction                          -> Interactive parameter selection
/design-direction --style soft             -> Calming luxury preset
/design-direction --style minimal          -> Editorial/Notion-inspired preset
/design-direction --style brutalist        -> Swiss typography, data-dense preset
/design-direction --style custom           -> Set each parameter manually
/design-direction --for-designer           -> Brief formatted for human designers
/design-direction --for-v0                 -> Style preamble for v0.dev prompts
/design-direction --for-lovable            -> Style preamble for Lovable prompts
/design-direction --for-bolt              -> Style preamble for Bolt.new prompts
/design-direction --for-prd               -> "Design Direction" section for a PRD
/design-direction --strict                 -> Engineering-ready animation/grid specs (GSAP, tokens, pre-flight)
```

**What to provide:** Feature name or PRD reference, target audience, and any brand constraints.

**What you get:** A design direction document with parameters, mood descriptors, do/don't examples, and tool-specific output.

**Output:** Saved to `outputs/prototypes/[feature]-design-direction.md`

**Time:** 5-10 minutes

---

## Context Routing Logic (Internal)

**Automatic Context Checks:**

| Source | Files/Folders | What to Extract |
|--------|---------------|-----------------|
| Brand Context | `context-library/business-info-template.md` | Brand colors, typography, visual identity, product personality |
| Active PRDs | `outputs/prds/*.md`, `context-library/prds/*.md` | Feature requirements, target users, success metrics |
| User Research | `context-library/research/*.md` | User expectations, audience demographics, accessibility needs |
| Competitor Analysis | `context-library/research/competitive-*.md` | Competitor visual approaches, differentiation opportunities |
| Previous Prototypes | `outputs/prototypes/*.md` | Existing design direction files, established patterns |
| Stakeholder Profiles | `context-library/stakeholder-*.md` | Design reviewers, their preferences and concerns |
| Second Brain | `context-library/second-brain/` | Product knowledge, customer insights relevant to visual direction |

**Context Priority:**
1. Brand guidelines and existing visual identity FIRST (constraints to work within)
2. Target user and audience SECOND (who are we designing for)
3. PRD requirements THIRD (what problem are we solving)
4. Competitor landscape FOURTH (how do we differentiate visually)

---

## When to Use

- **Before prototyping:** Set the tone before generating prompts for v0/Lovable/Bolt
- **Design kickoff:** Give designers a clear brief instead of "make it look good"
- **PRD enrichment:** Add a "Design Direction" section to your PRD
- **Stakeholder alignment:** Get agreement on visual approach before pixel work starts
- **Competitive differentiation:** Deliberately choose a different visual lane from competitors

## When NOT to Use

- You're redesigning an existing UI (use `/design-audit` first to assess what's there)
- You need pixel-level mockups (this sets direction, not specifics)
- The feature is purely backend/API with no user-facing UI

---

## The 3-Parameter System

Every visual direction can be described by three parameters. These give designers and AI tools a shared vocabulary instead of subjective opinions.

### Variance (0.0 - 1.0)
How conventional vs. distinctive should this feel?

| Value | Description | Example |
|-------|-------------|---------|
| 0.1 - 0.2 | Safe and familiar. Standard SaaS patterns. Users know exactly where everything is. | Stripe dashboard, GitHub |
| 0.3 - 0.4 | Mostly conventional with a few distinctive touches. Professional but not boring. | Linear, Notion |
| 0.5 - 0.6 | Noticeably different. Custom layouts, unexpected interactions. Memorable. | Vercel, Raycast |
| 0.7 - 0.8 | Distinctive and bold. Breaks conventions intentionally. Requires some learning. | Arc browser, Nothing Phone UI |
| 0.9 - 1.0 | Experimental. Art-directed. May sacrifice some usability for impact. | Award-winning agency sites |

**Default for most SaaS products:** 0.2 - 0.4. Don't go higher without a reason.

### Motion (0.0 - 1.0)
How animated should interactions feel?

| Value | Description | Example |
|-------|-------------|---------|
| 0.0 - 0.1 | Static. No transitions. Content appears instantly. | Text-heavy admin tools |
| 0.2 - 0.3 | Subtle. Fade-ins, soft hover states. Functional transitions only. | Notion, Linear |
| 0.4 - 0.5 | Moderate. Page transitions, micro-interactions, spring physics on key elements. | Vercel dashboard, Framer |
| 0.6 - 0.7 | Rich. Scroll-triggered animations, parallax, animated data visualizations. | Apple product pages |
| 0.8 - 1.0 | Cinematic. GSAP-level choreography, 3D transforms, immersive scroll experiences. | Award sites, product launches |

**Default for most SaaS products:** 0.2 - 0.3. Motion should serve comprehension, not decoration.

### Density (0.0 - 1.0)
How much information per screen?

| Value | Description | Example |
|-------|-------------|---------|
| 0.1 - 0.2 | Airy. Lots of whitespace. One idea per viewport. Editorial feel. | Apple.com, Stripe landing |
| 0.3 - 0.4 | Balanced. Clear hierarchy with comfortable breathing room. | Notion pages, Linear issues |
| 0.5 - 0.6 | Moderate density. Multiple content blocks visible. Efficient use of space. | GitHub repo view |
| 0.7 - 0.8 | Dense. Dashboard-style. Every pixel earns its place. Data-forward. | Bloomberg terminal lite, Grafana |
| 0.9 - 1.0 | Maximum density. Complex tables, multi-panel layouts, no wasted space. | Trading platforms, IDE layouts |

**Default for most SaaS products:** 0.3 - 0.5. Match to user expertise level.

---

## Style Presets

### --style soft
**Parameters:** Variance 0.4, Motion 0.5, Density 0.2

A calming, luxury aesthetic. Sophistication through restraint.

**Visual signature:**
- **Backgrounds:** Silver-grey or pure white. No harsh contrasts.
- **Typography:** Massive bold Grotesk typefaces for headlines (`text-4xl md:text-6xl tracking-tighter`). Clean geometric sans for body.
- **Spacing:** Airy, floating components. Generous padding (32px+ between sections).
- **Shadows:** Diffused ambient shadows, never hard drop shadows.
- **Color:** One muted accent color max. Desaturated palette.
- **Motion:** Soft spring animations on hover. Gentle fade-ins on scroll. Nothing jarring.

**Best for:** Consumer products, health/wellness, portfolio sites, premium SaaS onboarding.

**Reference products:** Calm, Notion landing page, Aesop, Apple.com

---

### --style minimal
**Parameters:** Variance 0.3, Motion 0.2, Density 0.3

An editorial, content-first aesthetic. Inspired by Linear and Notion.

**Visual signature:**
- **Backgrounds:** Warm off-white (#FAFAF8 or similar). Never pure white (#FFF).
- **Typography:** Extreme contrast between headline and body. Geometric sans-serif for body. Editorial serif for headings if appropriate. Monospace for metadata and labels.
- **Borders:** Consistently `1px solid #EAEAEA`. Never thicker.
- **Color:** Warm monochromatic foundation. Desaturated pastels for semantic accents only (status, categories).
- **Motion:** Fade-ins and ultra-soft hover states only. Never layout-shifting animations.
- **Components:** No gradients, no heavy shadows, no pill-shaped containers, no generic icons.

**Best for:** B2B tools, documentation, internal dashboards, content platforms.

**Reference products:** Linear, Notion, Stripe docs, Vercel dashboard

---

### --style brutalist
**Parameters:** Variance 0.7, Motion 0.1, Density 0.8

Mechanical precision. Swiss typography meets data density.

**Two visual modes:**

**Swiss Industrial Print (light):**
- Light substrates, heavy sans-serif headlines, aggressive negative space, primary red as sole accent

**Tactical Telemetry (dark):**
- Dark backgrounds, monospaced fonts, ASCII-style brackets, terminal aesthetic

**Visual signature:**
- **Typography dominates.** Extreme scale variance. Neo-grotesque heavyweights (Neue Haas Grotesk, Inter Black weight).
- **No border-radius.** 90-degree angles only. Sharp corners everywhere.
- **Rigid grid.** CSS Grid with visible compartmentalization. No overlapping.
- **Color:** Carbon on matte (light mode) or phosphor on dark (dark mode). Red is the only accent.
- **Analog effects:** Halftone patterns, scanlines, noise textures as decorative elements.
- **Components:** ASCII framing characters, registration marks, crosshair decorations.

**Best for:** Developer tools, monitoring dashboards, data platforms, technical documentation.

**Reference products:** Bloomberg Terminal, Vercel system status, brutalist web design movement

---

## Universal Anti-Patterns (Apply to All Styles)

These are banned regardless of which style preset you choose. They are hallmarks of generic AI-generated design.

**Never include:**
- AI purple/violet gradient aesthetic (the "ChatGPT look")
- Neon glows or glassmorphism without purpose
- Emoji as functional icons (use professional SVG icons instead)
- Pure black (#000000) backgrounds (use #0e1011 or #18181B for dark themes)
- Generic stock avatars or "Jane Doe" placeholder names
- Centered hero sections when Variance > 0.4 (use asymmetric/split-screen instead)
- Inter font as default (choose something with more character or use system fonts)
- Cookie-cutter icon grids (the 3-column feature grid with circular icons)
- Generic gradient buttons on white cards
- "Lorem ipsum" or any placeholder text (use realistic content)
- Broken or missing image fallbacks

**Always include:**
- Loading, error, and empty states for every view
- Hardware-accelerated animations only (transform, opacity). Never animate layout properties (width, height, top, left).
- Full-viewport safety (`min-h-[100dvh]` not `min-h-screen`)
- Mobile responsiveness as a constraint, not an afterthought
- Realistic sample data that makes the prototype feel production-ready

---

## Workflow

### Step 1: Gather Context

Ask the PM:
1. **What feature?** (Name, PRD reference, or quick description)
2. **Who's the user?** (Technical? Non-technical? Power user? First-time?)
3. **What's the emotional goal?** (Trust? Excitement? Efficiency? Calm?)
4. **Any brand constraints?** (Existing design system? Brand colors? Typography?)
5. **Who reviews this?** (Designer? Stakeholder? Engineering?)

**Silently check:** Brand context, existing PRDs, user research, competitor analysis.

### Step 2: Set Parameters

Based on context, recommend parameter values with rationale:

```
Based on [feature context] and [user research], here's my recommendation:

Variance: 0.3 -- We're building a B2B settings page. Users expect familiar
  patterns. Being distinctive here costs more than it gains.
Motion: 0.2 -- Functional transitions only. This is a utility screen, not
  a marketing moment.
Density: 0.5 -- Moderate. Settings screens need to show many options without
  overwhelming, so balanced density with clear grouping.

This puts us in "Linear territory" -- professional, efficient, quietly polished.

Want to adjust any parameter, or should I apply a preset instead?
```

### Step 3: Generate Design Direction Document

Build the full document with:
- Parameters and rationale
- Visual signature (colors, typography, spacing, motion, components)
- Do/don't examples (specific to this feature)
- Reference products
- Anti-patterns to avoid
- Output formatted for the requested audience (designer, AI tool, PRD)

### Step 4: Format for Target Audience

**--for-designer:** Full brief with mood descriptors, references, parameter explanations, do/don't with visual examples.

**--for-v0 / --for-lovable / --for-bolt:** Condensed style block to prepend to a prototype prompt:

```
## Visual Style (prepend to your prototype prompt)

Style: [Preset name or "Custom"]
Feel: [2-3 mood words]
Colors: [Specific palette with hex values]
Typography: [Font choices with size scale]
Spacing: [Grid system and padding guidelines]
Motion: [What animates and how]
Banned: [Specific things to avoid]
Reference: [1-2 products to emulate]
```

**--for-prd:** A self-contained "Design Direction" section:

```
## Design Direction

**Parameters:** Variance [X] / Motion [Y] / Density [Z]
**Preset:** [Name] or Custom
**Emotional goal:** [What the user should feel]
**Visual references:** [Products that capture the target feel]
**Key constraints:** [Brand rules, accessibility requirements, platform targets]
**Anti-patterns:** [What this should NOT look like]
```

---

## Mode: --strict (Engineering-Ready Specs)

When the PM needs to hand off precise visual specifications to engineering, `--strict` outputs deterministic design tokens and animation definitions derived from the gpt-tasteskill framework.

**What --strict adds:**

1. **Design Tokens (CSS Custom Properties)**
```css
:root {
  --color-surface: #0e1011;
  --color-text-primary: #F5F5F5;
  --color-accent: #3B82F6;
  --radius-default: 8px;
  --spacing-unit: 4px;
  --font-heading: 'Neue Haas Grotesk', system-ui;
  --font-body: 'Inter', system-ui;
  --transition-default: 200ms cubic-bezier(0.4, 0, 0.2, 1);
}
```

2. **GSAP Animation Definitions** (when Motion > 0.5)
```javascript
// Scroll-triggered entrance
gsap.from(element, {
  y: 40, opacity: 0, duration: 0.8,
  ease: "power3.out",
  scrollTrigger: { trigger: element, start: "top 85%" }
});

// Spring physics for interactive elements
gsap.to(element, {
  scale: 1.02, duration: 0.3,
  ease: "elastic.out(1, 0.5)"
});
```

3. **Grid Architecture Rules**
- Use `grid-auto-flow: dense` for bento layouts
- Mathematical tile verification: all tiles must fill grid without gaps
- Asymmetric tile sizing when Variance > 0.4

4. **Pre-Flight Verification Checklist**
- [ ] No emoji used as functional icons
- [ ] No centered hero layout (if Variance > 0.4)
- [ ] All animations use transform/opacity only
- [ ] Color palette limited to max one accent
- [ ] Typography uses max 2 font families
- [ ] All interactive states defined (hover, focus, active, disabled)
- [ ] Dark theme uses #0e1011 or similar, not pure #000
- [ ] Loading/error/empty states designed for every view

---

## Output Quality Self-Check

Before delivering the design direction, verify:

- [ ] **Parameters have rationale** -- Each value is justified by user context, not arbitrary
- [ ] **Brand context respected** -- Direction doesn't conflict with existing brand guidelines
- [ ] **Anti-patterns listed** -- Specific things to avoid, not just what to do
- [ ] **References are real** -- Cited products actually match the described aesthetic
- [ ] **Audience-appropriate format** -- Designer gets a brief, AI tool gets a style block, PRD gets a section
- [ ] **Actionable specifics** -- Hex colors, font names, spacing values. Not just "modern and clean"
- [ ] **Motion justified** -- Every animation has a purpose (guides attention, shows state change, provides feedback)
- [ ] **Accessibility not sacrificed** -- Contrast ratios, reduced motion support, and touch targets still meet standards
- [ ] **Realistic for the context** -- A settings page shouldn't get Variance 0.9 just because it sounds cooler

---

## Integration with Other Skills

**Before:**
- `/prd-draft` -- Define the feature first, then set its visual tone
- `/competitor-analysis` -- Understand the visual landscape before differentiating
- `/design-audit` -- Audit the current state before setting a new direction

**After:**
- `/generate-ai-prototype --with-taste` -- Inject this direction into AI tool prompts
- `/prototype` -- Build prototypes that follow this direction
- `/prototype-feedback --design` -- Evaluate prototypes against this direction

**Related:**
- `/napkin-sketch` -- Quick layout exploration before committing to a direction
- `/prd-review-panel` -- Designer sub-agent validates the direction fits the product

---

## Related Skills

- `/design-audit` -- Audit existing UIs before setting a new direction
- `/generate-ai-prototype` -- Generate tool-specific prompts with taste parameters
- `/prototype` -- Build prototypes in the chosen style
- `/prototype-feedback` -- Evaluate results against the direction
