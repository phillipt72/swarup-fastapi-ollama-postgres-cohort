# Workflow: Module Checkpoint

**Trigger:** `/checkpoint` or `/checkpoint N` (where N is the module number)

This workflow runs a structured comprehension check for a specific module. It is a multi-step sequence the student explicitly invokes — unlike skills, which the agent activates on its own.

---

## Step 1: Identify the module

If the student typed `/checkpoint 5`, use Module 5.
If they typed `/checkpoint` without a number, ask: "Which module do you want to checkpoint?"

## Step 2: Read the module

Read the module's source files in `dist/module_NN_*/app/` and its `README.md`.

## Step 3: Summarise the fundamental

State the module's single fundamental in one sentence. Then list the 2–3 most important code changes in that module (file, line, what changed).

Keep this under 5 sentences total. The student should already know this — you are refreshing, not teaching.

## Step 4: Ask two comprehension questions

Ask exactly two questions, one at a time. Wait for the student's answer before asking the second.

**Question 1** should test understanding of WHAT the code does:
> Example: "In Module 5, what SQL statement does `save_interaction()` execute, and what are the three values it inserts?"

**Question 2** should test understanding of WHY a decision was made:
> Example: "Why does the project use parameterised queries (`%s`) instead of f-strings to build the SQL?"

Design questions that have specific, verifiable answers — not open-ended discussion prompts.

## Step 5: Evaluate their answers

For each answer:

- If correct: confirm with one sentence explaining why it matters
- If partially correct: acknowledge the right part, then ask a follow-up that targets the gap
- If incorrect: do not give the answer. Point them to the specific file and line where they can find it, and ask them to try again

## Step 6: Give a readiness verdict

After both questions are answered (correctly or after coaching):

**Ready to move on:** "You've got Module N solid. The key insight — [fundamental in one sentence] — is clear in how you reasoned through both questions. Move to Module [N+1] when you're ready."

**Needs review:** "You understand [what they got right], but [specific gap] needs another look. Before moving on, re-read [specific file] and try to explain [specific thing] to yourself. Then run `/checkpoint N` again."

---

## Do NOT

- Ask more than two questions — this is a quick checkpoint, not an exam
- Accept "I don't know" without redirecting to the specific file where the answer lives
- Give the readiness verdict before the student has attempted both questions
- Say "not ready" without naming the specific gap and where to find the answer
