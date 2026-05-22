# How a Module Works

The course is nine modules numbered 00 through 08. Module 00 is install; Modules 01-08 follow the same small repeatable loop — once you've done Modules 1 and 2, every later module has the same shape. This page is the map of that shape.

Open it once before Module 1. Refer back when something feels off.

---

## The nine-step rhythm

Every module from Module 1 onward follows the same nine beats:

1. **`cd` into the module folder** — `cd dist/module_NN_<name>/` from the cohort repo root. (E.g. `cd dist/module_03_call_ollama/`.)
2. **Activate the shared venv** — `source ../../venv/bin/activate` (macOS and WSL2 — same command). Your prompt should now start with `(venv)`.
3. **Install any new dep, if the module introduces one** — `pip install -r requirements.txt`. Modules 1, 3, 5, 8 actually add a new dependency here; the rest run the same line as a no-op.
4. **Run it** — usually `uvicorn app.main:app --reload`. Module 8 adds `cp .env.example .env` before the uvicorn line. Open <http://localhost:8000>.
5. **Use the page** — actually click the button, ask a question, watch what happens in the browser and in the uvicorn log.
6. **Read the changed files** — the README tells you exactly which. Modules 1-6 it's usually just `app/main.py`. Modules 7-8 it's several files because the code is no longer in one place — the README lists them. Read the whole file with the README open beside it.
7. **Ask Gemini the Primary prompt** — paste the README's *Primary prompt* into the Gemini chat panel in Antigravity. Read what it says. Ask follow-ups in your own words.
8. **Defend It (do not paste into Gemini)** — answer the Defend-It question at the bottom of the README yourself, out loud or in writing. This is the only step that proves you actually understand the module.
9. **Commit** — `git add -A && git commit -m "Module N complete"`. One commit per module is enough; you're keeping a personal trail of where you are in your own understanding, not engineering a fine-grained log.

**Optional 10th — Tweak.** Each module's README suggests one small change (swap a system prompt, run a different SQL query, break a value and watch the failure). Hands-on learners gain a lot here. Not required, but recommended if the module clicked quickly and you have time before the next.

---

## What stays the same — what varies

| | Same in every module | Varies per module |
|---|---|---|
| **Pattern** | The 9-step rhythm above | — |
| **`cd` target** | `dist/module_NN_<name>/` | `NN` and `<name>` |
| **venv command** | `source ../../venv/bin/activate` | Never changes |
| **Run command** | `uvicorn app.main:app --reload` | Module 8 prepends `cp .env.example .env` |
| **File to read** | The README points at it | Modules 1-6: usually just `app/main.py`. Modules 7-8: several files (the README lists them) |
| **Gemini prompt shape** | One Primary prompt + optional "more questions if you want to go deeper" | The actual prompt is module-specific |
| **Defend-It** | One question at the bottom, do NOT paste into Gemini | Question text varies |
| **Commit message** | `"Module N complete"` | Just the number |

After Module 2 you'll find you stop reading the rhythm — you do it from muscle memory. That's the point.

---

## The small steps you stop noticing after Module 3

These are the things that are silently true in every Run block but only get spelled out in the early modules. Until they become invisible, this is the checklist:

- **One terminal per running uvicorn.** If uvicorn is running in terminal A, do not start uvicorn again in terminal B — the second one will fail with "address already in use" because both are trying to listen on port 8000. (Open a second terminal for `psql` or other side-quests; that's fine.)
- **Every fresh terminal needs the venv re-activated.** Opening a new tab does NOT carry the venv across. The symptom is `bash: uvicorn: command not found` — that's the signal. Re-activate.
- **`(venv)` in your prompt = venv is active.** No `(venv)` = venv is not active. Always glance at the prompt before pasting a command.
- **`cd` location matters.** You should be inside `dist/module_NN_<slug>/` when you run module commands, NOT at the cohort root. Look at your prompt — if it ends in `module_NN_<slug>$` you're in the right place.
- **The cohort repo never changes module to module.** You don't re-clone, you don't pull, you don't switch branches. Just `cd` into the next folder.

---

## When uvicorn restarts itself, and when you restart it

`uvicorn ... --reload` watches files in your project folder. When you **save** a file, uvicorn auto-restarts within a second. You'll see a log line like `Detected change in 'app/main.py'. Reloading...`. **This is the normal case** — for every code edit in this course, you just save the file. No manual restart. This includes `.env` edits in Module 8: saving `.env` triggers the same reload, which re-imports the module and re-reads the env vars.

You need to restart uvicorn manually (`Ctrl+C`, then re-run `uvicorn app.main:app --reload`) only when:

- You added a **new dependency** with `pip install` — the running uvicorn process has the old set of packages loaded and won't see the new one until restarted.
- Uvicorn is stuck in a failed-startup state from an earlier error you've since fixed.

If you find yourself doing `Ctrl+C` + restart after every code edit, you're working harder than the tool requires. Save the file and watch the uvicorn log.

---

## When to commit

At least once per coding module (Modules 1-8), after you've answered Defend It. You're not building a software engineering log of fine-grained commits — you're building a record of where you are in your own understanding. One commit per coding module gives you a clean undo point if a later module goes sideways: `git log` will show eight commits when you're done — one per coding module. (Module 0 is install; nothing app-side to commit.)

```bash
git add -A
git commit -m "Module 5 complete"
```

You're committing inside your own cohort clone. You're not pushing this anywhere; it's your local trail.

---

## Common confusions

| Symptom | What's happening | One-line fix |
|---|---|---|
| `bash: uvicorn: command not found` | Fresh terminal, venv not active | `source ../../venv/bin/activate` from inside the dist folder |
| Browser shows old output after you edited a file | uvicorn didn't pick it up, OR you edited the wrong file | Check the uvicorn log for a "Reloading..." line. Check your `cd` location. |
| `address already in use` when starting uvicorn | Another uvicorn is still running in some other terminal | Find that terminal and `Ctrl+C`. If you can't find it: `lsof -i :8000` (macOS / WSL2) shows the process |
| Module 8 only: `KeyError: 'DATABASE_URL'` | `.env` not in current directory, or has the wrong key | `pwd` should end in `module_08_configuration`. Then `cp .env.example .env`. Restart uvicorn. |

For everything else: paste the failing line into the Gemini chat panel — `AGENTS.md` is loaded at the cohort root and Gemini knows the course's friction points.

---

## Where to go for deeper detail

- **The exact `Run` commands and the Primary prompt for the module you're on:** that module's README at `dist/module_NN_<slug>/README.md`.
- **What every module is for (the one-fundamental-per-module map):** the [cohort README](../README.md).
- **How Gemini is coached to behave in this course:** [`AGENTS.md`](../AGENTS.md) at the cohort root.
- **The full narrative walkthrough, module by module:** the crash-course PDF you received with the course materials.
- **Publishing your V1 at the end:** [`publish_your_work.md`](publish_your_work.md).

---

*This page is the map. The modules are the territory.*
