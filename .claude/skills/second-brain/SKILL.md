---
name: second-brain
description: Build and maintain a compounding PM knowledge base inspired by Karpathy's LLM Wiki pattern. Scaffolds focus-area wikis under context-library/second-brain/, ingests sources with cross-linking, answers questions with citations, files good answers back, and surfaces gaps. Modes, init, ingest, query, compile, explore, lint, prep, status. Use when the PM wants a personal wiki, knowledge base, or persistent memory that gets smarter with every use.
disable-model-invocation: false
user-invocable: true
---

# /second-brain - A Knowledge Base That Compounds

Most AI tools (NotebookLM, ChatGPT uploads, basic RAG) re-derive answers from scratch every time. This is different. Your second brain is a **compiled, interlinked wiki** that the LLM maintains for you. Every new source updates multiple pages. Every good answer gets filed back. Every week the brain is smarter than it was last week.

You curate sources and ask questions. The LLM does the summarizing, cross-referencing, filing, and bookkeeping.

## Quick Start

```
/second-brain init competitive-intelligence
# Scaffolds context-library/second-brain/competitive-intelligence/

/second-brain ingest path/to/competitor-pricing-page.md
# Reads the source, summarizes, cross-links, flags contradictions, logs

/second-brain query "how does Competitor X price enterprise?"
# Reads index, finds relevant pages, synthesizes with [Source:] citations,
# offers to file the answer back

/second-brain explore      # Surface unexplored connections
/second-brain lint         # Health check: contradictions, stale claims, orphans
/second-brain prep "Q3 planning review"   # One-page briefing from the brain
/second-brain status       # All focus areas, page counts, last-ingest dates
```

## When to Use

- You're accumulating knowledge about a domain (competitors, customers, a technical area, your product) and want it to stick
- Before writing a PRD, strategy doc, or decision doc, you want to pull together everything you already know
- You've noticed the same research keeps getting re-done because the prior work gets buried in `outputs/`
- You want a persistent store for stakeholder profiles, decision history, customer insights, or competitive intel
- You're prepping for a meeting and want a one-pager that assembles relevant context with citations
- You want CLAUDE.md's "Self-Updating System" to actually have a concrete implementation

## Core Concept: The Compounding Loop

```
Sources (raw/)  →  Wiki (wiki/)  →  Queries (outputs/)  →  File back into wiki/
         ↑___________________________________________________________|
                       Knowledge compounds every cycle
```

Unlike standard skills that produce one-shot outputs, the brain **evolves**. Run it for a month and the brain knows your product, customers, competitors, and decision history well enough to answer most prep questions instantly.

---

## Architecture

### Location

All focus areas live under `context-library/second-brain/`. This fits the "reference & history" convention (CLAUDE.md line 409-419 — context-library is for reference, outputs is for active work).

```
context-library/second-brain/
├── README.md                         # How the brain works (created once)
└── {focus-slug}/                     # One folder per focus area
    ├── CLAUDE.md                     # Per-focus schema (rules for this brain)
    ├── raw/                          # Immutable source documents
    │   └── assets/                   # Images, screenshots, diagrams
    ├── wiki/                         # LLM-maintained compiled knowledge
    │   ├── index.md                  # Catalog of every page, one-line each
    │   ├── log.md                    # Append-only activity log
    │   └── *.md                      # Wiki pages (one per concept)
    └── outputs/                      # Briefings, reports, lint reports
```

### Default focus areas (offered at first init)

| Slug | What goes in it | Which skills feed it |
|---|---|---|
| `product-knowledge` | Your product architecture, key metrics baselines, activation/retention facts, internal docs | `/daily-plan`, `/feature-results`, `/analytics-instrumentation` |
| `competitive-intelligence` | Competitors, positioning, pricing, messaging, recent moves | `/competitor-analysis`, `/sales-battlecard`, `/win-loss-analysis` |
| `customer-insights` | Jobs-to-be-done, pain points, segments, verbatim quotes, VoC themes | `/user-research-synthesis`, `/voice-of-customer`, `/user-interview` |
| `stakeholders` | Stakeholder profiles, preferences, past decisions with them, relationship history | `/stakeholder-tactics --map`, `/meeting-notes` |
| `decisions` | Decision history, rationale, outcomes, revisit triggers | `/decision-doc`, `/weekly-review` |
| `domain-knowledge` | Your ongoing PM learning: methodology, frameworks you've adapted, lessons from launches | `/weekly-review`, `/feature-results`, `/post-mortem` |

