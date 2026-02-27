# PM Operating System

Your AI-powered copilot for modern product management. Built for Claude Code and Cursor.

## Philosophy

Most PMs use AI the same way they use Google: one-off questions, zero context. This system works differently.

- **Context over prompting.** AI is only as good as the context you give it. PM OS organizes your company knowledge, writing styles, stakeholder profiles, and past decisions so every output sounds like it came from someone who actually works there.
- **Workflows, not chat.** 41 slash commands cover the full PM loop: strategy, research, PRDs, metrics, meetings, launches, and retrospectives. Each one builds on the others.
- **Ship the draft, then iterate.** Documents are living artifacts. A 1-page PRD that ships Monday beats a 10-page spec that ships never.

## What You Get

This operating system transforms Claude Code into your personal PM copilot:

- **Pre-built context library** - Templates for company info, writing styles, stakeholder profiles
- **Slash commands** - One-line prompts for common PM tasks (PRDs, meeting notes, Slack messages)
- **Sub-agents** - Specialized reviewers (engineer, designer, exec, legal perspectives)
- **Example PRDs** - Real-world templates demonstrating modern best practices
- **Workflows** - End-to-end processes for user research, PRD creation, competitive intel

## Quick Start

### 1. Install Claude Code
```bash
curl -fsSL https://claude.ai/install.sh | bash

# Verify installation
claude --version
```

### 2. Set Up Your Workspace
```bash
# Clone or download this repository
cd pm-operating-system

# Launch Claude Code in this directory
claude
```

### 3. Initialize Your Context
In your first Claude Code session:
```
init
```

Claude will read the `CLAUDE.md` file and understand how to work with you.

### 4. Try Your First Command
```
/daily-plan        # Get a prioritized plan for today
/prd-draft         # Draft a PRD with your company context
/meeting-notes     # Process a meeting transcript into action items
```

Pick whichever fits your day. Claude will ask clarifying questions and pull from your context automatically.

## Directory Structure

```
pm-operating-system/
├── README.md                    # You are here
├── CLAUDE.md                    # Master instructions for Claude
│
├── .claude/                     # Claude Code configuration
│   └── skills/                  # 41 registered slash commands
│
├── setup/                       # Installation and configuration
├── context-library/             # Your company/product context
│   ├── strategy/                # Strategy docs + frameworks (7 Powers, JTBD, etc.)
│   ├── prds/                    # Your PRDs (reference)
│   ├── research/                # User research, competitive analysis
│   ├── decisions/               # Decision logs
│   ├── launches/                # Launch plans, release notes
│   ├── metrics/                 # Analytics reports, A/B tests
│   ├── meetings/                # Meeting notes
│   └── example-prds/            # Real PRD examples
├── sub-agents/                  # Specialized review agents (7 perspectives)
├── templates/                   # Empty templates (5 templates)
├── advanced/                    # Advanced workflows & automation
└── outputs/                     # Active work (PRDs, notes, updates)
```

## How This System Works

### The Claude Code Advantage

Unlike ChatGPT or regular Claude:
- Claude Code **reads your entire project directory** automatically
- It **references your context files** in every conversation
- You can **run multiple instances** in parallel for different initiatives
- It **executes code** and **creates files** directly in your workspace

### Three Layers of Context

1. **Project Knowledge** (`context-library/`) - Company info, writing styles, stakeholder profiles that apply across all your work
2. **Skills** (`.claude/skills/`) - 41 registered slash commands for recurring tasks
3. **Sub-Agents** (`sub-agents/`) - Specialized reviewers for different perspectives

When you ask Claude to draft a PRD, it automatically:
- References your company's business info
- Uses your preferred writing style
- Follows your PRD template
- Includes real examples from your library

### 5. Connect Your Tools (MCPs)

Connect Model Context Protocol (MCP) servers for real-time data access from your tools.

**Common PM Tools to Connect:**
- **Analytics**: Amplitude, Mixpanel, Posthog, Pendo, Heap
- **Project Management**: Linear, Jira, Asana, ClickUp
- **User Research**: Dovetail, UserTesting, Maze
- **Documentation**: Notion, Confluence, Coda
- **Communication**: Slack, Microsoft Teams

**How to Connect:**
```bash
# In Claude Code, run:
/connect-mcps connect to amplitude
/connect-mcps connect to linear
/connect-mcps connect to figma
```

Claude will automatically:
1. Check for official remote MCP servers (simplest method - e.g., Figma)
2. Guide you to use `claude mcp add --transport http [tool] [url]` if available
3. Or walk you through manual setup (OAuth, API tokens) if needed

**Note:** Tools like Figma have remote servers - no API keys needed, just authenticate!

