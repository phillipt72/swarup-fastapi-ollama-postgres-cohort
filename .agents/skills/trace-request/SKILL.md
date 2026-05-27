---
skill: trace-request
version: 1.0.0
description: >
  Trace a user request through the full application stack, explaining what
  happens at each boundary: browser form, JavaScript fetch, FastAPI route,
  Pydantic validation, Ollama service call, Postgres persistence, and
  response back to the browser. Use when a student asks to understand the
  request flow, how the pieces connect, or what happens when they click Ask.
triggers:
  - "trace the request"
  - "walk me through the flow"
  - "what happens when I click Ask"
  - "how does the request travel"
  - "explain the full stack"
---

# Trace Request Skill

## Purpose

Help the student understand how a single user action (clicking "Ask") flows through every layer of the application. This builds architectural thinking — the ability to see a system as connected boundaries rather than isolated files.

## Step 1: Identify which endpoint to trace

Ask the student which request they want to trace. The application has four endpoints:

- `POST /ask` — the main flow (question → Ollama → Postgres → response)
- `GET /history` — read-only database query
- `GET /healthz` — connectivity check for Ollama and Postgres
- `GET /` — serves the HTML page

If they do not specify, default to `POST /ask` — it touches the most layers.

## Step 2: Read the relevant source files

Read the files involved in the request. For `POST /ask`, read in this order:

1. `app/templates/index.html` — the browser-side JavaScript
2. `app/schemas.py` — the Pydantic models at the boundary
3. `app/main.py` — the FastAPI route
4. `app/services/ollama_service.py` — the Ollama HTTP client
5. `app/services/interaction_service.py` — the Postgres persistence
6. `app/database.py` — the database connection

Consult `references/request-flow-map.md` for the detailed boundary map.

## Step 3: Walk through each boundary

Explain the request at each boundary, in order. For each boundary:

1. **Name the boundary** (e.g., "Browser → FastAPI")
2. **Name the file and function** (e.g., "`app/main.py`, line 29, `ask()` function")
3. **Explain what happens** — what data arrives, what processing occurs, what leaves
4. **Name the data format at each side** (e.g., "JSON string becomes a Pydantic `AskRequest` object")

Use the format from `references/request-flow-map.md` to structure the explanation.

## Step 4: Highlight the key insight

After the walkthrough, highlight the architectural principle:

> Each boundary transforms data from one format to another. The route does not know how Ollama works. The Ollama service does not know about Postgres. Separation of concerns means each piece only knows about its immediate neighbours.

If the student is in Module 7 or later, connect this to the refactor: "This separation only became visible after Module 7. Before the refactor, all of this lived in a single `main.py`."

## NEVER

- Skip boundaries or summarise multiple steps into one — the whole point is seeing each handoff
- Show code snippets without naming the file and line number
- Explain what "should" happen — read the actual source files and explain what DOES happen

## Exit criteria

- [ ] Every boundary in the request path has been named and explained
- [ ] Each explanation references a specific file and line number
- [ ] Data format transformations are named at each boundary
- [ ] The student can articulate the flow in their own words (ask them)
