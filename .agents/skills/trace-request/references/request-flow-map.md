# Request Flow Map — POST /ask

Use this map when tracing the `/ask` request through the application.

## Boundary 1: User → Browser

- **Action:** User types a question in the `<textarea>` and clicks the "Ask" button
- **File:** `app/templates/index.html`, line 45 (event listener)
- **Data format:** Raw string in a DOM element

## Boundary 2: Browser → FastAPI

- **Action:** JavaScript `fetch()` sends a POST request with JSON body
- **File:** `app/templates/index.html`, lines 52–54
- **Data out:** `{"question": "user's text"}` as JSON over HTTP
- **Data in (at FastAPI):** Raw HTTP request body

## Boundary 3: FastAPI route — validation

- **Action:** FastAPI deserialises the JSON body into a Pydantic `AskRequest` object
- **File:** `app/main.py`, line 29 (`ask(payload: AskRequest)`)
- **Schema:** `app/schemas.py`, line 4 (`AskRequest` with a single `question: str` field)
- **Key point:** If the JSON does not match the schema, FastAPI returns a 422 automatically — the route function never runs

## Boundary 4: FastAPI → Ollama service

- **Action:** The route calls `call_ollama(question)` which sends an HTTP POST to the Ollama API
- **File:** `app/services/ollama_service.py`, lines 15–29
- **Data out:** A JSON payload with `model`, `messages` (system + user), and `stream: false`
- **Data in:** Ollama's JSON response containing `message.content`
- **Key point:** The system prompt (line 8–11) shapes every response. This is the Module 4 fundamental.

## Boundary 5: FastAPI → Postgres (save)

- **Action:** The route calls `save_interaction(question, answer)` which INSERTs a row
- **File:** `app/services/interaction_service.py`, lines 10–24
- **SQL:** `INSERT INTO interactions (question, answer, model_name) VALUES (%s, %s, %s)` — parameterised, never string-interpolated
- **Connection:** Uses `get_conn()` from `app/database.py`, which reads `DATABASE_URL` from environment

## Boundary 6: Postgres → FastAPI (read back)

- **Action:** The route calls `fetch_recent_history()` which SELECTs the 10 most recent interactions
- **File:** `app/services/interaction_service.py`, lines 27–36
- **Data out:** A list of `Interaction` Pydantic objects
- **Key point:** `dict_row` cursor factory (line 29) returns dictionaries that Pydantic can consume directly

## Boundary 7: FastAPI → Browser (response)

- **Action:** The route returns an `AskResponse` object which FastAPI serialises to JSON
- **File:** `app/main.py`, line 35
- **Schema:** `app/schemas.py`, line 16 (`AskResponse` with `answer: str` and `history: list[Interaction]`)
- **Data out:** JSON over HTTP back to the browser

## Boundary 8: Browser — render

- **Action:** JavaScript receives the JSON, updates the answer `<div>` and re-renders the history list
- **File:** `app/templates/index.html`, lines 57–60 (success path)
- **Key point:** `escapeHtml()` (line 34) prevents XSS — user input is never inserted as raw HTML

## Summary: data format at each boundary

```
User input (string)
  → JSON string (fetch body)
    → Pydantic AskRequest (validated)
      → Ollama API JSON (messages array)
        → Ollama response string
          → Postgres row (INSERT)
          → Postgres rows (SELECT)
        → Pydantic AskResponse (typed)
      → JSON string (HTTP response)
    → DOM update (rendered HTML)
```
