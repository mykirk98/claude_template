# Plan Execution Guideline

> This document defines how Claude Code reads and executes `plan.md`.
> For how to write a plan, see `plan_guideline.md`.

---

## Execution Rules

### 1. One task at a time
- Execute only the **first unchecked** `- [ ]` item in the list — never move on to the next until it is complete.

### 2. All completion criteria must be met
- If a task includes `Acceptance Criteria:`, every criterion must be satisfied before the task is considered done.
- If no criteria are listed, use the task description itself as the standard.

### 3. Stop after completion
- When a task is done, update `- [ ]` to `- [x]`.
- Then **stop immediately** and wait for human review — do not resume until approved.

### 4. Stay within scope
- Do not modify any file, module, or feature not mentioned in the task.
- Do not touch code that "seems like it could use improvement" if it is outside the task scope.
- If out-of-scope work appears necessary, report it to the human instead of acting on it.

### 5. Write a log entry after each step
- When a step is complete, append an entry to the log file before stopping.
- Log file location: same directory as the plan file.
- Log file naming: `<plan-filename>-log.md` (e.g. `plan01.md` → `plan01-log.md`).
- If the log file does not exist, create it.

Each log entry must follow the format in `template/plan-log-template.md`.

### 6. Commit after each step
- When a step is complete, commit the code changes with an appropriate type (`feat`, `fix`, `refactor`, etc.).
- Do **not** include plan files (`plan*.md`, `plan*-log.md`) in this commit.
- After **all** steps are complete, commit `plan*.md` and `plan*-log.md` together in a single `docs` commit.