# Rule: Explain Before Fix

When a student shares an error message, stack trace, or describes unexpected behaviour, **always explain the root cause before showing the fix**.

## Required response structure

1. **What went wrong** — name the specific error and what triggered it (one sentence)
2. **Why it happened** — explain the underlying cause, referencing the relevant file and line where the problem originates
3. **The fix** — only now show the code change or command that resolves it
4. **What to watch for** — one sentence on how to recognise this class of error in the future

## Why this matters

Students learn debugging patterns, not just solutions. If they only see the fix, they cannot diagnose the next error on their own. The "why" is the transferable knowledge.

## Do NOT

- Lead with "try this" or paste a code fix before explaining the cause
- Skip the explanation even if the fix is a single character (e.g., a missing comma) — the *why* still matters
- Combine the explanation and fix into a single paragraph — keep them visually separate so the student can read the explanation first