**Why Connect MCPs?**
- **Ask natural language questions** and get real-time data
  - "Give me metrics on feature X in the last 2 weeks" → Claude queries Amplitude
  - "Show my open tasks" → Claude queries Linear
  - "What did users say about checkout" → Claude queries Dovetail
- **No more context switching** to multiple tools
- **Intelligent routing** - Claude figures out which tool to use based on your question

**After connecting**, just talk naturally:
- "What's the retention rate for users who signed up last month?"
- "Show me the funnel for checkout"
- "Create a ticket for the login bug"
- "Compare conversion rates between mobile and web"

Claude will automatically route your question to the right tool and return results with insights.

---

## Getting Started Checklist

- [ ] Install Claude Code
- [ ] Fill out `context-library/business-info-template.md` with your product details
- [ ] Add your writing styles to `context-library/writing-style-*.md`
- [ ] **Connect your MCPs** with `/connect-mcps connect to [tool]` (Recommended!)
- [ ] Create your first PRD with `/prd-draft`
- [ ] Customize the slash commands for your workflow
- [ ] Add example PRDs from your company to `context-library/example-prds/`

## Available Slash Commands (41 Total)

Type `/` in Claude Code to see autocomplete menu with all commands.

### Core PM Workflows
- `/connect-mcps` - Connect MCPs for real-time tool integration
- `/prd-draft` - Create modern, AI-era PRDs
- `/meeting-notes` - Transform meeting transcripts into action items
- `/user-interview` - Process user interviews systematically
- `/status-update` - Generate stakeholder status updates
- `/decision-doc` - Document important product decisions
- `/slack-message` - Draft team communications
- `/competitor-analysis` - Research competitors systematically

### Strategic Frameworks
- `/strategy-sprint` - Create product strategy (1 day/week/month)
- `/prioritize` - LNO Framework task classification
- `/define-north-star` - Identify North Star Metric
- `/metrics-framework` - Leading vs lagging indicators

### Product Analysis
- `/activation-analysis` - Setup → Aha → Habit framework
- `/retention-analysis` - Cohort analysis and optimization
- `/expansion-strategy` - Revenue expansion tactics
- `/experiment-decision` - When to A/B test vs ship
- `/experiment-metrics` - STEDII framework

### Specialized & Advanced
- `/user-research-synthesis` - Research synthesis
- `/competitor-analysis` - Weekly competitor tracking
- `/meeting-cleanup` - Batch process multiple meetings
- `/prototype-feedback` - Build → review → iterate
- `/launch-checklist` - Launch readiness planning
- `/journey-map` - User/customer journey maps
- `/interview-guide` - Create JTBD interview guides
- `/interview-prep` - Pre-interview preparation
- `/interview-feedback` - Post-interview debrief
- `/generate-ai-prototype` - AI prototype prompts
- `/napkin-sketch` - ASCII wireframes
- `/prd-review-panel` - Multi-perspective PRD review
- `/create-tickets` - Create Linear/Jira tickets
- `/feature-results` - Post-launch analysis
- `/code-first-draft` - Initial feature implementation

## Common Workflows

### Draft a PRD
```
/prd-draft
```

### Process meeting notes from a customer interview
```
/meeting-notes
```

### Get multi-perspective feedback on your PRD
```
Please review [prd-file.md] from the perspective of an engineer, designer, and executive.
```

### Prioritize your weekly tasks
```
/prioritize
```

## Pro Tips

1. **Use dictation** - Talk to Claude Code like a colleague. Much faster than typing.
2. **Gossip to your copilot** - "You won't believe what just happened in my stakeholder meeting..." Keep it updated.
3. **Plan mode for complex tasks** - Use `Shift+Tab` to review Claude's plan before it executes.
4. **Multiple instances** - Run 2-3 Claude Code sessions in parallel for different initiatives.
5. **Clear context when switching** - Use `clear` to reset the conversation for a new topic.

## What Makes This Different From Regular AI Tools?

| Regular ChatGPT/Claude | Claude Code + This System |
|------------------------|---------------------------|
| Forgets your company context | Always remembers your business, team, stakeholders |
| Generic output | Matches your writing style automatically |
| One-shot responses | Evolves PRDs through 6 stages |
| No file system access | Creates/edits files directly in your project |
| Single perspective | Multi-agent reviews (engineer, designer, exec, legal) |

## Support & Updates

This is a living system. As Claude Code evolves and new PM best practices emerge, we'll update the templates and workflows.

For questions or feature requests, check the documentation in each folder.

---

**Ready to start? Fill out your business context, run `/daily-plan`, and see what an AI copilot that actually knows your company feels like.**
