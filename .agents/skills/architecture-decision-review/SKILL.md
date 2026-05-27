---
skill: architecture-decision-review
version: 1.0.0
description: >
  Evaluate a specific architectural decision in this project against its
  alternatives. Scores trade-offs across simplicity, learnability,
  maintainability, and production-readiness using a structured rubric.
  Honest about what this project sacrifices for teachability. Use when a
  student asks why a specific technology or pattern was chosen, or wants
  to understand the trade-offs behind a design decision.
triggers:
  - "why did we choose"
  - "why not use"
  - "evaluate the architecture"
  - "what are the trade-offs"
  - "why this instead of"
  - "is this how you'd do it in production"
  - "what would you change for production"
---

# Architecture Decision Review Skill

## Purpose

This project makes deliberate trade-offs — choosing teachability over production-readiness in many places. Students need to understand BOTH what was chosen and what was sacrificed. This skill evaluates specific decisions honestly, using a structured rubric so assessments are consistent and repeatable.

## Step 1: Identify the decision to review

The student will ask about a specific choice. Common questions:

- "Why psycopg instead of SQLAlchemy?"
- "Why no tests?"
- "Why single file until Module 7?"
- "Why os.environ instead of Pydantic BaseSettings?"
- "Why httpx instead of requests?"
- "Why no Docker?"
- "Why no async?"
- "Is this production-ready?"

If the question is vague ("is the architecture good?"), ask them to name one specific decision they are curious about.

## Step 2: Read the rubric and relevant code

1. Read `references/decision-rubric.md` for the scoring framework
2. Read the relevant source files to ground your evaluation in actual code
3. Read AGENTS.md Section 1 (coding doctrine) to understand why the decision was made

## Step 3: Present the decision as a structured review

For each decision, provide:

### The decision
State what was chosen, in one sentence.

### The alternatives considered
Name 1–3 realistic alternatives. For each alternative, state what it would have given and what it would have cost.

### Trade-off scoring

Score the chosen approach AND the top alternative across the four rubric dimensions (from `references/decision-rubric.md`):

| Dimension | Chosen approach | Top alternative |
|-----------|----------------|-----------------|
| Simplicity | score + reason | score + reason |
| Learnability | score + reason | score + reason |
| Maintainability | score + reason | score + reason |
| Production-readiness | score + reason | score + reason |

### The honest verdict

State clearly:
- **What this project gains** from the chosen approach
- **What this project sacrifices** — be specific
- **When the alternative would be the right choice** — name the context where you would switch (e.g., "In a team of 5 shipping a production API, SQLAlchemy's migration tooling outweighs the learning curve")

## NEVER

- Defend every decision as correct — some are deliberate compromises for teachability and the student should understand that
- Say "it depends" without specifying what it depends ON
- Dismiss the alternatives as bad — they are legitimate choices in different contexts
- Evaluate decisions against production standards when the project explicitly optimises for learning (but DO name what production would require)
- Pretend the project has no weaknesses — honest assessment of trade-offs is the skill

## Anti-rationalisation

- "The student might lose confidence in the project if I'm too critical" — No. Acknowledging trade-offs teaches mature engineering thinking. A project that claims to have no compromises is lying.
- "I'll just say 'for simplicity' and move on" — No. Explain WHAT is simpler, for WHOM, and what was given up. "For simplicity" without specifics teaches nothing.

## Exit criteria

- [ ] The chosen approach and at least one alternative are named
- [ ] All four rubric dimensions are scored for both approaches
- [ ] The verdict names what was gained AND what was sacrificed
- [ ] The student can name a context where the alternative would be better
