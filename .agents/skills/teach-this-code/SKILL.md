---
skill: teach-this-code
version: 1.0.0
description: >
  Teaches a student what a specific file or function does, concept by concept.
  Reads the file, breaks it into meaningful chunks, explains each chunk in
  plain language, names the programming principle behind it, and checks
  understanding with a prediction question. Use when a student points at
  a file or function and asks to be taught what it does.
triggers:
  - "teach me"
  - "teach this code"
  - "explain this file"
  - "walk me through this file"
  - "what does this file do"
  - "help me understand"
  - "break this down for me"
---

# Teach This Code Skill

## Purpose

A student points at a file and says "teach me main.py." Your job is to sit beside them like a patient tutor and explain what the code does — not line by line (that is tedious and unhelpful), but concept by concept. Each concept gets a plain-language explanation, a named programming principle, and a prediction question that tests whether the student actually understood.

This skill composes with the `cite-the-code` rule. Every explanation you give will automatically reference specific files and line numbers. You do not need to remind yourself to do this — the rule handles it.

## Step 1: Identify the file or function

The student will say something like:

- "teach me main.py"
- "teach me the call_ollama function"
- "teach me app/services/interaction_service.py"
- "teach me schemas.py"

If they give a filename without a path, find it. Common locations:

- `app/main.py`
- `app/database.py`
- `app/schemas.py`
- `app/services/ollama_service.py`
- `app/services/interaction_service.py`
- `app/templates/index.html`

If they point at a module folder (e.g., "teach me module 5"), use the files inside `dist/module_05_*/app/`.

If the file does not exist, say so and list the files that do exist.

## Step 2: Read the file

Read the entire file. Also read `references/code-patterns-glossary.md` — you will need it to name patterns accurately.

## Step 3: Break into concepts

Do NOT explain line by line. Instead, identify the meaningful concepts in the file. A concept is a coherent unit of functionality — usually 3–15 lines that work together to accomplish one thing.

For example, in `app/main.py`:

| Concept | Lines | What it does |
|---------|-------|-------------|
| Environment bootstrap | 1–3 | Loads .env before any imports read os.environ |
| Import organisation | 5–15 | Groups stdlib, framework, local imports |
| App initialisation | 17–20 | Creates FastAPI app, mounts static files, sets up templates |
| Index route | 23–25 | Serves the HTML page |
| Ask endpoint | 28–35 | Accepts a question, calls Ollama, saves to Postgres, returns response |
| History endpoint | 38–40 | Returns recent interactions |
| Health check | 43–59 | Tests connectivity to Ollama and Postgres |

Aim for 4–8 concepts per file. If a file has more, it may be too complex — note this as an observation.

## Step 4: Teach each concept

For each concept, provide three things in this order:

### A. Plain-language explanation

Explain what the code does as if talking to someone who can read Python but has not seen this codebase before. Use short sentences. Avoid jargon unless you immediately define it.

**Good:** "Line 1 imports `load_dotenv` and line 3 calls it immediately — before any other imports. This reads your `.env` file and puts those values into `os.environ`. The order matters: `database.py` reads `DATABASE_URL` from `os.environ` at import time (line 5 of database.py), so if `load_dotenv()` has not run yet, that import will crash with a `KeyError`."

**Bad:** "This loads environment variables from the .env file using the dotenv package."

The bad version tells the student WHAT but not WHY, and does not connect it to the consequence of getting it wrong.

### B. The pattern name

Name the programming principle or pattern at work. Use the glossary in `references/code-patterns-glossary.md`. Keep it to one sentence.

**Example:** "This is the **eager environment loading** pattern — configure the runtime before importing modules that depend on configuration."

### C. Prediction question

Ask the student to predict what would happen if something changed. This tests whether they understood the concept or just read your explanation.

**Example:** "What would happen if you moved `load_dotenv()` to line 10, after the other imports? Try to predict the error before you test it."

Wait for their answer. If correct, confirm and move to the next concept. If incorrect, explain what would actually happen and why.

## Step 5: Summarise with a mental model

After all concepts are taught, give the student a one-paragraph summary that connects the concepts into a mental model of the file:

**Example for main.py:** "This file is the application's front door. It boots the environment, creates the web server, and defines four endpoints — each one a thin coordinator that delegates to specialised services. The routes do not contain business logic themselves; they call `call_ollama()`, `save_interaction()`, and `fetch_recent_history()`, which live in the services/ directory. If you need to change how Ollama is called, you never touch this file — you go to `ollama_service.py` instead."

## Step 6: Offer to go deeper

Ask: "Want me to teach you any of the files this one depends on?" and list them.

For `main.py`, that would be: `database.py`, `schemas.py`, `ollama_service.py`, `interaction_service.py`.

This lets the student follow the dependency chain at their own pace.

## NEVER

- Explain line by line — group into concepts. A student who reads "line 1 does X, line 2 does Y, line 3 does Z" learns nothing about how the pieces relate.
- Skip the prediction question — it is the only way to verify understanding. An explanation without a check is a lecture, not teaching.
- Use jargon without defining it the first time — "context manager," "decorator," "serialisation" all need a one-sentence plain-language definition on first use.
- Explain code that is not in the file — if the student needs to understand `call_ollama()` to understand `main.py`, tell them what it does in one sentence and offer to teach that file next. Do not inline a full explanation of another file.
- Assume the student understands imports — many mid-career professionals in this cohort are not software engineers. Explain what `from app.database import get_conn` means the first time it appears.

## Anti-rationalisation

- "The student probably knows what a decorator is" — No. Define it on first use. If they already know, they will skim past it. If they do not know, you have saved them from pretending.
- "I'll skip the prediction question to save time" — No. The prediction question is the learning. The explanation is just the setup.
- "This file is simple, I'll just give a quick overview" — No. Simple files are where students build the mental models they will need for complex files. Teach them thoroughly.

## Exit criteria

- [ ] The file was broken into 4–8 meaningful concepts (not line-by-line)
- [ ] Each concept has a plain-language explanation, a named pattern, and a prediction question
- [ ] The student answered at least 2 prediction questions
- [ ] A summary mental model was provided connecting all concepts
- [ ] The student was offered the chance to follow dependency chains to related files
