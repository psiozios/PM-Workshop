# Second Brain — Starter Prompts

Copy-paste versions of every mode, plus the PM-specific query prompts. These are the operational manual for your brain.

All prompts follow the schema defined in each focus area's `CLAUDE.md` (under `context-library/second-brain/{focus}/`).

---

## Core Operations

### Initialize a New Focus Area

> /second-brain init competitive-intelligence

Or interactively:

> /second-brain init
> (Answer: What area? What 3-5 things do you want to stay on top of? Any seed URLs?)

---

### Compile (Run This After a Batch of Sources)

Drop multiple files into `context-library/second-brain/{focus}/raw/` then:

> /second-brain compile --focus=competitive-intelligence
>
> Read the CLAUDE.md at context-library/second-brain/competitive-intelligence/. Then process every file in raw/ not yet logged as ingested. For each: read fully, create or update a summary page in wiki/, update index.md, update all relevant existing pages, add backlinks, flag contradictions, log the ingest. When done, show me the updated index.md so I can see the shape.

---

### Ingest One Source (Preferred)

> /second-brain ingest path/to/source.md --focus=customer-insights
>
> Read the source fully. Summarize the 3-5 key takeaways to me first. Once we've discussed, update the wiki: create/update the summary page, update index, update all relevant existing pages, add backlinks, flag contradictions, log. Show me which pages you created or updated.

**One at a time with discussion produces dramatically sharper wikis than batch `compile`.**

---

### Scrape a URL Into raw/

> /second-brain ingest https://example.com/article --focus=competitive-intelligence

If `agent-browser` is installed, the skill will scrape automatically. Otherwise use WebFetch or paste the content manually into `raw/`.

Install agent-browser (optional):
```
npm install -g agent-browser && agent-browser install
```

---

### Health Check

> /second-brain lint --focus=customer-insights
>
> Run the full lint workflow per CLAUDE.md. Output to outputs/lint-report-[date].md. Flag contradictions, stale claims, orphan pages, missing cross-references, claims without source citations. Suggest 3 sources that would fill the biggest gaps.

Run weekly, or after every 10 ingests.

---

### Explore Connections

> /second-brain explore --focus=product-knowledge
>
> Read wiki/index.md. Identify the 5 most interesting unexplored connections between existing topics. For each, explain what insight it might reveal and what source would confirm it. If any are strong enough, create new wiki pages or add cross-references and update the index.

---

### Status Dashboard

> /second-brain status
>
> Scan every focus area under context-library/second-brain/. Report page counts, raw source counts, last-ingest dates, top 5 hub pages, orphan counts, open contradictions, and any brain that hasn't been touched in >30 days.

---

## PM Queries That Turn the Brain Into a Working Tool

Use these once a focus area has 10+ wiki pages.

### Explore What You Know

> /second-brain query "What are the 5 biggest gaps in this knowledge base? What do I think I know that's only backed by a single source? What should I research next to strengthen the weakest areas?" --focus=competitive-intelligence

> /second-brain query "What patterns keep showing up across multiple sources? What themes are emerging that I might not have noticed?" --focus=customer-insights

---

### Prep for a Meeting

> /second-brain prep "Q3 roadmap review with Maya"

Will produce a one-page briefing structured as current state → key tensions → open questions → my recommendation, with citations. Saved to `outputs/briefing-*.md`.

> /second-brain prep "enterprise pricing competitive landscape"

---

### Competitive Intelligence

> /second-brain query "Give me a full picture of Competitor X: what they're doing, where they're strong, where they're weak, and what's changed recently. If the wiki is thin, tell me exactly what sources to add." --focus=competitive-intelligence

> /second-brain query "Compare Competitor A vs Competitor B across positioning, pricing, and ICP. Build a comparison page and file it into wiki/." --focus=competitive-intelligence

---

### Spot Contradictions and Staleness

