---
skill: explain-module
version: 1.0.0
description: >
  Reads a specific module folder (dist/module_NN_*/) and explains the single
  fundamental it teaches — what code was added, what stayed the same, and why.
  Connects each module to the architectural layer it introduces.
  Use when a student asks to understand a module, wants a summary of what
  they just built, or needs the big picture of how modules connect.
triggers:
  - "explain module"
  - "what does this module teach"
  - "what did I just build"
  - "summarise this module"
  - "what changed in this module"
  - "what's the point of module"
---

# Explain Module Skill

## Purpose

Each module in this curriculum teaches exactly one fundamental. Students sometimes lose sight of that single idea amid the code changes. This skill reads the module's actual files, identifies the fundamental, and explains it clearly — connecting the code to the concept.

## Step 1: Identify which module

Determine which module the student is asking about:

- If they say "module 5" or "Module 5," use `dist/module_05_save_postgres/`
- If they have a file open in `dist/module_NN_*/`, use that module number
- If they say "this module" or "what did I just build," ask which module they are on

Consult `references/module-fundamentals.md` for the module list and fundamentals.

## Step 2: Read the module's files

Read the module's source files in `dist/module_NN_*/app/`. Compare mentally with the previous module's files to understand what changed.

Also read the module's `README.md` if it exists — it contains the exercise instructions and Defend It questions.

## Step 3: Explain the fundamental

Structure your explanation as:

### 1. The one thing this module teaches
State the fundamental in one sentence. Use the exact framing from `references/module-fundamentals.md`.

### 2. What code was added or changed
Name the specific files and lines that are new or modified in this module. Cite them precisely.

### 3. Why this matters
Explain why this fundamental is important for building real applications. Connect it to the student's future work, not just this exercise.

### 4. What stayed the same
Name what was NOT changed. This is equally important — it shows the student that adding a new capability does not require rewriting everything.

### 5. How it connects to the module before and after
One sentence linking backward ("Module 4 gave us the LLM call; this module saves the result") and one forward ("Next module will read this data back").

## NEVER

- Explain code from a module the student has not reached yet
- List every line of code in the module — focus on the meaningful changes
- Say "this module teaches several concepts" — each module teaches ONE fundamental. If you think it teaches more, you have misread the curriculum.
- Skip the "what stayed the same" section — it is critical for understanding incremental development

## Exit criteria

- [ ] The single fundamental is stated in one sentence
- [ ] New or changed code is cited with file names and line numbers
- [ ] The explanation connects backward to the previous module and forward to the next
- [ ] The student can restate the fundamental in their own words (ask them)