The PM can also create arbitrary focus areas (e.g., `nps-program`, `pricing-v3-research`).

### Wiki page conventions (preserved from the original pattern)

Every wiki page starts with:

```yaml
---
title: <Page Title>
created: YYYY-MM-DD
last_updated: YYYY-MM-DD
source_count: <n>
status: draft | working | stable
---
```

Followed by a one-paragraph summary, then the body.

**Cross-linking rules:**
- Use `[[page-name]]` Obsidian-style links between pages. Link aggressively.
- Every factual claim cites its source inline: `[Source: filename.md]`
- When a new source contradicts an existing claim: flag both, cite both, note which is more recent. Never silently overwrite.

**Index and log:**
- `wiki/index.md` — catalog, updated on every ingest. Organize by category.
- `wiki/log.md` — append-only. Format: `## [YYYY-MM-DD] {action} | {one-line description}` where action is `init | ingest | query | explore | lint | update`.

---

## Context Routing Logic (Internal - for Claude)

Before running any mode, check:

| Mode | Check first | Then |
|---|---|---|
| `init` | Does `context-library/second-brain/{slug}/` already exist? | If yes, do not clobber. Ask whether to use existing or pick new slug. |
| `ingest` | Which focus area does this source belong to? | If ambiguous, ask. A single source can be ingested into multiple focus areas. |
| `query` | Which focus area(s) are relevant? | Read all relevant `wiki/index.md` files first, then drill. |
| `prep` | Which focus areas touch this topic? | Pull from all of them; cite across. |
| `lint`, `explore` | Operates on one focus area at a time. | Ask which if not specified. |
| `status` | Scan `context-library/second-brain/*/` | Report per-focus counts. |

**Graceful degradation:**
- If `agent-browser` is installed (`which agent-browser`), use it to scrape URLs in `ingest`. If not, use `WebFetch` tool or ask the PM to paste content manually. Do not prompt them to install it unless they ask.
- If the brain doesn't exist yet and the PM asks a query, offer to run `/second-brain init` first.

---

## Mode 1: `init` — Scaffold a New Focus Area

**Trigger:** `/second-brain init [focus-slug]`

### Flow

**If slug provided:** slugify (lowercase, hyphens, no special chars) and proceed to scaffold.

**If no slug:** ask one question at a time:

1. "What do you want to build a knowledge base around? (a product area, competitors, customers, a domain you're learning — anything you're accumulating knowledge about)"
2. "What are 3-5 things you're always trying to stay on top of within that area?"
3. "Any URLs or files you'd like to seed the brain with right now? (optional — you can always add more later)"

Or offer the default focus areas (product-knowledge, competitive-intelligence, customer-insights, stakeholders, decisions, domain-knowledge) and let them pick one or more.

### Scaffold

Create:
- `context-library/second-brain/{slug}/raw/assets/`
- `context-library/second-brain/{slug}/wiki/`
- `context-library/second-brain/{slug}/outputs/`
- `context-library/second-brain/{slug}/CLAUDE.md` (per-focus schema, template below)
- `context-library/second-brain/{slug}/wiki/index.md` (bootstrap)
- `context-library/second-brain/{slug}/wiki/log.md` (with the `init` entry)

### Per-focus CLAUDE.md template

