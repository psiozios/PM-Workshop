---
name: interview-guide
description: Create JTBD-based interview guides for user research. Structured questions for discovery interviews.
disable-model-invocation: false
user-invocable: true
---

# /interview-guide - Create Interview Guides

Generate structured interview guides using Jobs-to-be-Done methodology.

## Quick Start

```
/interview-guide

Planning user research interviews? I'll create a JTBD-based interview guide.

Tell me:
1. What topic are we exploring? (e.g., "why users churn after trial")
2. Who are we interviewing? (role, context, segment)
3. What hypothesis are we testing? (optional but helpful)

I'll check your existing research first, then generate a guide that
focuses on gaps -- not re-asking what you already know.

Output: outputs/interview-guides-[topic]-[date].md
```

---

## Context Routing Logic (Internal - for Claude)

**Automatic Context Checks:**
When this skill is invoked, immediately check:

| Source | Files/Folders | Search Terms | What to Extract |
|--------|---------------|--------------|-----------------|
| Research Plan | `context-library/research/*.md` | hypothesis, problem statement, target users | Current understanding of problem |
| User Personas | `context-library/research/personas*.md` or stakeholder template | target segment, role, company size | Who to interview and their context |
| PRDs | `context-library/prds/*.md` | problem statement, user pain | Problem framing for interview hypothesis |
| Strategy | `context-library/strategy/*.md` | user segment, JTBD canvas | Jobs framework and strategic fit |
| Previous Research | `context-library/research/interviews*.md` | similar problem area, past themes | Avoid revalidating, build on insights |

**Context Priority:**
1. Current PRD and problem statement FIRST
2. Target user personas and segments SECOND
3. Previous research on related topics THIRD
4. Strategic context FOURTH

**Cross-Skill Links:**
- After interview → Link to `/user-interview` for processing transcripts
- After interview → Link to `/user-research-synthesis` for deeper synthesis
- If extracting insights → Link to `/user-research-synthesis` for analysis
- If developing strategy from findings → Link to `/write-prod-strategy`
- Note: `/interview-prep` is for PM job interviews, not user research

---

## Step 0: Understanding Your Research Objective

Before creating the guide, let me understand what you're trying to learn...

**Checking:**
- `context-library/prds/` for the feature or problem you're researching
- `context-library/research/` for previous findings on this area
- `context-library/strategy/` for strategic context
- Stakeholder profiles for interview target information

**Based on what I find, I'll show you:**

### What We Know About This Research

