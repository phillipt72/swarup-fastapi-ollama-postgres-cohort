# Module Fundamentals Reference

Each module teaches exactly one fundamental. Use this reference when explaining modules to students.

| Module | Folder | Single fundamental |
|--------|--------|-------------------|
| 0 | `dist/module_00_verify_setup/` | All dependencies must be reachable before any code runs |
| 1 | `dist/module_01_http_server/` | A web server is a long-running HTTP-listening process |
| 2 | `dist/module_02_typed_request/` | Typed request/response — validation lives at the boundary |
| 3 | `dist/module_03_call_ollama/` | Your backend is a client of other local services (Ollama) |
| 4 | `dist/module_04_system_prompt/` | An LLM call is a list of messages with roles; the system message shapes every response |
| 5 | `dist/module_05_save_postgres/` | An application persists state in a database |
| 6 | `dist/module_06_read_history/` | An application reads state back from the database |
| 7 | `dist/module_07_refactor/` | A maintainable codebase separates concerns (refactor with no behaviour change) |
| 8 | `dist/module_08_configuration/` | Code = behaviour, env = environment; fail loudly on missing required vars |

## How modules build on each other

```
Module 0: Can we reach everything?
    ↓
Module 1: Start a web server (FastAPI + uvicorn)
    ↓
Module 2: Accept structured input (Pydantic schemas)
    ↓
Module 3: Call another service (Ollama via httpx)
    ↓
Module 4: Shape the LLM's behaviour (system prompt)
    ↓
Module 5: Save to a database (psycopg + INSERT)
    ↓
Module 6: Read from the database (SELECT + render)
    ↓
Module 7: Refactor into modules (same behaviour, better structure)
    ↓
Module 8: Externalise configuration (env vars + loud failure)
```

## Key architectural layers introduced by module

- **Modules 0–1:** HTTP layer (server, routes)
- **Module 2:** Validation layer (Pydantic at boundaries)
- **Modules 3–4:** External service layer (Ollama client)
- **Modules 5–6:** Data layer (Postgres read/write)
- **Module 7:** Code organisation (services, separation of concerns)
- **Module 8:** Configuration layer (environment variables)

## What each module does NOT include

This is important — students often expect things that are deliberately omitted:

- **No tests in any module** — testing is its own fundamental, not in V1
- **No logging** — `print()` in Module 0 only, nothing else
- **No ORM** — raw SQL with psycopg, never SQLAlchemy
- **No configuration framework** — `os.environ["..."]` reads, never Pydantic BaseSettings
- **No Docker** — local development with native services
- **Modules 1–6 are single-file** — the refactor only happens in Module 7
