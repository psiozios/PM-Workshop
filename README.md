# PM Workshop

Your AI-powered copilot for modern product management. Built for Claude Code.

## Philosophy

Most PMs use AI the same way they use Google: one-off questions, zero context. This system works differently.

- **Context over prompting.** AI is only as good as the context you give it. PM Workshop organizes your company knowledge, writing styles, stakeholder profiles, and past decisions so every output sounds like it came from someone who actually works there.
- **Workflows, not chat.** 69 slash commands cover the full PM loop: strategy, research, PRDs, metrics, meetings, launches, prototyping, code, and retrospectives. Each one builds on the others.
- **Ship the draft, then iterate.** Documents are living artifacts. A 1-page PRD that ships Monday beats a 10-page spec that ships never.

## What You Get

- **Pre-built context library** with templates for company info, writing styles, stakeholder profiles, and strategic frameworks (7 Powers, JTBD, PLG Iceberg, Growth Loops, and more)
- **69 slash commands** for recurring PM tasks, from PRDs and meeting notes to pricing analysis and code reviews
- **7 sub-agents** for multi-perspective reviews (engineer, designer, executive, legal, UX researcher, skeptic, customer voice)
- **Knowledge assets** including 63 curated interview questions, validated survey templates, canvas templates, and 139 AI prompt references
- **Example PRDs** demonstrating modern best practices
- **Workflow chains** that connect skills together for daily routines, PRD lifecycles, strategic planning, and more

## Quick Start

### 1. Install Claude Code
```bash
curl -fsSL https://claude.ai/install.sh | bash

# Verify installation
claude --version
```

### 2. Set Up Your Workspace
```bash
# Clone this repository
git clone <repo-url>
cd pm-workshop

# Launch Claude Code in this directory
claude
```

### 3. Initialize Your Context
In your first Claude Code session, Claude will read the `CLAUDE.md` file and understand how to work with you. Fill out your context templates:
- `context-library/business-info-template.md` with your product details
- `context-library/stakeholder-template.md` with key stakeholder profiles
- `context-library/writing-style-*.md` with your preferred styles per audience

### 4. Try Your First Command
```
/daily-plan        # Get a prioritized plan for today
/prd-draft         # Draft a PRD with your company context
/meeting-notes     # Process a meeting transcript into action items
```

Pick whichever fits your day. Claude will ask clarifying questions and pull from your context automatically.

### 5. Connect Your Tools (Optional)

Connect Model Context Protocol (MCP) servers for real-time data access from your existing tools.

```bash
# In Claude Code, run:
/connect-mcps connect to amplitude
/connect-mcps connect to linear
/connect-mcps connect to figma
```

**Common tools to connect:**
- **Analytics**: Amplitude, Mixpanel, Posthog, Pendo, Heap
- **Project Management**: Linear, Jira, Asana, ClickUp
- **User Research**: Dovetail, UserTesting, Maze
- **Documentation**: Notion, Confluence, Coda
- **Communication**: Slack, Microsoft Teams

Claude will check for official remote MCP servers first (simplest method), then walk you through manual setup if needed. After connecting, just ask naturally:

- "What's the retention rate for users who signed up last month?"
- "Show me my open tasks in Linear"
- "Create a ticket for the login bug"

All skills work without MCPs by falling back to your context library files. You can connect tools at any time.

## Directory Structure

```
pm-workshop/
├── README.md                       # You are here
├── CLAUDE.md                       # Master instructions for Claude
├── LICENSE.md                      # MIT License
│
├── .claude/
│   └── skills/                     # 69 registered slash commands
│
├── setup/                          # Installation and configuration guides
├── advanced/                       # Advanced workflows and automation
├── sub-agents/                     # 7 specialized review agents
│
├── context-library/                # Your company/product context
│   ├── strategy/                   # Strategy docs + frameworks
│   ├── prds/                       # Your PRDs (reference)
│   ├── research/                   # User research, competitive analysis
│   ├── decisions/                  # Decision logs
│   ├── launches/                   # Launch plans, release notes
│   ├── metrics/                    # Analytics reports, A/B tests
│   ├── meetings/                   # Meeting notes
│   └── example-prds/              # Real PRD examples
│
├── templates/                      # Empty templates (7 templates)
│   └── knowledge/                  # PM knowledge assets (survey questions,
│                                   #   interview libraries, canvas templates,
│                                   #   AI prompt references)
│
└── outputs/                        # Active work (all Claude-generated files)
    ├── prds/                       # PRDs being drafted
    ├── meeting-notes/              # Processed meeting notes
    ├── status-updates/             # Stakeholder updates
    ├── analyses/                   # Impact sizing, feature analysis
    ├── research-synthesis/         # Interview synthesis
    ├── decisions/                  # Decision docs in progress
    ├── slack-messages/             # Draft communications
    ├── prototypes/                 # Prototype prompts, wireframes
    ├── journey-maps/              # User/customer journey maps
    ├── surveys/                    # Survey designs
    ├── roadmaps/                   # Roadmap drafts
    ├── board-decks/                # Executive presentations
    ├── business-canvases/          # BMC and Lean Canvas outputs
    ├── okrs/                       # OKR planning documents
    ├── pre-mortems/                # Pre-mortem risk registers
    ├── win-loss/                   # Win/loss analyses
    └── ...                         # Additional output types
```