```markdown
# {Focus Title} — Second Brain Schema

## What This Is
A personal knowledge base about {focus}. The human curates sources and asks questions. The LLM writes and maintains the entire wiki.

## Architecture
- raw/ contains immutable source documents. Never modify files in raw/.
- raw/assets/ holds images and diagrams referenced by sources.
- wiki/ is LLM-owned: pages, cross-references, index, log.
- outputs/ holds generated briefings, reports, and lint reports. Promote valuable outputs into wiki/ as new pages.

## Wiki Conventions
- Every page: its own .md file, YAML frontmatter (title, created, last_updated, source_count, status), one-paragraph summary.
- [[page-name]] cross-links. Link aggressively.
- Every factual claim cites source: [Source: filename.md].
- On contradiction: flag both claims, cite both, note recency. Never silently overwrite.

## Index and Log
- wiki/index.md: catalog of every page with a one-line description, organized by category.
- wiki/log.md: append-only. Format: `## [YYYY-MM-DD] action | Description`. Actions: init, ingest, query, explore, lint, update.

## Ingest Workflow (when a source lands in raw/)
1. Read the full source.
2. Summarize key takeaways to the PM.
3. Create or update a summary page in wiki/.
4. Update wiki/index.md.
5. Update ALL relevant existing pages — one source often touches many.
6. Add backlinks from existing pages to new content.
7. Flag contradictions with existing content.
8. Append entry to wiki/log.md.

## Query Workflow (when the PM asks a question)
1. Read wiki/index.md to find relevant pages.
2. Read those pages, following [[links]] for connected context.
3. Synthesize answer with citations to wiki pages.
4. Render in the best format: wiki page, comparison table, briefing, Marp slides, or visualization.
5. If the answer is substantial, offer to file it back into wiki/.
6. If the question reveals a gap, flag it and suggest what sources would fill it.

## Focus Areas
- {interest 1}
- {interest 2}
- {interest 3}
```

### Bootstrap `wiki/index.md`

```markdown
# Wiki Index — {Focus Title}

_Last updated: {today}_

Maintained by the LLM. Every page listed with a one-line summary.

_(No pages yet. Add sources to raw/ and run `/second-brain compile` to build the wiki.)_
```

### Bootstrap `wiki/log.md`

```markdown
# Wiki Log — {Focus Title}

Append-only record of all wiki activity.

## [{today}] init | Knowledge base scaffolded
Focus: {focus}. Interests: {interests}. Ready for first sources.
```

### Scrape initial URLs (if provided)

For each URL:
1. Check `which agent-browser`.
2. If available: open → wait networkidle → extract text from `article` (fallback `main`, then `body`) → get title → save as `raw/{slugified-title}.md` → close.
3. If not available: use `WebFetch`, or ask the PM to paste content into a new file in `raw/`.

### Finish

After scaffolding, print the next-steps summary and point the PM to `.claude/skills/second-brain/references/starter-prompts.md` for the full prompt library.

---

## Mode 2: `ingest` — Add a Source

**Trigger:** `/second-brain ingest <path-or-url> [--focus=<slug>]`

### Flow

1. **Locate source.** If URL and agent-browser is available, scrape into `raw/`. If local file outside the brain, copy into `raw/` with a descriptive slug. If already in `raw/`, proceed.
2. **Identify focus area.** Use `--focus` flag if passed. Otherwise infer from content and ask to confirm. A source can be ingested into multiple focus areas (e.g., a customer interview touches both `customer-insights` and `product-knowledge`).
3. **Read fully.** Don't skim.
4. **Discuss takeaways.** Summarize the 3-5 key claims to the PM before writing anything. Let them redirect if they want a different angle.
5. **Write/update wiki:**
   - Create or update the relevant summary page in `wiki/`.
   - Update `wiki/index.md` (new page entry + any category reorganization).
   - Update ALL other pages where this source adds, confirms, or contradicts a claim.
   - Add backlinks from existing pages to new content.
   - On contradiction: flag explicitly with both citations, note recency.
6. **Log.** Append `## [YYYY-MM-DD] ingest | {filename} — touched N pages` to `wiki/log.md`.
7. **Report.** Tell the PM exactly which pages were created or updated so they can scan the diff.

