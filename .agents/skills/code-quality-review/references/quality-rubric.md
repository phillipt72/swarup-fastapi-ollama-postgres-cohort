# Code Quality Rubric

Use this rubric to evaluate code in the Local LLM Question Log project. Score each dimension as **Strong**, **Adequate**, or **Needs work**. Always cite specific files and lines as evidence.

---

## 1. Naming clarity

Do function names, variable names, and file names clearly describe what they do?

- **Strong:** Names are verb-noun for functions (`call_ollama`, `save_interaction`), noun for variables (`question`, `answer`), descriptive for files (`ollama_service.py`). A reader understands purpose without reading the body.
- **Adequate:** Most names are clear but a few are ambiguous (e.g., `data`, `result`, `handler`).
- **Needs work:** Names require reading the implementation to understand (e.g., `process()`, `do_thing()`, `x`).

## 2. Separation of concerns

Does each file/function do one thing? Are responsibilities clearly divided?

- **Strong:** Routes in `main.py` only coordinate. Business logic in `services/`. Database access in `database.py`. Schemas in `schemas.py`. No file does two jobs.
- **Adequate:** Mostly separated but some logic leaks between layers (e.g., a route that contains SQL).
- **Needs work:** Multiple responsibilities in a single function or file. A change to one concern forces changes in unrelated code.

## 3. Error handling

Are errors handled at the right level? Are error messages useful?

- **Strong:** Errors caught at the narrowest scope (`psycopg.Error`, `httpx.HTTPError`). Error messages tell the user what failed and what to do. No bare `except Exception`.
- **Adequate:** Errors are caught but messages are generic ("something went wrong").
- **Needs work:** Bare `except` blocks, swallowed errors (catch and pass), or no error handling at all.

## 4. Boundary validation

Is data validated where it enters the system?

- **Strong:** Pydantic models validate all incoming data at the API boundary. Invalid requests never reach business logic. Type hints at function signatures.
- **Adequate:** Some validation exists but is inconsistent or happens too late (e.g., checking for empty strings inside the service instead of at the route).
- **Needs work:** No validation — raw user input flows directly into SQL queries or API calls.

## 5. Simplicity and YAGNI

Is the code as simple as possible for what it does? Are there unnecessary abstractions?

- **Strong:** No code exists that is not needed right now. No wrapper functions called from one place. No configuration classes, no custom exception hierarchies, no "extensibility hooks."
- **Adequate:** Mostly simple but contains one or two abstractions that are not yet needed.
- **Needs work:** Over-engineered — base classes, factory patterns, configuration frameworks for a three-endpoint application.

## 6. Dependency management

Are dependencies explicit, minimal, and pinned?

- **Strong:** `requirements.txt` lists every dependency with version pins. No unlisted imports. No unnecessary packages.
- **Adequate:** Dependencies are listed but not all are pinned, or there are packages installed that are not used.
- **Needs work:** No requirements file, or dependencies installed ad-hoc without recording them.

## 7. Security basics

Are obvious security risks handled?

- **Strong:** SQL queries use parameterised statements (never string interpolation). User input is escaped before rendering in HTML. Environment variables hold secrets, not source code.
- **Adequate:** Most security basics are covered but one area is weak.
- **Needs work:** SQL injection vectors, unescaped user input in HTML, or secrets hardcoded in source.

---

## How to use this rubric

1. Read the code being reviewed
2. Score each dimension with evidence (file name, line number, specific pattern)
3. Note: this project intentionally omits tests, logging, and CI/CD — do NOT penalise for their absence. The AGENTS.md coding doctrine explains why.
4. When scoring "Needs work," always suggest the specific change that would raise it to "Adequate"