## How It Works

### The Claude Code Advantage

Unlike ChatGPT or regular Claude:
- Claude Code **reads your entire project directory** automatically
- It **references your context files** in every conversation
- You can **run multiple instances** in parallel for different initiatives
- It **executes code** and **creates files** directly in your workspace

### Three Layers of Context

1. **Project Knowledge** (`context-library/`) - Company info, writing styles, stakeholder profiles, strategy frameworks, and past decisions that apply across all your work
2. **Skills** (`.claude/skills/`) - 69 registered slash commands for recurring tasks, each with built-in context routing and cross-skill integration
3. **Sub-Agents** (`sub-agents/`) - 7 specialized reviewers for multi-perspective feedback

When you ask Claude to draft a PRD, it automatically:
- References your company's business info
- Uses your preferred writing style
- Follows your PRD template
- Checks alignment with your strategy and OKRs
- Includes real examples from your library

## Available Skills (69 Total)

Type `/` in Claude Code to see the autocomplete menu with all commands.

### Core PM Workflows (18)
| Command | What it does |
|---------|-------------|
| `/connect-mcps` | Connect MCPs for real-time tool integration |
| `/prd-draft` | Create modern PRDs (supports `--brief` for one-pagers) |
| `/prd-lite` | Lightweight feature proposals for prioritization decisions |
| `/prd-review-panel` | Multi-agent PRD review from 7 perspectives |
| `/daily-plan` | Generate PM daily plan with context |
| `/weekly-plan` | Set next week's priorities tied to quarterly goals |
| `/weekly-review` | Review week's progress, meetings, and learnings |
| `/meeting-notes` | Transform transcripts into structured action items |
| `/meeting-agenda` | Create structured meeting agendas (supports `--oneonone`) |
| `/meeting-feedback` | Post-meeting effectiveness feedback |
| `/meeting-cleanup` | Batch process multiple meetings from a single day |
| `/status-update` | Generate stakeholder updates (supports `--story`) |
| `/decision-doc` | Document decisions (supports `--deliberate`, `--brainstorm`) |
| `/slack-message` | Draft team communications (supports `--thread`, `--difficult`, `--translate`) |
| `/board-deck` | Prepare executive and board-level presentations |
| `/editing-assistant` | Edit and improve any PM document |
| `/stakeholder-tactics` | Navigate stakeholder dynamics (map, align, resolve, prepare) |
| `/content-marketing` | Create product-led content assets |

### User Research and Interviews (7)
| Command | What it does |
|---------|-------------|
| `/user-interview` | Process user interviews systematically |
| `/interview-guide` | Create JTBD interview guides (63-question curated library) |
| `/interview-prep` | PM job interview preparation (supports `--mock`, `--brutal`) |
| `/interview-feedback` | Post-PM-interview debrief and improvement |
| `/user-research-synthesis` | Turn interviews into actionable insights |
| `/survey-builder` | Design surveys using validated methodologies (PMF, NPS, CSAT, CES, JTBD, Van Westendorp) |
| `/voice-of-customer` | Aggregate customer feedback into VoC reports |

### Strategic Frameworks (8)
| Command | What it does |
|---------|-------------|
| `/write-prod-strategy` | Product strategy docs using 7-component framework |
| `/strategy-sprint` | Create strategy in 1 day, 1 week, or 1 month timeframes |
| `/business-model-canvas` | Build or critique a Business Model Canvas or Lean Canvas |
| `/okr-planning` | Create and align quarterly OKRs |
| `/prioritize` | LNO Framework task classification |
| `/define-north-star` | Identify and validate your North Star Metric |
| `/metrics-framework` | Leading vs lagging indicators |
| `/journey-map` | Create user and customer journey maps |

### Product Analysis (13)
| Command | What it does |
|---------|-------------|
| `/impact-sizing` | Quantify feature value with driver trees and TAM/SAM/SOM |
| `/feature-metrics` | Define success metrics using STEDII framework |
| `/feature-results` | Post-launch analysis (supports `--edit`) |
| `/activation-analysis` | Diagnose activation funnel (supports `--debug`) |
| `/retention-analysis` | Cohort retention optimization |
| `/expansion-strategy` | Upsell, cross-sell, and account growth tactics |
| `/experiment-decision` | When to A/B test vs ship (supports `--brainstorm`) |
| `/experiment-metrics` | STEDII framework for trustworthy metrics |
| `/feature-request-analysis` | Synthesize and prioritize feature requests |
| `/root-cause-analysis` | Structured problem investigation (5 Whys, fishbone) |
| `/opportunity-sizing` | TAM/SAM/SOM market opportunity evaluation |
| `/pricing-analysis` | Pricing strategy design and analysis |
| `/win-loss-analysis` | Synthesize sales win/loss data into product insights |