**Research Topic:**
- [Feature or problem you're exploring]
- [Current hypothesis: what do you think you'll find?]
- [Strategic fit: why does this research matter?]

**Target User:**
- [Role and company size from stakeholder profiles]
- [Current tools they use for this job]
- [Past interviews with similar users: what did they tell you?]

**Research Goal:**
- [Specific question you're trying to answer]
- [Assumptions you're validating]
- [What would surprise you?]

### PM-Specific Diagnosis Questions

1. **Hypothesis Confidence:** How sure are you about the problem? (Testing assumption vs validating)
2. **User Segment:** Are you talking to current users, power users, or non-users?
3. **Scope:** Is this a deep dive on one problem or broad discovery?
4. **Previous Research:** What did you learn in past interviews on this topic?
5. **Time Available:** How many interviews can you do? (Affects research scope)

---

## Step 0.5: Integrate Past Research

**Before creating the guide, check `context-library/research/` for existing interview syntheses and findings.**

Search for:
- Previous interview synthesis files (`*interview*`, `*research*`, `*synthesis*`)
- Findings related to this topic area
- Quote banks or insight repositories

**Categorize what you find into three buckets:**

```
## Research Foundation for This Interview Guide

**Based on [N] previous interviews/syntheses found in context-library/research/:**

### Well-Validated Themes (don't over-index here)
These themes have strong evidence from prior research. Include 1-2 confirmation
questions but don't spend 10 minutes re-exploring what you already know.
- [Theme 1]: Supported by [N] interviews. Evidence: "[key quote]"
- [Theme 2]: Supported by [N] interviews. Evidence: "[key quote]"

### Themes Needing More Evidence (probe deeper here)
These came up in prior research but need more data points or deeper understanding.
Allocate extra time in the guide for these areas.
- [Theme 3]: Mentioned by [N] users but unclear why. Probe: [specific angle]
- [Theme 4]: Conflicting signals from past research. Probe: [what to clarify]

### Unexplored Areas (add discovery questions)
These areas haven't been covered in prior research and represent blind spots.
Add open-ended discovery questions to surface new insights.
- [Area 1]: No prior data. Suggested opening: "[question]"
- [Area 2]: Adjacent to known themes but never directly explored.
```

**Add a note at the top of the generated guide:**
> "Based on [N] previous interviews, we already know [validated themes summary]. This guide focuses on deepening [weak evidence areas] and exploring [new areas]. Avoid spending excessive time re-validating established findings."

If no previous research is found, note: "No existing research found in context-library/research/. This is a discovery interview -- all questions are exploratory."

---

## When to Use

- Planning user research interviews
- Discovery for new features
- Understanding customer needs
- Validating problem hypotheses

---

## JTBD Interview Framework

Focus on understanding:
1. **Use Case** - What situation triggers the need?
2. **Alternatives** - What do they use today?
3. **Progress** - What does success look like?
4. **Value** - What's the impact of solving this?
5. **Price** - What would they pay/trade for a solution?

---

## Curated Question Bank (63 Questions)

*Source: Validated question library organized by phase and category. Select questions that fit your research goal — don't use all of them. A 45-60 min interview typically uses 10-15 questions.*

**Selection logic:**
- Use 3-5 Introduction questions to warm up (pick based on interviewer comfort and participant context)
- Use 8-12 Interview questions for core insight extraction (prioritize JTBD and Storytelling categories)
- Always use 3-4 Conclusion questions to close every session

---

### Phase 1: Introduction Questions

**Purpose:** Build rapport, set context, move from small talk to substance.

**General (role & daily life)**
- "Could you tell me a little about your role and what a typical day looks like?" *(open-ended, allows them to start with familiar territory)*
- "If you had to explain your job to a child, how would you describe it?" *(fun, disarming ice-breaker)*
- "Outside of work, what are your interests or hobbies?" *(personal, non-work — relaxes the atmosphere)*

**General (expertise & career)**
- "What initially interested you in [their area of expertise]?" *(shows genuine interest in their background)*
- "Is there a particular project or achievement in your career that you're especially proud of?" *(lets them share a success, sets positive tone)*
- "It's such a fascinating field. How does it feel to [do something meaningful they do]?" *(connects on a human level)*
- "How have things changed in your role or industry since you started?" *(acknowledges their expertise, invites insights)*
- "I've always been interested in [something relevant] because [reason]. How did you manage to break into [specific field]?" *(demonstrates genuine curiosity)*

**General (topic transition)**
- "Before we dive into specifics, I'd love to hear more about your experience with [relevant topic or product]." *(allows them to share general feelings before detailed questions)*
- "If you had a magic wand, what one thing would you change about your current tools or processes?" *(general, easy, surprising answers possible)*

**Homework-based (use when you've done research on them)**
- "I noticed [something relevant about their company or role from your research] — could you tell me more about that?" *(demonstrates commitment and respect for their time)*

---

### Phase 2: Interview Questions

**Purpose:** Extract stories, understand jobs-to-be-done, find root causes. This is the core of the interview.

---

**JTBD (Jobs-to-be-Done) — use for product-market fit and outcome research**
- "When [job step], how important is it for you to achieve [desired outcome]?" *(JTBD importance — pair with satisfaction question)*
- "When [job step], how satisfied are you with your ability to achieve [desired outcome]?" *(JTBD satisfaction — pair with importance question)*

---

**Storytelling: Start a Specific Story** *(always prefer past behavior over opinions)*
- "Tell me about a specific time when [something happened with the product or problem]."
- "Can you describe a moment when our product didn't meet your expectations?" *(starts a concrete story)*
- "Have you used our product in an unconventional or creative way? Can you share that experience?"
- "What was the biggest challenge you faced when learning to use our product? Tell me about that."
- "Have you ever felt frustrated while using our product? Can you tell me about that experience?"
- "Can you walk me through the last time you encountered a problem with [specific task or process]?"
- "Can you recall a recent situation where you felt our product significantly saved your time or effort?"
- "Tell me about the time when our product impacted the relationships or interactions you have with your customers."

**Storytelling: Story Detail Probes** *(use AFTER a story starts to go deeper)*
- "Where were you?"
- "What time of day was it?"
- "Did someone help you?"
- "What happened next?"
- "What was the conversation like when you talked about [topic of the conversation]?"
- "What triggered you to think about [action/product]?"
- "How much time did you spend trying to solve this?" *(if they didn't spend much time, it's probably not a real problem)*
- "What tools or methods did you use?" *(surfaces alternatives and indirect competitors)*
- "Have you tried any workarounds? Can you describe them?" *(workarounds reveal unsolved problems)*

---

**Root Cause Analysis** *(dig into why something matters)*
- "You told me [paraphrase]. Is that correct? Could you help me understand it better? Why is it a problem for you?"
- "What I'm hearing is [paraphrase]. What exactly were the consequences?"
- "How do you feel about [paraphrase]? How exactly did it influence your work, plans, or team?" *(probes importance and emotional weight)*

---

**Feature Requests** *(careful — these are opinions, not behaviors)*
- "Why is it a problem for you? What were the consequences the last time?" *(root-cause the request before taking it at face value)*
- "What would that do for you if you had that feature?" *(understand the underlying job, not just the surface request)*

---

**New Customer Journey** *(use for new user or acquisition research)*
- "What other solutions did you try? Why or why not?"
- "What did you imagine using this product or service would be like?" *(surfaces expectations vs. reality)*
- "What other solutions did you consider before trying the product?"
- "If this product wasn't available to you, what would you do instead?"
- "When did you first realize you needed something to solve your problem? Tell me about that situation."
- "Did you have any anxiety about the purchase?"
- "Did you talk to anyone else about what choices you were considering? Tell me about that situation."
- "When choosing a tool or solution for [specific task], what factors are most important to you?"

---

**General Interview Questions** *(opinion-based — flag that these aren't pure JTBD but can be useful context)*
- "What's something you're trying to achieve in your role that our product doesn't currently support?"
- "When recommending our product to someone, what would you say? Better: Did you actually recommend it? Tell me about that."
- "Is there anything you feel is missing in the market that could help you with [specific task or problem]?"
- "How do you measure success in your role, and how does [specific task or process] contribute to that?"
- "Are there any external factors that influence how you use our product (e.g., industry trends, organizational changes)?"
- "Is there a particular aspect of the product you find confusing or overly complex?"
- "What's your ideal outcome for [specific task or challenge]?"
- "Are there any trends or changes in your industry that are influencing how you approach [specific task or process]?"
- "How do you stay informed or up-to-date in your field?"

*Note on opinion questions: These can be useful but be cautious. People often say what they think you want to hear or what sounds good, not what they'd actually do. Always try to anchor opinion questions to specific past behavior ("Tell me about the last time…").*

---

### Phase 3: Conclusion Questions

**Purpose:** Surface final insights, ensure completeness, recruit next participants.

**Insights** *(something may have shifted during the interview)*
- "Reflecting on our conversation, what do you think is the most important aspect we should focus on?"
- "Can you think of any additional resources or support that would enhance your experience with [product]?"
- "How would you summarize your overall experience with [product] in a few sentences?"
- "Do you have any final thoughts or comments about the topics we discussed today?"

**Important Details** *(ensure nothing critical was missed)*
- "Were there any questions you expected me to ask that I didn't?"
- "Is there anything we haven't covered that you think is important for us to know about your experience with [product]?"

**Recruiting** *(keep the research momentum going)*
- "Would you be open to follow-up conversations if we have more questions in the future?"
- "Do you know of anyone else who might have valuable insights to share? We'd love an introduction."

**Interview Feedback** *(helps improve the process)*
- "Is there anything you found particularly enjoyable or frustrating about this interview? What could we improve?"
- "Based on our conversation, what should be our immediate next steps?" *(surfaces what they think you should act on)*

---

## Quick Start Prompt

When PM types `/interview-guide`, respond:

```
Let's create an interview guide. I'll use the JTBD framework.

Tell me:
1. What topic are we exploring?
2. Who are we interviewing? (role, context)
3. What hypothesis are we testing? (optional)

I'll generate a structured guide with opening, core, and closing questions.
```

---

## Interview Guide Structure

### Part 1: Opening (5 min)
Build rapport, set context

### Part 2: Background (10 min)
Understand their world

### Part 3: Core JTBD Questions (25 min)
Dig into the job to be done

### Part 4: Solution Exploration (10 min)
Test ideas without leading

### Part 5: Closing (5 min)
Wrap up, ask for referrals

---

## Output Template

```markdown
# Interview Guide: [Topic]

**Research Goal:** [What we want to learn]
**Target Participant:** [Who we're interviewing]
**Duration:** 45-60 minutes

---

## Part 1: Opening (5 min)

"Thanks for joining. I'm [name], a PM at [company]. We're researching [topic] to better understand [goal]. There are no right or wrong answers—I'm here to learn from your experience."

**Logistics:**
- "Is it okay if I record this for notes?"
- "Everything you share is confidential"
- "Feel free to skip any question"

---

## Part 2: Background (10 min)

1. "Tell me about your role and what you do day-to-day."

2. "How does [topic area] fit into your work?"

3. "Walk me through a typical [relevant workflow]."

---

## Part 3: Core JTBD Questions (25 min)

### Situation & Trigger
4. "Think about the last time you needed to [job]. What was happening?"

5. "What triggered that need? What were you trying to accomplish?"

### Current Solution
6. "How do you handle [job] today?"

7. "What tools or processes do you use?"

8. "What works well about your current approach?"

9. "What's frustrating or difficult about it?"

### Progress & Success
10. "When [job] goes really well, what does that look like?"

11. "How do you know when you've succeeded?"

### Impact & Value
12. "What happens when [job] goes poorly?"

13. "How does that affect you / your team / your business?"

14. "If you could wave a magic wand, what would be different?"

---

## Part 4: Solution Exploration (10 min)

**Show concept/prototype if applicable**

15. "Looking at this [concept], what stands out to you?"

16. "How would this fit into your current workflow?"

17. "What concerns would you have about using something like this?"

18. "What would make this more valuable to you?"

---

## Part 5: Closing (5 min)

19. "Is there anything else about [topic] that I should have asked?"

20. "Who else should I talk to about this?"

21. "Would you be open to a follow-up conversation?"

**Thank them and explain next steps.**

---

## Notes Template

| Question | Response | Key Quote | Insight |
|----------|----------|-----------|---------|
| Q1 | | | |
| Q2 | | | |

---

## Post-Interview
- [ ] Send thank you email
- [ ] Transcribe key quotes
- [ ] Tag themes
- [ ] Add to research repository
```

---

## Question Types to Use

**Open-ended:** "Tell me about..." / "Walk me through..."
**Follow-up:** "Why was that?" / "Can you give an example?"
**Clarifying:** "When you say X, what do you mean?"
**Emotional:** "How did that make you feel?"

## Questions to Avoid

**Leading:** "Don't you think X is annoying?"
**Binary:** "Do you like X?" (yes/no)
**Hypothetical:** "Would you use X if we built it?"
**Multiple:** "What about X and Y and Z?"

---

## Output Integration

### Where Files Go

**Interview guides:**
- Active work: `outputs/interview-guides-[topic]-[date].md`
- When finalized: Archive to `context-library/research/interview-guides/` for team reference
- Use directly: Share with interviewing team before sessions

### Link to Other Work

After creating the guide:
- **Share with team** - Use as standard guide for all interviews on this topic
- **Synthesize findings** - After interviews, use `/user-research-synthesis`
- **Update PRD** - If research validates hypothesis, update in `/prd-draft`
- **Inform strategy** - If research reveals new insights, feed to `/write-prod-strategy`

### Cross-Skill Integration

**Feeds into:**
- `/user-research-synthesis` - After interviews, synthesize the raw notes
- `/prd-draft` - Research findings go into Hypothesis section
- `/write-prod-strategy` - Deep user research informs strategic decisions

**Pulls from:**
- `context-library/research/` - Previous research on this topic
- `context-library/strategy/` - Strategic context about this user problem
- Stakeholder profiles - Information about target user segment

---

## Output Quality Self-Check

Before delivering the interview guide, verify:

| Check | Criteria | Pass? |
|-------|----------|-------|
| **Past research checked** | Searched `context-library/research/` and categorized findings into validated/needs-evidence/unexplored | [ ] |
| **Guide reflects prior knowledge** | Questions focus on gaps, not re-validating what's already known | [ ] |
| **Research foundation note included** | Top of guide states what's known and where this guide focuses | [ ] |
| **No leading questions** | Every question is open-ended, not suggesting a desired answer | [ ] |
| **JTBD framework applied** | Questions cover situation/trigger, current solution, progress/success, impact/value | [ ] |
| **Time-boxed sections** | Each section has a suggested time allocation that totals 45-60 min | [ ] |
| **Follow-up prompts included** | At least 2-3 follow-up probes per core question | [ ] |
| **Closing includes referrals** | Guide asks "Who else should I talk to?" | [ ] |
| **Output file saved** | Guide saved to `outputs/interview-guides-[topic]-[date].md` | [ ] |
| **Question bank used** | Questions drawn from the curated 63-question library where applicable, not improvised | [ ] |
| **Question count is right** | 10-15 questions total for a 45-60 min interview — not more, not fewer | [ ] |

**If any check fails, address it before delivering the output.**

---

## Tips

- **Listen more than talk** - 80/20 rule
- **Follow the energy** - If they light up, dig deeper
- **Capture exact quotes** - Their words > your paraphrase
- **Silence is okay** - Let them think
- **Avoid leading questions** - Don't suggest the answer you want
- **Reference past behavior** - "Last time you X, what happened?" beats "Would you use X?"

---

## Mode: --recruit (PM Candidate Interview Guide)

Use `/interview-guide --recruit` to build a structured guide for interviewing PM candidates. This is for hiring, not user research.

```
/interview-guide --recruit

Tell me:
1. PM role and level (IC PM, Senior PM, Group PM, etc.)
2. Top 2-3 competencies you're hiring for
3. Any specific signals you've seen in resumes or screens that you want to probe

Output: outputs/interview-guides/pm-interview-[level]-[date].md
```

### PM Candidate Interview Structure (60 min)

```
## Opening (5 min)
"Walk me through your background — specifically how you got into product and what problems you've loved working on."
Listen for: Genuine curiosity, user empathy, ability to tell a clear story

## Product Sense (15-20 min)
"Tell me about a product you use every day that you think is underrated. What makes it good, and what would you change?"
Listen for: Depth of thinking, user focus vs. feature focus, quality of tradeoff reasoning

Follow-up: "How would you decide what to build next on that product?"
Listen for: Structured thinking, willingness to use data AND intuition

## Execution (15-20 min)
"Tell me about a project where you had to manage significant ambiguity or change. Walk me through how you handled it."
Listen for: Resilience, clear ownership, how they involve the team

Follow-up: "What would you have done differently?"
Listen for: Self-awareness and learning orientation

## Stakeholder / Influence (10 min)
"Tell me about a time you had to get alignment from stakeholders who had competing priorities. How did you approach it?"
Listen for: Political savvy, negotiation, influence without authority

## Curiosity + Culture (5-10 min)
"What's a PM skill or area you're actively trying to develop?"
"What do you look for in a team you want to join?"
Listen for: Self-awareness, growth mindset, whether they'd thrive here

## Their Questions (5-10 min)
Good candidates ask about: team structure and culture, how decisions get made, what success looks like in 90 days, the hardest part of the role
```

### Candidate Evaluation Rubric

| Competency | Signal looked for | Score (1-5) |
|------------|------------------|-------------|
| Product sense | Quality of product critique, user empathy, tradeoff reasoning | |
| Execution | Scope management, dealing with ambiguity, follow-through | |
| Communication | Clarity, structure, listening skills | |
| Stakeholder influence | Cross-functional examples, political savvy | |
| Learning orientation | Self-awareness, what they'd do differently | |
| Culture fit | Curiosity, team orientation, values alignment | |
