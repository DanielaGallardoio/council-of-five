---
name: council-of-five
description: "Spawn 5 Opus subagents with randomly-generated distinct personas to debate a problem from multiple angles, then synthesize their perspectives into a comparison and recommendation. Use this skill whenever the user wants diverse perspectives on a decision, mentions 'council', 'debate this', 'multiple perspectives', 'devil's advocate', 'pros and cons from different angles', naming decisions, positioning debates, UX trade-offs, architecture choices, or any situation where there's no single right answer and outside-the-box thinking would help. Also trigger when the user says 'council of five', 'consejo', 'que lo ataque el consejo', or wants to stress-test an idea."
---

# Council of Five

Spin up 5 parallel Opus agents, each with a **randomly generated** distinct persona, to explore a problem from radically different angles. The weirder the persona, the more unexpected the insight.

## When to Use

- UX/design decisions with no obvious "right answer"
- Architecture trade-offs (speed vs. maintainability, etc.)
- Naming, branding, positioning, or messaging decisions
- API design, workflow design, content strategy
- Any decision where groupthink is the enemy

## How It Works

1. **Generate**: Randomly select 5 personas from different archetype categories (or invent new ones)
2. **Announce**: Present the 5 personas and their one-sentence philosophies to the user in a table
3. **Launch**: Spawn 5 Opus subagents in parallel using the Agent tool (all in a single message so they run concurrently)
4. **Explore**: Each agent argues from their persona's angle, proposing concrete alternatives
5. **Synthesize**: Gather all results into a comparison table, identify agreements and clashes, suggest hybrids
6. **Decide**: Ask the user "What resonates?" and proceed based on their choice

## Random Persona Generation

Each invocation generates 5 fresh personas. The key rules:

- **Pick 5 from different categories** — no two from the same category
- **Invent new ones freely** — the pool below is a starting point, not a limit
- **Name them vividly** — "The Haiku Master" not "The Brevity Person"
- **Give each a one-sentence philosophy** — this grounds their argument and makes it memorable
- **Tailor to the problem domain** — if the debate is about branding, pick personas relevant to branding (the skeptical customer, the copywriter, the competitor's strategist) rather than generic archetypes

### Archetype Pool (starting point — invent freely beyond this)

| Category | Example Personas |
|---|---|
| Reduction | The Minimalist, The Deletionist, The "YAGNI" Zealot, The Haiku Master |
| Narrative | The Storyteller, The Novelist, The Stand-up Comic, The Documentary Filmmaker |
| Visual | The Dashboard Engineer, The Infographic Designer, The Color Theorist, The Whitespace Monk |
| Verification | The Paranoid Auditor, The Penetration Tester, The QA Gremlin, The "Trust No One" Agent |
| Behavior | The UX Researcher, The Cognitive Psychologist, The Lazy User Simulator, The Angry Customer |
| Performance | The Latency Hunter, The Memory Miser, The Big-O Obsessive, The Cache Whisperer |
| Accessibility | The Screen Reader Advocate, The Color Blind Designer, The Keyboard-Only Navigator |
| Philosophy | The Unix Philosopher, The Functional Purist, The "Worse is Better" Advocate, The Pragmatist |
| Chaos | The Edge Case Finder, The Chaos Monkey, The "What If" Catastrophist, The Entropy Embracer |
| History | The Legacy Code Archaeologist, The "We Tried That" Historian, The Pattern Recognizer |
| Future | The 10x Scale Predictor, The Deprecation Prophet, The "Your Future Self" Advocate |
| Outsider | The New Hire, The Non-Technical Stakeholder, The Customer Support Rep, The Intern |
| Wild Cards | The Time Traveler from 2035, The Toddler ("But WHY?"), The Poet Laureate, The Lawyer, The Sleep-Deprived On-Call Engineer, The Competitor'� Product Manager |

When the problem domain suggests it, invent personas that don't exist in the pool. For a branding debate you might create "La Diseñadora Quemada" or "La Gen Z Escéptica." For an API design debate, "The Developer Who Only Reads the First Paragraph of Docs." Creativity is the point.

## Prompt Template for Each Agent

When spawning each subagent, use this structure:

```
You are [PERSONA NAME]. Your core philosophy: "[one-sentence worldview]."

[CONTEXT ABOUT THE PROBLEM — include all relevant background the user has provided]

THE PROBLEM TO DEBATE: [specific question or decision]

Argue from your persona's perspective. Be opinionated, specific, and provocative.
Propose a concrete alternative or approach — not just criticism.
Max [WORD_LIMIT] words. Respond in [LANGUAGE matching user's language].
```

Important implementation details:
- Set `model: "opus"` for each agent to get the strongest reasoning
- Send ALL 5 Agent tool calls in a **single message** so they run concurrently
- Include rich context in each prompt — the agents have no memory of the conversation, so they need the full picture
- Set a word limit (200-400 words works well) to keep arguments focused
- Match the user's language in the agent prompts

## Output Format

After all 5 agents return, synthesize into:

### 1. Comparison Table

| Persona | Core Argument | Proposed Approach |
|---|---|---|
| [Name] | [One-line thesis] | [Concrete proposal] |

### 2. Where They Agree
Identify the points of convergence — these are usually the strongest signals.

### 3. Where They Clash
Highlight the fundamental tensions. These reveal the real trade-offs in the decision.

### 4. Hybrid Suggestion (if applicable)
Can elements from multiple proposals combine into something better?

### 5. Question to User
Always end with: "What resonates?" or a more specific question based on the debate.

## Controlling the Council

**Full random** (default):
```
Council of five on [problem]
```

**Seeded** — guarantee one specific angle:
```
Council of five on [problem], but make sure one persona is [angle]-focused
```

**Category bias** — weight toward certain domains:
```
Council of five on [problem], bias toward Behavior and Accessibility personas
```

**Reroll** — if the personas don't fit:
```
Those personas didn't fit — reroll with completely different ones
```

**Attack mode** — have the council critique an existing decision:
```
I chose [decision]. Have the council attack it.
```

In attack mode, all 5 personas argue AGAINST the decision, looking for flaws, risks, and blind spots. This is useful for stress-testing before committing.

## Multi-Round Debates

The council can run multiple rounds. After the first synthesis:

- The user picks a direction or names a winner
- A second round can be spawned where all 5 personas attack the chosen direction
- Or: keep 2-3 personas and replace the rest with new ones for fresh perspectives

This iterative approach prevents premature convergence and surfaces issues that only emerge when you commit to a direction.

## Tips for Best Results

- **Be specific about what you care about** — speed? clarity? memorability? cost? The personas argue better with clear evaluation criteria
- **Provide the current approach** — personas can critique something concrete much better than theorize in a vacuum
- **Include real user/customer quotes** — if you have research data, include it in the context so personas can reference real voices
- **Ask for hybrids** — if multiple approaches resonate, a follow-up round combining elements often produces the best result
- **Match the stakes to the method** — council of five is heavy artillery, best for decisions that matter. For quick choices, just ask for pros and cons directly
