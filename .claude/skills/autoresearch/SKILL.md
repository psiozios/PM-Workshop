---
name: autoresearch
description: Autonomous iterative experimentation loop inspired by Karpathy's autoresearch. Use when Claude should run repeated hypothesis-test cycles for any product, strategy, research, analytics, or code problem with a measurable objective, keep improvements, discard regressions, and log evidence until stopped or a cap is reached.
disable-model-invocation: false
user-invocable: true
---

# /autoresearch - Autonomous Experiment Loops

Inspired by [Karpathy's autoresearch](https://github.com/karpathy/autoresearch), adapted for PM Workshop workflows.

The job is simple: establish a baseline, run controlled experiments, keep what improves the target metric, discard what does not, and repeat.

## Quick Start

```
/autoresearch

Goal: [what to optimize]
Success metric: [name + direction, e.g., activation rate (maximize)]
Mode: [metric-run | rubric-score | hybrid]
Scope: [what can and cannot be changed]
Cadence: [how many iterations or "run until I stop you"]
```

**Output:**
- `outputs/analyses/autoresearch-[topic]-[date].md` (research log + decisions)
- `outputs/analyses/autoresearch-[topic]-[date].tsv` (iteration table)

---

## Context Routing Logic (Internal - for Claude)

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Product strategy | `context-library/strategy/*.md` | objective, goal, pillar, OKR | Strategic constraints and success criteria |
| Product requirements | `context-library/prds/*.md`, `outputs/prds/*.md` | feature, scope, acceptance criteria | What should change vs stay fixed |
| Research context | `context-library/research/*.md`, `outputs/research-synthesis/*.md` | evidence, interview, pain point, hypothesis | Existing insights to avoid duplicate work |
| Metrics context | `context-library/metrics/*.md`, `outputs/analyses/*.md` | baseline, benchmark, funnel, retention | Existing baselines and prior experiments |
| Decisions | `context-library/decisions/*.md`, `outputs/decisions/*.md` | trade-off, chose, rejected | Historical decisions to respect |
| Meetings | `context-library/meetings/*.md`, `outputs/meeting-notes/*.md` | blocker, dependency, commitment | Operational constraints and owners |
| Existing autoresearch | `outputs/analyses/autoresearch-*.md` | same topic | Prior iterations and dead ends |
| Codebase (if code mode) | project source files | module names, benchmark/test commands | Editable files, evaluation command, failure modes |

**Context Priority:**
1. User goal, metric, constraints, and timeline
2. Existing workspace evidence and baselines
3. Live MCP data (if connected)
4. External research only for unresolved gaps

**Cross-Skill Links:**
- Problem diagnosis unclear -> `/root-cause-analysis`
- Need experiment metric quality check -> `/experiment-metrics`
- Need A/B decision framing -> `/experiment-decision`
- Code-heavy optimization -> `/explore-codebase` then `/code-first-draft`
- Competitor-driven hypothesis -> `/competitor-analysis`
- Convert winning direction into plan -> `/execution-plan`

---

## Step 0: Build the Research Charter

Before looping, define the minimum viable charter:

1. **Goal statement**
   - Example: "Improve onboarding completion for new workspace admins"
2. **Primary metric and direction**
   - Minimize (`latency`, `error rate`, `val_bpb`) or maximize (`activation`, `CTR`, `coverage`)
3. **Evaluation method**
   - Command, query, rubric, or hybrid
4. **Scope boundaries**
   - What is editable, what is read-only
5. **Stop rule**
   - Iteration cap, timebox, or explicit "until I stop you"

If any of these are missing, fill gaps before starting. No metric, no autoresearch.

---

## Step 1: Select Execution Mode

### Mode A: `metric-run` (quantitative)

Use when one command returns a numeric signal.

Examples:
- `uv run train.py` -> `val_bpb`
- `npm run build` -> bundle KB
- `pytest` -> total seconds and pass rate
- analytics query -> conversion or retention rate

### Mode B: `rubric-score` (qualitative)

Use when output quality is textual/strategic and must be judged by rubric.

Scoring pattern:
- 5 criteria, each 1-5
- weighted final score 0-100
- keep only if score improvement clears threshold

Example criteria:
- Strategic fit
- Evidence quality
- Clarity
- Decision usefulness
- Implementation feasibility

### Mode C: `hybrid`

Use when both numeric and qualitative quality matter.

Example:
- Metric: signup conversion (maximize)
- Rubric: implementation complexity + user clarity
- Keep only if metric improves and rubric does not regress materially

---

## Step 2: Setup and Guardrails

### 1) Branch and file hygiene

If experiments modify code/config/docs repeatedly:
- Create a dedicated branch: `autoresearch/[date]-[topic]`
- Keep iteration log files uncommitted until user asks otherwise

### 2) Result artifacts

Create:

`outputs/analyses/autoresearch-[topic]-[date].tsv`

Header:

```
iteration	id	score	status	description	evidence
```

Where:
- `iteration` = 0 for baseline, then 1..N
- `id` = commit hash, doc version, or run id
- `score` = main score (or normalized hybrid score)
- `status` = `keep`, `discard`, `crash`
- `description` = hypothesis tested
- `evidence` = metric line, query output, or rubric note

### 3) Thresholds and tie-breakers

Define before experiment 1:
- **Improvement threshold** (minimum delta to count as real)
- **Complexity penalty** (small gain + big complexity = discard)
- **Stability rule** (require repeat run for close calls)

### 4) Runtime guardrails

Set:
- max time per run
- max consecutive crashes before strategy reset
- iteration cap unless user explicitly requests indefinite mode

---

## Step 3: Baseline First (Iteration 0)

Always run baseline with no experimental changes.

1. Execute baseline evaluation
2. Extract baseline score
3. Log iteration `0` in TSV with status `keep`
4. Write a short baseline summary in `outputs/analyses/autoresearch-[topic]-[date].md`

Do not skip baseline. No baseline means no valid comparison.

---

## Step 4: Experiment Loop

Repeat until cap reached or user interruption.

### Per-iteration protocol

1. **Choose one hypothesis**
   - Single variable when possible
   - Explicit expected direction
2. **Apply the change**
   - Stay inside agreed scope
3. **Run evaluation with redirected output**
   - Keep logs compact and parse only needed lines
4. **Extract score and compare against current best**
5. **Decision**
   - `keep` if meaningful improvement
   - `discard` if equal/worse or improvement fails complexity test
   - `crash` if run fails and recovery is not worth another attempt
6. **Log row in TSV + short note in markdown log**
7. **Advance only from best known state**

### Decision rule

Use this logic exactly:

- If score improves beyond threshold and complexity is acceptable -> keep
- If score change is negligible and code/doc gets simpler -> keep (simplicity win)
- If score regresses or complexity cost outweighs gain -> discard
- If crash:
  - quick fix (syntax/import/path) -> retry once
  - structural failure (OOM, deadlock, invalid approach) -> log crash and move on

### Crash handling

If `grep`/parsing finds no metric:
1. inspect tail of run log
2. classify crash cause
3. decide retry vs discard quickly
4. avoid repeated retries on same idea

---

## Step 5: Synthesize and Hand Off

At stop condition, produce:

1. **Best result summary**
   - baseline vs best score
   - absolute delta and percent change
2. **What worked**
   - top 3 kept hypotheses
3. **What failed**
   - discarded/crash pattern analysis
4. **Recommended next loop**
   - 3 highest-leverage follow-up hypotheses
5. **Operational recommendation**
   - ship now, keep experimenting, or pivot strategy

Save final synthesis to:
`outputs/analyses/autoresearch-[topic]-[date].md`

---

## Output Template

```markdown
# Autoresearch Log: [Topic]

**Date:** [date]
**Mode:** [metric-run | rubric-score | hybrid]
**Goal:** [goal statement]
**Primary Metric:** [name] ([minimize/maximize])
**Scope:** [editable files/systems]
**Read-only Boundaries:** [protected areas]
**Stop Rule:** [cap/timebox/interrupt]

---

## Baseline

- Score: [value]
- Evidence: [how measured]
- Notes: [context]

---

## Iteration Summary

- Total iterations: [N]
- Keep: [N]
- Discard: [N]
- Crash: [N]
- Best score: [value]
- Improvement vs baseline: [delta, percent]

---

## Winning Changes

1. [Change + why it likely worked]
2. [Change + why it likely worked]
3. [Change + why it likely worked]

## Failed Patterns

1. [Pattern + failure reason]
2. [Pattern + failure reason]

## Recommendation

[Ship / Continue / Pivot] with rationale.

## Next 3 Hypotheses

1. [Hypothesis]
2. [Hypothesis]
3. [Hypothesis]
```

---

## Anti-Patterns to Avoid

- Running without a baseline
- Changing many variables in one iteration when not necessary
- Ignoring complexity cost for tiny metric gains
- Letting logs flood context instead of parsing targeted lines
- Repeating known failed ideas without a meaningful new angle
- Violating scope boundaries to force improvements

---

## Output Quality Self-Check

- [ ] Baseline captured before experiments
- [ ] Metric and direction explicitly defined
- [ ] Keep/discard decisions tied to evidence, not intuition
- [ ] Each experiment logged in TSV with status and rationale
- [ ] Best-known state preserved at the end
- [ ] Final synthesis saved to `outputs/analyses/`
- [ ] Natural next-step hypotheses provided
