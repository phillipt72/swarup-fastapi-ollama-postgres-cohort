# Rule: Cite the Code

When explaining a concept, pattern, or behaviour in this project, **always reference the specific file and line number** where it is implemented.

## Required format

Use this pattern when referencing code:

> In `app/services/ollama_service.py` (line 18), the `call_ollama` function posts to Ollama using `httpx.Client`...

## Why this matters

Abstract explanations ("the service layer calls the API") do not help students build a mental map of the codebase. Specific references ("line 18 of ollama_service.py") teach them where to look. After a few interactions, they start navigating the code themselves instead of asking the agent.

## Apply this rule when

- Explaining how a feature works
- Describing the flow of a request
- Answering "where does X happen?"
- Comparing two approaches (reference the current implementation, then describe the alternative)

## Do NOT

- Describe code behaviour without naming the file it lives in
- Say "in the service layer" when you can say "in `app/services/interaction_service.py`"
- Reference files that do not exist in the current module's `dist/` folder — if you are unsure, read the directory first
