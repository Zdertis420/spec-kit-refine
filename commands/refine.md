---
description: "Amend the latest feature with user-provided fixes/additions, then run plan, tasks, analyze, fix issues, and implement."
---

## User Input

```text
$ARGUMENTS
```

The user input describes fixes and/or additions to the **latest feature**. If it is empty, stop and ask the user what to refine.

## Workflow

Run the whole pipeline end to end without pausing for confirmation, unless a step below says to stop. Run each stage by invoking the named command through the Skill tool (not by imitating it), so its own hooks and rules apply.

### 1. Resolve the feature
Use `feature_directory` from `.specify/feature.json`; if missing, use the highest-numbered directory in `specs/`. Do **not** create a new feature or branch. If no feature exists, stop and tell the user to run `__SPECKIT_COMMAND_SPECIFY__` first. Read `spec.md`, `plan.md`, `tasks.md` and the other artifacts there, plus the relevant code, so you understand the current state.

### 2. Amend the spec
Classify each item in the user input as **fix** (existing behavior is wrong or incomplete) or **addition** (new behavior). Edit `spec.md` in place:
- Update or add requirements, user stories, acceptance scenarios, edge cases and success criteria so they reflect the change. Keep the existing structure, numbering and style; keep requirements technology-agnostic as the template demands. A new user story gets the next priority number.
- Never silently delete content: mark a removed item with strikethrough and a one-line reason.
- Add a short `## Refinements` entry (create the section at the end if absent) with today's date and a one-line summary per change, so history stays traceable.
- Do not rewrite unrelated sections. If an item contradicts an existing requirement, change the old requirement rather than leaving both.
- If something is genuinely ambiguous and affects scope, ask the user at most 3 targeted questions before continuing; otherwise decide with a reasonable default and note it in the spec's assumptions.

### 3. Plan
Run `__SPECKIT_COMMAND_PLAN__` with the refinement summary as its argument. Tell it to **update the existing** `plan.md`, `research.md`, `data-model.md`, `contracts/` and `quickstart.md` rather than regenerate them from scratch, preserving decisions that are still valid.

### 4. Tasks
Run `__SPECKIT_COMMAND_TASKS__`. Existing completed tasks (`[X]`) must stay as they are; add new tasks (continuing the ID sequence) for the refinement, and reopen or add a task for any completed work that the change invalidates. Do not duplicate finished work.

### 5. Analyze and fix
Run `__SPECKIT_COMMAND_ANALYZE__`. Then **fix every CRITICAL and HIGH issue, and every MEDIUM issue that is clearly correct to fix**, by editing `spec.md` / `plan.md` / `tasks.md` directly (analyze itself is read-only; the fixing is your job here). Re-run analyze after fixing and repeat at most 3 rounds. Do not edit the constitution; if an issue conflicts with it, adjust the spec/plan/tasks instead. List remaining LOW issues in the final report.

### 6. Implement
Run `__SPECKIT_COMMAND_IMPLEMENT__` and carry it through to completion: all new/reopened tasks done, tasks marked `[X]` in `tasks.md`, project builds/tests pass where applicable.

### 7. Report
Summarize to the user, concisely:
- Which feature was refined and what changed in spec/plan/tasks
- Analyze findings fixed, and any that remain
- Tasks completed, files changed, build/test result
- Anything skipped, assumed, or needing the user's decision

Do not commit unless the user asked for it.