**One source at a time produces better wikis than batch ingest.** Encourage the PM to ingest individually and discuss takeaways. Use `compile` only for bulk catch-up.

---

## Mode 3: `query` — Ask the Brain

**Trigger:** `/second-brain query "<question>" [--focus=<slug>]`

### Flow

1. **Identify focus area(s).** Use `--focus` if given. Otherwise pick the most relevant — if the question crosses focus areas (e.g., "how does Competitor X match our pricing for enterprise customers"), read multiple indexes.
2. **Read `wiki/index.md`** for each relevant focus area.
3. **Drill into pages.** Follow `[[links]]` for connected context. Stop when you have enough; don't read everything.
4. **Synthesize answer.**
   - Every factual claim cites `[Source: wiki-page.md]` or `[Source: raw-source.md]`.
   - If evidence is thin, say so. If the wiki has contradictions on this topic, surface them.
   - Render in the best format: prose, comparison table, briefing, Marp slide outline, decision matrix.
5. **Offer to file back.** "Save this as `wiki/{suggested-name}.md` and update the index?" If yes, do it and append an `update` entry to the log.
6. **Flag gaps.** If the question surfaced something the wiki doesn't cover well, suggest 1-3 sources that would fill the gap.

---

## Mode 4: `compile` — Batch Build/Rebuild

**Trigger:** `/second-brain compile [--focus=<slug>]`

Use when you've dropped many sources into `raw/` at once and want the LLM to process them all. Weaker than one-at-a-time ingest but valuable for initial bootstrap or catch-up.

### Flow

1. For each file in `raw/` not yet in the log as ingested:
   - Run the full ingest workflow.
2. After the batch, show the updated `wiki/index.md` so the PM can see the shape of what was built.
3. Append a batch entry to the log: `## [YYYY-MM-DD] compile | Processed N sources, touched M pages`.

---

## Mode 5: `explore` — Find Unexplored Connections

**Trigger:** `/second-brain explore [--focus=<slug>]`

### Flow

1. Read `wiki/index.md` and scan all pages for topics that appear in multiple contexts but aren't directly linked.
2. Identify the 5 most interesting unexplored connections.
3. For each: explain what insight it might reveal and what source would confirm it.
4. Offer to create new pages or cross-references for confirmed connections.
5. Save the explore session as `outputs/explore-YYYY-MM-DD.md` and log it.

---

## Mode 6: `lint` — Health Check

**Trigger:** `/second-brain lint [--focus=<slug>]`

### Checks
- Contradictions between pages
- Stale claims superseded by newer sources (check against `last_updated` and source dates)
- Orphan pages with no inbound `[[links]]`
- Important concepts mentioned across pages but never given their own page
- Missing cross-references (same entity named but not linked)
- Claims without `[Source:]` citations

**Output:** `outputs/lint-report-YYYY-MM-DD.md` with each issue, location, and suggested fix. Also suggest 3 sources that would fill the biggest gaps. Append `lint` entry to log.

Run roughly weekly or after every 10 ingests.

---

## Mode 7: `prep` — Meeting Briefing

**Trigger:** `/second-brain prep "<topic>"`

### Flow

1. Identify which focus areas touch the topic (can be multiple).
2. Read relevant indexes and pages.
3. Produce a one-page briefing structured as:
   - **Current state** — what we know, with citations
   - **Key tensions** — contradictions, open debates, trade-offs
   - **Open questions** — what the wiki can't answer yet
   - **Recommendation** — what I'd argue for, given the evidence
4. Save to `outputs/briefing-{topic-slug}-YYYY-MM-DD.md` and log.
5. Offer to also file the briefing as a wiki page if it's evergreen.

This is the mode the PM will use most often. It turns the brain into a 5-minute prep ritual before any meeting.

---

## Mode 8: `status` — Brain Dashboard

**Trigger:** `/second-brain status`

Report:
- All focus areas under `context-library/second-brain/`
- Per focus area: page count, raw source count, last-ingest date, top 5 most-linked pages (hubs), count of orphan pages, count of open contradictions flagged
- Brains that haven't been touched in >30 days (stale)
- Suggested next action per focus area ("lint overdue", "low source count", "big gap flagged in last query")

