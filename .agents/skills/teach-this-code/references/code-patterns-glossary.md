# Code Patterns Glossary

Use this glossary when naming patterns during code teaching. Each entry has the pattern name, a plain-language definition suitable for mid-career professionals who are not software engineers, and where it appears in this project.

---

## Eager environment loading

**What it is:** Loading configuration values (like database URLs, API endpoints) at the very start of the program, before importing modules that need them.

**Plain language:** "Set up the room before the guests arrive. If a module tries to read a setting during import and the setting is not there yet, the program crashes."

**Where in this project:** `app/main.py` lines 1–3 — `load_dotenv()` runs before any app imports.

---

## Context manager

**What it is:** A Python object that handles setup and cleanup automatically using a `with` statement. When you enter the `with` block, setup runs. When you leave (even if an error occurs), cleanup runs.

**Plain language:** "It's like opening a file, doing your work, and having Python close the file for you automatically — even if something goes wrong in the middle."

**Where in this project:** `app/database.py` line 9 — `get_conn()` is a context manager that opens a database connection and ensures it is closed when done. Used everywhere with `with get_conn() as conn:`.

---

## Parameterised query

**What it is:** A SQL query where user-provided values are passed separately from the SQL string, using placeholders like `%s`. The database driver handles escaping and quoting, preventing SQL injection.

**Plain language:** "Instead of gluing user input directly into your SQL (dangerous — someone could type SQL commands into your form), you leave placeholders and hand the values separately. The database knows the difference between 'part of the query' and 'user data.'"

**Where in this project:** `app/services/interaction_service.py` line 15 — `VALUES (%s, %s, %s)` with values passed as a tuple.

---

## Boundary validation

**What it is:** Checking that incoming data is valid at the exact point it enters your system — not deeper inside the code.

**Plain language:** "Check IDs at the door, not inside the building. If bad data gets past the entrance, every room has to defend itself. If you validate at the boundary, the rest of the code can trust its inputs."

**Where in this project:** `app/schemas.py` — Pydantic models (`AskRequest`, `AskResponse`, `Interaction`) define the shape of valid data. FastAPI applies these automatically at the route level.

---

## Decorator

**What it is:** A Python feature that wraps a function with additional behaviour. Written as `@something` above the function definition. The decorator runs before and/or after your function.

**Plain language:** "It's a label you stick on a function that says 'before running this function, do X first.' In FastAPI, `@app.post("/ask")` means 'when someone sends a POST request to /ask, run this function.'"

**Where in this project:** `app/main.py` lines 23, 28, 38, 43 — route decorators `@app.get()` and `@app.post()`.

---

## Separation of concerns

**What it is:** Organising code so that each file or function has one job. Changes to one concern (e.g., how you talk to the database) do not require changes to unrelated concerns (e.g., how you talk to Ollama).

**Plain language:** "The kitchen does not handle reservations. The host does not cook. Each part of the restaurant has one job, and they communicate through well-defined handoffs."

**Where in this project:** After Module 7 refactor — `main.py` coordinates, `ollama_service.py` handles LLM calls, `interaction_service.py` handles database operations, `database.py` handles connections.

---

## Type hints at boundaries

**What it is:** Using Python type annotations on function signatures and Pydantic models to declare what data looks like as it crosses from one part of the system to another.

**Plain language:** "It's like labelling the plugs and sockets so you do not plug the monitor cable into the network port. The types tell you — and the computer — what shape of data fits where."

**Where in this project:** `app/main.py` line 29 — `def ask(payload: AskRequest)` declares that this function expects an `AskRequest` object. `app/schemas.py` defines what `AskRequest` looks like.

---

## Loud failure

**What it is:** Designing your program to crash immediately with a clear error message when something essential is missing, instead of continuing with a default value and failing mysteriously later.

**Plain language:** "If the building has no fire exits, you want to know before the grand opening — not during the fire. A `KeyError` on startup that says 'DATABASE_URL is missing' is better than a silent failure at 2am when a customer's data does not save."

**Where in this project:** `app/database.py` line 5 — `os.environ["DATABASE_URL"]` (not `.get()` with a default). If the variable is missing, Python crashes immediately with a clear `KeyError`.

---

## HTTP client pattern

**What it is:** Your application acting as a client of another service — sending HTTP requests to an API and processing the response.

**Plain language:** "Your app is not just a server answering questions — it is also a customer, walking up to Ollama's counter and placing an order. It sends a request, waits for the response, and brings the answer back to the user."

**Where in this project:** `app/services/ollama_service.py` lines 15–29 — `httpx.Client` sends POST requests to Ollama's `/api/chat` endpoint.

---

## Row factory

**What it is:** A psycopg feature that controls how database rows are returned — as tuples (default), dictionaries, or other formats.

**Plain language:** "By default, the database gives you rows as numbered lists like `(1, 'hello', 'world')`. A row factory changes that to named dictionaries like `{'id': 1, 'question': 'hello', 'answer': 'world'}` — much easier to work with."

**Where in this project:** `app/services/interaction_service.py` line 29 — `cursor(row_factory=dict_row)` returns dictionaries that Pydantic can consume directly.

---

## Static file mounting

**What it is:** Telling the web server to serve files (CSS, JavaScript, images) directly from a directory without processing them.

**Plain language:** "Some files do not need Python to handle them — they are just files the browser downloads. Mounting tells FastAPI: 'anything requested under /static/, just hand them the file from this folder.'"

**Where in this project:** `app/main.py` line 19 — `app.mount("/static", StaticFiles(directory="app/static"))`.

---

## Template rendering

**What it is:** Combining an HTML template with data to produce a complete web page. The template contains placeholders; the server fills them in before sending the page to the browser.

**Plain language:** "It's like a form letter with blanks. The template is the letter; your data fills in the blanks. The browser receives the finished page, not the template."

**Where in this project:** `app/main.py` lines 20, 25 — Jinja2 templates in `app/templates/`. The index route renders `index.html` with the request context.