> /second-brain query "What claims in the wiki contradict each other? Which source is more recent or reliable for each contradiction?" --focus=customer-insights

> /second-brain query "Which wiki pages are most likely outdated based on newer sources? Prioritize what needs refreshing." --focus=competitive-intelligence

---

### Build a PRD Background Section

> /second-brain query "I'm writing a PRD for <feature>. Pull everything relevant from the wiki — market context, user problems, competitive landscape, technical constraints — into a background section I can paste. Cite sources so I can link to them."

Typically crosses `product-knowledge`, `customer-insights`, and `competitive-intelligence`.

---

### Synthesize User/Customer Knowledge

> /second-brain query "Pull together everything the wiki knows about <user segment>. Pain points, what they care about, where my understanding is thin." --focus=customer-insights

> /second-brain query "What feature requests, complaints, or workarounds show up across multiple sources? Group by theme." --focus=customer-insights

---

### Make a Decision

> /second-brain query "I'm deciding between Option A and Option B for <context>. Based on everything in the wiki, what evidence supports each? Where is the data strong and where am I guessing? File the analysis back into wiki/." --focus=decisions

Often called from `/decision-doc` — the decision doc gets the evidence layer from the brain.

---

### Prep a Stakeholder Conversation

> /second-brain prep "1:1 with VP Sales — enterprise deal blockers"

Will pull from `stakeholders` (relationship history + preferences) and `competitive-intelligence` / `customer-insights` / `decisions` as needed.

---

## Generate Shareable Outputs

> /second-brain query "Based on everything in wiki/, write a 500-word executive briefing on <topic>. Cite sources. Structure: current state, key tensions, open questions, recommended next steps. Save to outputs/."

> /second-brain query "Write an executive summary of <topic> as a Marp slide deck outline. 5 slides max. Save to outputs/."

> /second-brain query "Create a comparison table of <entities> across <dimensions>. Render as markdown. Save to outputs/."

> /second-brain query "Write a market landscape overview. Save to outputs/."

---

## The Compounding Loop

The single most important habit: **file good answers back into the wiki.**

After any query that produced a substantial answer:

> Save that answer as a new wiki page under {focus}/wiki/ and update the index.

> File that analysis into the wiki so I can reference it next time.

A comparison, an analysis, a briefing, a pattern you noticed — these are all knowledge. If they stay in chat history they disappear. If they go back into the wiki, every future query gets smarter.

That's the whole system. Sources go in. Wiki gets compiled. You query it. Good answers go back. Knowledge compounds.

---

## Integration With Other PM OS Skills

**These skills write to the brain** (they'll ask "File to Second Brain?" at the end of their runs):

- `/meeting-notes` → `decisions` + relevant topic focus
- `/decision-doc` → `decisions`
- `/user-research-synthesis` → `customer-insights`
- `/voice-of-customer` → `customer-insights`
- `/competitor-analysis` → `competitive-intelligence`
- `/weekly-review` → `domain-knowledge`
- `/stakeholder-tactics --map` → `stakeholders`

**These skills read from the brain** (they pull evidence before generating):

- `/prd-draft` — background section from `product-knowledge` + `customer-insights` + `competitive-intelligence`
- `/daily-plan` — what you know about today's topics
- `/meeting-agenda` — auto-runs `/second-brain prep`
- `/strategy-sprint`, `/write-prod-strategy` — evidence base across focus areas
- `/decision-doc` — past related decisions from `decisions`

---

## Tips

- **Open `context-library/second-brain/` in Obsidian** for clickable `[[links]]` and the graph view. Any markdown editor works.
- **Commit the brain to git.** It's all markdown. You get history, diff, and undo for free.
- **Start small.** Pick 2-3 focus areas that match what you're working on this month. Add more only when you have sources for them.
- **Lint weekly.** Contradictions caught early are cheap; late are expensive.
- **Use `/second-brain prep` before meetings.** Five minutes of brain-assisted prep beats an hour of scrambling.
