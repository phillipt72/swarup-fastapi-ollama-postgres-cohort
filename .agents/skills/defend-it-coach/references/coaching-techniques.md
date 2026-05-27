# Coaching Techniques for Defend It Questions

Use these techniques when helping students reason through design decisions.

---

## Technique 1: The Counterfactual

Present the opposite of the current design and ask what would happen.

**When to use:** The student cannot articulate why a decision was made.

**Pattern:**
> "What if we did the opposite — instead of [current approach], we used [alternative]. What would happen to [specific concern]?"

**Example:**
- Student asks: "Why do we use psycopg instead of SQLAlchemy?"
- Coach: "What if we used SQLAlchemy here — what extra concepts would a Module 5 student need to learn before they could write their first database query?"

---

## Technique 2: The Consequence Chain

Walk forward in time from a hypothetical change.

**When to use:** The student's reasoning is partially correct but they have not followed the implications far enough.

**Pattern:**
> "OK, so if we [their suggestion], then what happens when [next situation]? And after that, when [further situation]?"

**Example:**
- Student says: "We should catch all exceptions so the app never crashes."
- Coach: "If we catch all exceptions, what happens when Postgres is misconfigured? The INSERT silently fails. Now the user thinks their question was saved, but it wasn't. They come back tomorrow and their history is empty. Which is worse — a crash that tells you what broke, or silent data loss?"

---

## Technique 3: The Smaller Question

Break a big "why" into a concrete, observable "what."

**When to use:** The student says "I don't know" or gives a vague answer.

**Pattern:**
> "Let's start smaller. Open [file] and look at line [N]. What does [specific thing] actually do?"

**Example:**
- Student says: "I don't know why we validate at the boundary."
- Coach: "Open `app/schemas.py` and look at `AskRequest`. What happens if you send a POST to `/ask` with an empty JSON body — no `question` field at all? Try it with curl and tell me what you get."

---

## Technique 4: The Trade-off Frame

Present two valid approaches and ask the student to weigh them.

**When to use:** The student thinks there is one "right answer" but the decision involves trade-offs.

**Pattern:**
> "There are actually two reasonable approaches here: [A] and [B]. [A] gives you [benefit] but costs you [drawback]. [B] gives you [other benefit] but costs you [other drawback]. Which matters more for this project, and why?"

**Example:**
- Student asks: "Why not add type hints everywhere?"
- Coach: "Type hints at function signatures catch bugs early — that's [A]. But type hints on every local variable inside a 5-line function add visual noise without catching anything new — that's the cost. This project puts types at boundaries (function signatures, Pydantic models) but not on obvious locals. Which matters more for a teaching codebase: catching every possible type error, or keeping the code readable enough that a beginner can follow it?"

---

## Technique 5: The Confirmation

Validate strong reasoning with specificity.

**When to use:** The student's reasoning is correct.

**Pattern:**
> "That's exactly right. The key insight is [restate their strongest point in slightly more precise language]. In industry, this principle shows up as [broader concept]."

**Example:**
- Student says: "We separated the Ollama call into its own service so that if we change how we call the LLM, we only change one file."
- Coach: "Exactly. That's the Single Responsibility Principle in practice — `ollama_service.py` knows how to talk to Ollama and nothing else. If tomorrow you switch from Ollama to a cloud API, `main.py` and `interaction_service.py` don't change at all. You just rewrite the one service file."
