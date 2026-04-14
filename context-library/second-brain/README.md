# Second Brain

This folder is the persistent, compounding knowledge layer of the PM OS. It's a collection of **focus-area wikis** maintained by the LLM and owned by you.

Inspired by Andrej Karpathy's LLM Wiki pattern, adapted for PM work by Aakash Gupta, integrated into this workspace by the `/second-brain` skill.

## Structure

```
context-library/second-brain/
├── README.md                          # This file
└── {focus-slug}/                      # One folder per focus area
    ├── CLAUDE.md                      # Per-focus schema and rules
    ├── raw/                           # Immutable source documents
    │   └── assets/                    # Images, screenshots, diagrams
    ├── wiki/                          # LLM-maintained compiled knowledge
    │   ├── index.md                   # Catalog of every page
    │   ├── log.md                     # Append-only activity log
    │   └── *.md                       # One page per concept
    └── outputs/                       # Briefings, lint reports, explore reports
```

## Focus Areas

Focus areas are created on demand via `/second-brain init <slug>`. The PM OS has six PM-tuned defaults:

| Focus area | What goes in it |
|---|---|
| `product-knowledge` | Your product architecture, key metrics, activation/retention facts, internal docs |
| `competitive-intelligence` | Competitors, positioning, pricing, messaging, recent moves |
| `customer-insights` | Jobs-to-be-done, pain points, segments, verbatim quotes, VoC themes |
| `stakeholders` | Stakeholder profiles, preferences, past decisions, relationship history |
| `decisions` | Decision history with rationale, outcomes, and revisit triggers |
| `domain-knowledge` | Your ongoing PM learning: methodology, frameworks, lessons from launches |

You can create any focus area you want (`/second-brain init pricing-v3-research`, `/second-brain init nps-program`, etc.).

## How to Use

See the full operational manual at:
`.claude/skills/second-brain/references/starter-prompts.md`

Or just run `/second-brain init` to bootstrap a focus area and get the starter prompts printed for you.

## Core Rules (Enforced by Each Focus Area's CLAUDE.md)

- `raw/` is immutable. Never modify source files once added.
- `wiki/` is LLM-owned. The LLM creates, updates, and cross-links pages.
- Every factual claim in a wiki page cites its source: `[Source: filename.md]`.
- Every wiki page starts with YAML frontmatter (title, created, last_updated, source_count, status).
- Cross-reference aggressively with `[[page-name]]` Obsidian-style links.
- On contradiction: flag both claims, cite both sources, note which is more recent. Never silently overwrite.
- `wiki/index.md` gets updated on every ingest.
- `wiki/log.md` is append-only. Format: `## [YYYY-MM-DD] action | description`.

## Integration With the Rest of PM OS

Seven skills **write** to the brain (they offer "File to Second Brain?" at the end of their runs):
`/meeting-notes`, `/decision-doc`, `/user-research-synthesis`, `/voice-of-customer`, `/competitor-analysis`, `/weekly-review`, `/stakeholder-tactics --map`

Five skills **read** from the brain before generating:
`/prd-draft`, `/daily-plan`, `/meeting-agenda`, `/strategy-sprint`, `/write-prod-strategy` (plus `/decision-doc` reads past decisions)

## Recommended Tooling (Optional)

- **Obsidian** — Point it at this folder for clickable `[[links]]` and the graph view.
- **git** — The brain is all markdown. Commit it for history, diff, and undo.
- **agent-browser** (npm) — Enables `/second-brain ingest <url>` to auto-scrape. Without it, scraping falls back to WebFetch or manual paste.
