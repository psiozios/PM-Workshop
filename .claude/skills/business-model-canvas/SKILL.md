---
name: business-model-canvas
description: Build and analyze a Business Model Canvas or Lean Canvas. Two modes - generate from scratch with AI assistance, or critique an existing canvas for gaps and weak assumptions.
disable-model-invocation: false
user-invocable: true
---

# /business-model-canvas - Build or Critique Your Canvas

Map how your business or product creates, delivers, and captures value. Two modes: **Generate** (build a canvas from your context) or **Critique** (review an existing canvas for gaps).

## Quick Start

```
/business-model-canvas

Two modes available:

Generate: I'll build a Business Model Canvas or Lean Canvas from your product description and context files.
Critique: Paste your existing canvas and I'll flag weak blocks, missing assumptions, and strategic gaps.

Tell me:
1. Mode: Generate or Critique?
2. Canvas type: Business Model Canvas (established product) or Lean Canvas (new product / startup)?
3. If Generating: What's the product or initiative you're mapping?

Output: outputs/business-canvases/[bmc|lean]-canvas-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

**Automatic Context Checks:**
When this skill is invoked, immediately check:

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Business Info | `context-library/business-info-template.md` | revenue model, pricing, customer segments, partnerships | Current business model context |
| Strategy | `context-library/strategy/*.md` | value proposition, positioning, channels, GTM | Strategic context for canvas blocks |
| Research | `context-library/research/*.md` | customer segments, pain points, jobs-to-be-done | Customer segment and problem definition |
| PRDs | `context-library/prds/*.md` | feature description, solution approach | Solution and key activities |
| Decisions | `context-library/decisions/*.md` | pricing decisions, partnership decisions | Revenue and channel decisions |

**Context Priority:**
1. Business info template FIRST (pricing, revenue model, segments)
2. Strategy docs SECOND (positioning, channels, GTM)
3. User research THIRD (customer segments and pain validation)
4. PRDs FOURTH (solution specifics)

**Cross-Skill Links:**
- After canvas → Link to `/write-prod-strategy` to build full strategy
- If pricing block is weak → Link to `/survey-builder` (Van Westendorp) to research pricing
- If customer segments are unclear → Link to `/interview-guide` to run discovery interviews
- If building business case → Reference canvas in `/prd-draft` Strategic Fit section
- If doing 1-day sprint → Canvas output pairs with `/strategy-sprint`

---

## Step 0: Understanding Your Context

Before building the canvas, let me check what already exists...

**Checking:**
- `context-library/business-info-template.md` for current business model
- `context-library/strategy/` for value proposition and positioning
- `context-library/research/` for customer segment and pain data

**Based on what I find, I'll show you:**

### What We Already Know

**Business Model Context:**
- [Revenue model from business-info template]
- [Customer segments from strategy or research]
- [Key channels from GTM or strategy docs]

**Gaps to Fill:**
- [Blocks with no existing context - will need PM input]

### PM-Specific Diagnosis Questions

1. **Stage:** Is this an established product or a new initiative/startup?
2. **Audience:** Is this canvas for internal alignment, investor presentation, or strategic planning?
3. **Scope:** Are we mapping the whole business or a specific product line?
4. **Depth:** Do you want a quick draft or a detailed, validated canvas?

---

## Canvas Type Selector

| Canvas Type | Best For | Key Difference |
|-------------|----------|----------------|
| Business Model Canvas | Established products, businesses with known revenue | 9 blocks, covers operations and customer-facing model |
| Lean Canvas | New products, startups, early-stage initiatives | 9 blocks optimized for uncertainty and rapid iteration; includes Problem and Unfair Advantage instead of Key Partners and Customer Relationships |

---

## Business Model Canvas (BMC)

*Best for: Established products and businesses with a working model*

**9 Blocks:**

### 1. Key Partners
Who do you rely on to operate successfully?
- Suppliers of key resources you don't own
- Manufacturers or distributors in your delivery chain
- Ecosystem partners (integrations, resellers, agencies)
- Strategic alliances for scale or risk sharing

*Guiding questions:* Who could you not operate without? Who provides inputs you can't produce yourself? What partnerships reduce your cost structure or increase your reach?

### 2. Key Activities
What must you do exceptionally well to deliver your value proposition?
- Product development and engineering
- Customer acquisition and marketing
- Customer success and support
- Platform maintenance and operations

*Guiding questions:* What does your business do every day? What activities create the most value for customers? What activities are impossible to outsource?

### 3. Key Resources
What assets do you need to operate?
- Physical: servers, equipment, facilities
- Intellectual: IP, patents, proprietary data, brand
- Human: specialized talent, expertise, culture
- Financial: capital, credit lines, cash flow

*Guiding questions:* What do your key activities require? What resources do competitors lack? What's your most defensible resource?

### 4. Value Propositions
What value do you deliver to your customer? What problem do you solve?
- Pain relievers (eliminate specific frustrations)
- Gain creators (create outcomes customers want)
- Table stakes (must-haves to compete in the category)

*Guiding questions:* Why do customers choose you? What would they lose if you disappeared? How are you meaningfully different from alternatives?

### 5. Customer Relationships
What relationship do you establish with each segment?
- Personal assistance (dedicated support, account management)
- Self-service (tools, FAQs, knowledge base)
- Automated (AI, algorithms, recommendations)
- Communities (user forums, peer networks)
- Co-creation (users help build the product)

*Guiding questions:* What does your customer expect? How does this fit your cost structure? Which relationship type creates the most loyalty?

### 6. Channels
How do you reach customers and deliver value?
- Direct (your app, website, sales team)
- Indirect (marketplaces, resellers, partners)
- Digital (social media, SEO, email, ads)
- Physical (events, retail, outbound)

*Guiding questions:* Which channels do your customers prefer? Which are most cost-effective? Which give you the most direct relationship?

### 7. Customer Segments
Who are you creating value for?
- Mass market (one offering for a broad group)
- Niche market (specialized segment with specific needs)
- Segmented (multiple related segments with varying needs)
- Multi-sided platform (two+ interdependent groups, like buyers and sellers)

*Guiding questions:* Who is your best customer today? Who do you want to serve? Are some segments more profitable than others?

### 8. Cost Structure
What are the most significant costs in your business model?
- Fixed costs (salaries, infrastructure, rent)
- Variable costs (cost of goods, usage-based hosting)
- Economies of scale (costs that decrease as volume grows)
- Economies of scope (cost advantages from multiple products)

*Guiding questions:* What are your top 5 cost drivers? What's your cost per customer? What does each key activity cost?

### 9. Revenue Streams
How do you generate revenue from each customer segment?
- Subscriptions (recurring monthly/annual fees)
- Usage-based (pay per use, per seat, per transaction)
- Licensing (IP, software, content)
- One-time sales (product purchases)
- Advertising (audience monetization)
- Freemium (free tier + paid upgrade)

*Guiding questions:* What are customers paying for today? What would they prefer to pay? Are there untapped revenue streams?

---

## Lean Canvas

*Best for: New products, startups, early-stage initiatives where the model is still being validated*

**9 Blocks:**

### 1. Problem
What are the top 3 problems you're solving?

For each problem, also capture:
- **Existing alternatives:** How do customers solve this today? (This reveals your real competition)

*Guiding questions:* Have you talked to customers about this? Is this a problem they urgently want solved or a nice-to-have?

### 2. Solution
What are the top 3 features or capabilities that address the problem?

Keep this minimal. The Lean Canvas is not a feature list — it's a hypothesis about the smallest thing that could work.

*Guiding questions:* Can you describe this in one sentence? Would a customer understand the value instantly?

### 3. Key Metrics
What are the 1-3 numbers that tell you if your business is working?

Use the AARRR framework as a guide: Acquisition → Activation → Retention → Revenue → Referral. Pick one number per stage that matters most right now.

*Guiding questions:* What metric goes up when customers are getting value? What's your North Star?

### 4. Unique Value Proposition
What's the single, clear, compelling message that explains why you're different and worth attention?

A strong UVP has three parts:
- Who it's for
- What it does
- Why it's better than the alternative

*Guiding questions:* Could a first-time visitor understand this in 5 seconds? Is it specific enough to be believed?

### 5. Unfair Advantage
What do you have that can't easily be copied or bought?

True unfair advantages are rare. Be honest. Common answers that aren't real advantages: "first mover," "better technology," "great team." Real advantages: proprietary data, exclusive partnerships, network effects, regulatory moats, domain expertise that takes years to build.

*Guiding questions:* If a well-funded competitor tried to copy you tomorrow, what couldn't they replicate?

### 6. Channels
How will you reach your customer segments?

For early-stage: focus on channels you can test cheaply (direct outreach, community, content) before investing in expensive channels (paid ads, sales team).

### 7. Customer Segments
Who are your early adopters specifically?

Not "everyone who needs X" — that's a mass market. Find the specific people who feel the problem most acutely and are already looking for a solution.

*Guiding questions:* Who wakes up thinking about this problem? Who would be devastated if your solution disappeared tomorrow?

### 8. Cost Structure
What are your main costs right now?

For early-stage, focus on: (a) costs to build the MVP, (b) costs to acquire your first 100 customers, (c) ongoing costs to serve them.

### 9. Revenue Streams
How will you make money?

Even at the earliest stage, have a pricing hypothesis. It doesn't have to be right — but building something free by default is a strategic choice that's hard to undo.

---

## Critique Mode

When you paste an existing canvas for review, I'll evaluate each block against these criteria:

| Block | What Good Looks Like | Common Failure Modes |
|-------|---------------------|---------------------|
| Value Proposition | Specific, differentiated, verifiable | Generic ("best product," "easy to use"), not tied to real customer pain |
| Customer Segments | Specific enough to find and talk to | "SMBs," "enterprises," "anyone who needs X" — too broad to act on |
| Problem (Lean) | Validated with customer conversations | Assumed, not validated; no existing alternatives listed |
| Revenue Streams | Specific model with pricing hypothesis | "TBD," "depends on customer," "subscription or licensing" |
| Unfair Advantage | Something that's genuinely hard to replicate | "Our team," "first mover," "passion" |
| Key Metrics | 1-3 leading indicators tied to the business model | Long lists of vanity metrics; no North Star |
| Channels | Specific, testable | "Social media," "partnerships," "word of mouth" without specifics |

**Critique output format:**
- 🟢 Strong block: Evidence it's well-defined
- 🟡 Needs work: What's missing and how to improve
- 🔴 Weak block: Significant gap or risky assumption — validate before proceeding

---

## Output Template

```markdown
# [Business Model Canvas / Lean Canvas]: [Product/Initiative Name]

**Date:** [date]
**Canvas type:** [BMC / Lean Canvas]
**Version:** [v1 / iteration number]
**Status:** [Draft / Validated / Final]

---

## Canvas

### [Block 1 Name]
[Content]

### [Block 2 Name]
[Content]

[...continue for all 9 blocks...]

---

## Key Assumptions

List the 3-5 most critical assumptions in this canvas that, if wrong, would invalidate the model:

1. **[Assumption 1]** - Confidence: [High/Med/Low] - How to validate: [action]
2. **[Assumption 2]** - Confidence: [High/Med/Low] - How to validate: [action]
3. **[Assumption 3]** - Confidence: [High/Med/Low] - How to validate: [action]

---

## Next Steps

- [ ] [Action to validate most critical assumption]
- [ ] [Action to fill weakest block]
- [ ] [Share with stakeholders for input on [specific blocks]]
```

---

## Output Integration

### Where Files Go

**Canvases:**
- Active work: `outputs/business-canvases/[bmc|lean]-canvas-[date].md`
- When finalized: Move to `context-library/strategy/` for ongoing reference

### Cross-Skill Integration

**Feeds into:**
- `/write-prod-strategy` - Canvas informs the business model section of strategy docs
- `/strategy-sprint` - 1-day or 1-week sprint benefits from canvas as a foundation
- `/prd-draft` - Canvas context populates "Strategic Fit" and "Business Model" sections
- `/impact-sizing` - Revenue streams and cost structure inform sizing assumptions

**Pulls from:**
- `context-library/business-info-template.md` - Auto-populates known blocks
- `context-library/research/` - Customer segment and pain data
- `context-library/strategy/` - Value proposition and positioning context

---

## Output Quality Self-Check

Before delivering a canvas, verify:

- [ ] **Canvas type matches the stage:** BMC for established products, Lean Canvas for new/early-stage
- [ ] **Context was checked first:** Pulled from business-info, strategy, and research files before asking for input
- [ ] **All 9 blocks are filled:** No empty or "TBD" blocks without explanation
- [ ] **Value proposition is specific:** Not generic — it names who, what, and why better than alternatives
- [ ] **Customer segment is specific:** Narrow enough to find and interview, not "SMBs" or "everyone"
- [ ] **Key assumptions identified:** Top 3-5 assumptions explicitly called out with validation actions
- [ ] **Revenue is specific:** A real pricing model, not "TBD" or "freemium/enterprise depending on the customer"
- [ ] **Critique mode (if used): Every block rated:** 🟢/🟡/🔴 with specific reasoning, not just "this is weak"
- [ ] **Output file saved:** Canvas saved to `outputs/business-canvases/[bmc|lean]-canvas-[date].md`