---

## Integration With Existing Skills

The brain compounds only if outputs flow into it. These skills will prompt "File to Second Brain?" at the end of their runs:

| Skill | What gets filed | Target focus area |
|---|---|---|
| `/meeting-notes` | Key decisions + new facts from the meeting | `decisions` + topic-relevant area |
| `/decision-doc` | Decision, rationale, options considered, revisit trigger | `decisions` |
| `/user-research-synthesis` | Themes, jobs-to-be-done, verbatim quotes | `customer-insights` |
| `/voice-of-customer` | Consolidated themes with source counts | `customer-insights` |
| `/competitor-analysis` | Competitor updates, positioning deltas | `competitive-intelligence` |
| `/weekly-review` | Lessons learned, surprising patterns | `domain-knowledge` |
| `/stakeholder-tactics --map` | Stakeholder profile updates (replaces local store) | `stakeholders` |

These skills **query** the brain at start:

| Skill | What it pulls |
|---|---|
| `/prd-draft` | Background section: product, customer, competitive context with citations |
| `/daily-plan` | "What do I know about today's meeting topics" |
| `/meeting-agenda` | Calls `/second-brain prep` on the meeting topic |
| `/strategy-sprint`, `/write-prod-strategy` | Evidence base from all relevant focus areas |
| `/decision-doc` | Related past decisions — don't re-relitigate old debates |

---

## Output Locations

- **Wiki pages** → `context-library/second-brain/{focus}/wiki/*.md` (LLM-owned, the permanent store)
- **Briefings, lint reports, explore reports** → `context-library/second-brain/{focus}/outputs/*.md`
- **Raw sources** → `context-library/second-brain/{focus}/raw/*.md`

Note: the brain uses its own nested `outputs/` folder (per focus area) rather than the top-level `outputs/`. This keeps each focus area self-contained and portable — you could tarball a focus area and share it.

---

## Tips for Best Results

- **Ingest one source at a time** whenever you can. Discuss takeaways. That discussion is where the brain gets sharp.
- **File good answers back.** The single most important habit. A briefing that stays in chat disappears. A briefing filed as a wiki page makes every future query smarter.
- **Run `lint` weekly** or every 10 ingests, whichever comes first. Catches contradictions before they metastasize.
- **Use `prep` before every meeting.** Five minutes of brain-assisted prep beats an hour of scrambling.
- **Let it grow organically.** You don't need 10 focus areas on day one. Start with 2-3 that match the work you're doing this month.
- **Obsidian is optional but lovely.** Point Obsidian at `context-library/second-brain/` and the `[[links]]` become clickable plus you get the graph view. Any markdown viewer works.
- **Git-friendly by design.** The brain is all markdown. Commit it. You get history, diff, and undo for free.

---

## Scale Notes

The index-based approach holds up to ~100 raw sources and a few hundred wiki pages per focus area. Beyond that:
- Consider splitting a focus area into sub-areas (e.g., `competitive-intelligence` → `comp-enterprise`, `comp-smb`)
- Consider adding a search tool like [qmd](https://github.com/tobi/qmd) (local markdown search, BM25/vector hybrid, works as CLI or MCP server)

---

## Related Skills

- **Writes to the brain:** `/meeting-notes`, `/decision-doc`, `/user-research-synthesis`, `/voice-of-customer`, `/competitor-analysis`, `/weekly-review`, `/stakeholder-tactics`
- **Reads from the brain:** `/prd-draft`, `/daily-plan`, `/meeting-agenda`, `/strategy-sprint`, `/write-prod-strategy`, `/decision-doc`
- **Complementary:** `/learning-mode` (teaches new concepts; complements `domain-knowledge` focus area)

---

## Reference

Full operational prompt library — copy-paste versions of every mode plus PM-specific queries — lives at:

`.claude/skills/second-brain/references/starter-prompts.md`
