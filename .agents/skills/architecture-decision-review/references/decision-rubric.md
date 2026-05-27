# Architecture Decision Rubric

Use this rubric to evaluate architectural decisions in the Local LLM Question Log project. Score each dimension for the chosen approach AND its top alternative.

---

## Dimension 1: Simplicity

How much does a developer need to know to understand this part of the code?

- **High (3):** One concept, one file, obvious control flow. A developer who has never seen the codebase can read it in under 60 seconds.
- **Medium (2):** Requires understanding one abstraction layer or configuration pattern. Understandable in 5 minutes with documentation.
- **Low (1):** Multiple abstractions, configuration files, or framework conventions to learn before the code makes sense.

## Dimension 2: Learnability

How well does this approach teach the underlying concept?

- **High (3):** The code directly maps to the concept. Reading the code IS learning the concept. No framework magic hides the mechanism.
- **Medium (2):** The concept is visible but some framework behaviour is implicit (e.g., you need to know that FastAPI calls Pydantic automatically).
- **Low (1):** The framework does so much implicitly that the student does not understand what is actually happening (e.g., an ORM hides SQL entirely).

## Dimension 3: Maintainability

How easy is it to change this code when requirements evolve?

- **High (3):** Changes are localised to one file or function. The change does not ripple to unrelated code. The developer can predict the impact before making the change.
- **Medium (2):** Changes require updates in 2–3 places but the connections are clear.
- **Low (1):** Changes ripple unpredictably. Modifying one part breaks something unrelated.

## Dimension 4: Production-readiness

How close is this approach to what a production team would ship?

- **High (3):** A production team would use this approach as-is or with minimal additions (e.g., add connection pooling but keep psycopg).
- **Medium (2):** A production team would use the same library but add significant infrastructure (e.g., keep psycopg but add Alembic migrations, connection pooling, retry logic).
- **Low (1):** A production team would replace this approach entirely (e.g., replace raw SQL with an ORM for a large application with many models).

---

## Common decisions in this project and their typical scores

These are starting points — adjust based on the student's specific question.

### psycopg vs SQLAlchemy
| Dimension | psycopg (chosen) | SQLAlchemy (alternative) |
|-----------|------------------|--------------------------|
| Simplicity | High — raw SQL, no ORM concepts | Low — session management, declarative models, engine config |
| Learnability | High — student sees actual SQL | Low — SQL hidden behind Python objects |
| Maintainability | Medium — fine for 1 table, painful at 20 | High — migrations, relationships, query builder |
| Production-readiness | Medium — works but needs pooling, migrations | High — industry standard for Python ORMs |

### os.environ vs Pydantic BaseSettings
| Dimension | os.environ (chosen) | BaseSettings (alternative) |
|-----------|---------------------|---------------------------|
| Simplicity | High — 3 lines, no imports | Medium — class definition, .env parsing, type coercion |
| Learnability | High — student sees exactly what happens on missing var | Medium — magic happens in the base class |
| Maintainability | Medium — scattered reads, no validation | High — centralised, typed, validated |
| Production-readiness | Low — no validation, no defaults, no grouping | High — industry standard for FastAPI config |

### httpx vs requests
| Dimension | httpx (chosen) | requests (alternative) |
|-----------|---------------|----------------------|
| Simplicity | High — nearly identical API to requests | High — most widely known HTTP library |
| Learnability | High — same concepts, modern API | High — enormous documentation and tutorials |
| Maintainability | High — async-ready if needed later | Medium — no async support without additional libraries |
| Production-readiness | High — modern, async-capable, actively maintained | Medium — sync-only, aging but stable |

### Single file (Modules 1–6) vs early refactor
| Dimension | Single file (chosen) | Early refactor (alternative) |
|-----------|---------------------|------------------------------|
| Simplicity | High — one file, scroll to find everything | Medium — must navigate multiple files |
| Learnability | High — student sees full picture in one place | Low — cognitive load of understanding file organisation before understanding code |
| Maintainability | Low — becomes unwieldy past ~150 lines | High — each file has one responsibility |
| Production-readiness | Low — no team would ship a single-file API | High — standard project structure |

---

## How to use this rubric

1. Identify the specific decision the student is asking about
2. Name the chosen approach and the most common alternative
3. Score both across all four dimensions
4. State the verdict: what was gained, what was sacrificed, and when the alternative would be the right choice
5. Always be honest about production-readiness — students deserve to know what they would need to change
