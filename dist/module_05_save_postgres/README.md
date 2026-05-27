# Module 5 — Save to Postgres

**Single fundamental:** an application persists state in a database. The state outlives the request.

Every successful answer from `/ask` is now INSERTed into the `interactions` table. A new `/healthz` endpoint reports the reachability of Ollama and Postgres independently. The system prompt from Module 4 still wraps every question.

## Class Flow (5 steps)

1. **Open** this folder (`dist/module_05_save_postgres/`) in Antigravity. Postgres must be running.
2. **Run** it — paste the *Run* commands. Use the page in your browser. Each question now lands as a row in `interactions`.
3. **Read** `app/main.py` in your editor. Find the new `psycopg.connect(...)` and `cur.execute("INSERT ...")` lines plus the new `/healthz` route.
4. **Ask Gemini** to explain it — paste the **Primary prompt** under *Ask Gemini* below into the chat panel. Read what Gemini says. Ask follow-ups.
5. **Answer the Defend It question** at the bottom yourself before moving to Module 6.

(Optional 6th — for hands-on learners: try the *Tweak* suggestion at the end.)

## Run

```bash
source venv/bin/activate
pip install -r requirements.txt   # adds psycopg[binary]
uvicorn app.main:app --reload
```

## Verify (self-check)

```bash
curl -s http://localhost:8000/healthz
# → {"ollama":true,"postgres":true}

curl -s -X POST http://localhost:8000/ask \
  -H "Content-Type: application/json" \
  -d '{"question":"What is the capital of France?"}'

psql "postgresql://postgres:postgres@localhost:5432/llm_question_log" \
  -c "SELECT id, LEFT(question,40), LEFT(answer,60), created_at FROM interactions ORDER BY id DESC LIMIT 10;"
```

To prove persistence: stop uvicorn, restart it, run the SELECT again. The row is still there.

## Ask Gemini

**Primary prompt** (paste this first into the chat panel):

> Walk me through how the answer reaches Postgres in this module. Trace the path from `/ask` through to the row in the `interactions` table. What does Postgres give us that a Python list doesn't?

### More questions if you want to go deeper

**About the doctrine choice — no `get_conn` wrapper yet:**
> Module 5 calls `psycopg.connect(DATABASE_URL)` inline at two places (`/ask` and `/healthz`) instead of wrapping it in a helper like `get_conn()`. Coach me through *why* the doctrine waits until Module 7 to extract that helper. What rule does it satisfy?

**About `/healthz` and narrow exceptions:**
> The `/healthz` route catches `httpx.HTTPError` and `psycopg.Error` specifically — not bare `Exception`. Imagine I had used `except Exception:` instead. Walk me through a concrete bug that the bare except would silently hide.

**Try the failure mode:**
> Stop Postgres (`brew services stop postgresql@16` on Mac). Run `curl /healthz` and `curl POST /ask`. What status codes do you see? What do they tell the operator? Restart Postgres and try again.

**About commit:**
> The INSERT is followed by `conn.commit()`. What would happen if I forgot it? Predict, then test by commenting it out and asking a question and querying the table.

**Self-check before you move to Module 6:**
> Module 6 adds `/history` and a Recent list in the UI. Predict: what new SQL query do I need? What's the smallest possible Pydantic model change? What change to `index.html`?

## Tweak (optional, for hands-on learners)

Wipe the table and watch the row count climb as you ask questions:

```bash
psql "postgresql://postgres:postgres@localhost:5432/llm_question_log" \
  -c "TRUNCATE interactions RESTART IDENTITY;"
```

Then ask three different questions in the browser. After each, run the SELECT from the verify section. You'll see id 1, then 2, then 3. Stop uvicorn. Restart it. Run the SELECT again. **All three rows are still there.** That's the difference between a Python list and a database.

## Defend It (do not paste into Gemini — answer it yourself)

> Why save to a database instead of an in-memory Python list? What does the database give us that a list does not?

## Notes

A Python list only lives in your computer's temporary memory (RAM). If the server crashes, restarts, or you stop the process, that list is wiped. Postgres writes that data to disk, ensuring persistence.

Here is the trace of how that answer travels from the user to the database:

```mermaid
graph TD
    A[Browser / Client] -->|1. POST /ask JSON| B[FastAPI ask route]
    B -->|2. HTTP Request| C[Ollama Client]
    C -->|3. HTTP Response| B
    B -->|4. psycopg.connect| D[(Postgres Database)]
    B -->|5. cur.execute INSERT| D
    B -->|6. conn.commit| D
    B -->|7. JSON Response| A
```

## The Path:
- **The Request**: The user submits a question. FastAPI receives the POST request at `/ask`, parses the JSON body, and validates it using your AskRequest Pydantic model.
- **The LLM Call**: Your backend acts as a client. It opens an HTTP client using `httpx` and sends the question (along with the system prompt) to the Ollama server at `localhost:11434`.
- **The LLM Response**: Ollama generates the text and returns it to your backend.
- **The Database Connection**: Your code opens a connection to Postgres (`psycopg.connect(DATABASE_URL)`).
- **The Insertion**: You open a cursor and execute an `INSERT` statement, sending the question, the generated answer, and the model name to the interactions table.
- **The Commit**: `conn.commit()` is called. This tells Postgres to permanently write the new row to disk.
- The Server Response: The route returns `AskResponse(answer=answer)` back to the browser.