### Competitive Intelligence (2)
| Command | What it does |
|---------|-------------|
| `/competitor-analysis` | Deep competitive analysis with SWOT, Porter's Five Forces, PESTEL |
| `/sales-battlecard` | Create competitive battle cards for the sales team |

### Prototyping and Design (4)
| Command | What it does |
|---------|-------------|
| `/prototype` | Advanced prototyping with Artifacts, Figma, Lovable, v0, Bolt |
| `/generate-ai-prototype` | Generate AI prototype prompts from PRD specs |
| `/napkin-sketch` | ASCII wireframes and browser capture |
| `/prototype-feedback` | Build, review, iterate workflow (supports `--design`) |

### Development and Execution (15)
| Command | What it does |
|---------|-------------|
| `/create-tickets` | Create tickets via Linear/Jira MCP or formatted text (supports `--quick`, `--stories`) |
| `/launch-checklist` | Comprehensive launch planning (supports `--testplan`) |
| `/pre-mortem` | Structured pre-mortem before launches or major decisions |
| `/post-mortem` | Blameless post-mortem after incidents or failed launches |
| `/sprint-planning` | Sprint planning and backlog grooming |
| `/analytics-instrumentation` | Design tracking plans for product features |
| `/deprecation-plan` | Plan and execute feature deprecations |
| `/ai-quality-debug` | Evaluate and debug AI/ML feature quality |
| `/explore-codebase` | Pre-implementation codebase exploration (read-only) |
| `/execution-plan` | Create tracked markdown execution plans |
| `/code-first-draft` | Initial feature implementation (supports `--from-plan`) |
| `/code-review` | Comprehensive code review across 8 dimensions |
| `/peer-review` | Cross-model code review verification |
| `/update-docs` | Update documentation after code changes |
| `/cto-consult` | CTO simulation for technical pushback and phase planning |

### Learning and Growth (1)
| Command | What it does |
|---------|-------------|
| `/learning-mode` | Three-level teaching mode for technical PMs |

### Fun (1)
| Command | What it does |
|---------|-------------|
| `/ralph-wiggum` | Devil's advocate reviewer with humor and sharp critique |

## Recommended Workflows

These workflows chain skills together for common PM activities.

**Daily PM Workflow:**
`/daily-plan` → take meeting notes → `/meeting-notes` → `/slack-message` for follow-ups

**Weekly PM Cycle:**
Monday `/weekly-plan` → daily workflow → Friday `/weekly-review` → `/status-update`

**PRD Lifecycle:**
`/user-research-synthesis` → `/impact-sizing` → `/prd-draft` → `/prd-review-panel` → `/create-tickets` → `/launch-checklist` → `/feature-results`

**Strategic Planning:**
`/define-north-star` → `/metrics-framework` → `/write-prod-strategy` → `/prioritize`

**Build with AI (for PMs who code):**
`/cto-consult` → `/explore-codebase` → `/execution-plan` → `/code-first-draft --from-plan` → `/code-review` → `/update-docs`

**Pre-Launch Risk:**
`/pre-mortem` → `/launch-checklist` → `/decision-doc`

**New Feature Research:**
`/opportunity-sizing` → `/survey-builder` → `/interview-guide` → `/voice-of-customer` → `/prd-draft`

## Pro Tips

1. **Use dictation.** Talk to Claude Code like a colleague. Much faster than typing.
2. **Gossip to your copilot.** "You won't believe what just happened in my stakeholder meeting..." Keep it updated on organizational dynamics.
3. **Plan mode for complex tasks.** Use `Shift+Tab` to review Claude's plan before it executes.
4. **Multiple instances.** Run 2-3 Claude Code sessions in parallel for different initiatives.
5. **Clear context when switching.** Use `clear` to reset the conversation for a new topic.
6. **Upload existing docs early.** Drop your PRDs, strategy docs, research, and meeting notes into `context-library/` for richer outputs from day one.

## How Is This Different From Regular AI Tools?

| Regular ChatGPT/Claude | PM Workshop |
|------------------------|-------------|
| Forgets your company context | Always knows your business, team, and stakeholders |
| Generic output | Matches your writing style automatically |
| One-shot responses | Evolves PRDs through multiple stages |
| No file system access | Creates and edits files directly in your project |
| Single perspective | Multi-agent reviews from 7 perspectives |
| No tool integration | Connects to your analytics, PM tools, and more via MCPs |

## Getting Started Checklist

- [ ] Install Claude Code
- [ ] Clone this repo and run `claude` from the project root
- [ ] Fill out `context-library/business-info-template.md` with your product details
- [ ] Add your writing styles to `context-library/writing-style-*.md`
- [ ] Add stakeholder profiles to `context-library/stakeholder-template.md`
- [ ] Connect your tools with `/connect-mcps connect to [tool]`
- [ ] Try `/daily-plan` or `/prd-draft` for your first command
- [ ] Upload existing PRDs, strategy docs, and research to `context-library/`

## License

MIT. See [LICENSE.md](LICENSE.md).

---

**Ready to start? Fill out your business context, run `/daily-plan`, and see what an AI copilot that actually knows your company feels like.**
