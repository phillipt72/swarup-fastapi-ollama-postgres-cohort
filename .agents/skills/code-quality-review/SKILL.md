---
skill: code-quality-review
version: 1.0.0
description: >
  Evaluate code against software development best practices using a structured
  rubric. Assesses separation of concerns, error handling, naming, boundary
  validation, and simplicity. Produces a scored review with specific
  file and line references. Use when a student asks to review code quality,
  evaluate their code, or understand what makes code good or bad.
triggers:
  - "review this code"
  - "how good is this code"
  - "evaluate the code quality"
  - "what would a senior developer say"
  - "code review"
  - "is this clean code"
---

# Code Quality Review Skill

## Purpose

Teach students to evaluate code the way a senior developer would — not by instinct, but against a consistent, repeatable rubric. The rubric in `references/quality-rubric.md` defines the criteria. This skill applies it.

## Step 1: Identify what to review

Ask the student what they want reviewed:

- A specific file (e.g., "review app/main.py")
- A specific function (e.g., "review the call_ollama function")
- The whole project at its current state
- A diff or change they just made

If they say "review the code" without specifics, review the file they currently have open or the most recently modified file.

## Step 2: Read the code and the rubric

1. Read the file(s) the student wants reviewed
2. Read `references/quality-rubric.md` for the evaluation criteria

## Step 3: Evaluate against each rubric dimension

For each dimension in the rubric, provide:

1. **Score** (Strong / Adequate / Needs work)
2. **Evidence** — cite the specific file and line that supports the score
3. **Why it matters** — one sentence connecting this dimension to real-world consequences

Use this format:

```
### Naming clarity — Strong
Evidence: `call_ollama` (ollama_service.py:15), `save_interaction` (interaction_service.py:10),
`fetch_recent_history` (interaction_service.py:27) — all verb-noun, all describe exactly what they do.
Why it matters: A new team member can navigate this codebase by reading function names alone.
```

## Step 4: Summarise with a verdict

After scoring all dimensions, give an overall assessment:

- What this code does well (be specific — name the strongest pattern)
- What could improve (be specific — name the file and what would change)
- One thing the student should carry to their next project

## NEVER

- Give a generic "looks good" without citing evidence
- Evaluate code against standards the project explicitly rejects (e.g., do not penalise for missing tests — AGENTS.md says "No tests in this curriculum")
- Suggest refactoring that belongs to a later module than the student is currently on
- Score every dimension as "Strong" to be encouraging — honest assessment teaches more than praise

## Anti-rationalisation

- "The student might feel bad about a low score" — No. Honest feedback with specific evidence is kind. Vague praise is not.
- "I'll skip the rubric and just give my impression" — No. The rubric exists to make reviews repeatable and teachable. Use it.

## Exit criteria

- [ ] Every rubric dimension has a score with evidence
- [ ] Evidence cites specific files and line numbers
- [ ] The summary names at least one strength and one area for improvement
- [ ] No advice contradicts AGENTS.md coding doctrine
