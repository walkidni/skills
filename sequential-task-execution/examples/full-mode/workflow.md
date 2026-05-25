# Workflow

This file is the task-local execution contract for project conventions and workflow constraints.

## Mode

Use `sequential-task-execution` Full Mode.

## Source Of Truth

Product behavior and design constraints live in:

- `design.md`

Do not use this file as the product behavior store.

## Approval Gates

- Do not implement before the sequential task split is approved.
- For each main task, present the task-specific implementation approach in chat first.
- Wait for explicit user approval before writing that task's `brainstorm.md`, expanding `plan.md`, or adding detailed checklist items to `todo.md`.
- Pause after each completed main task for user review.
- After the user reviews and approves a closed task, commit that task before starting the next task.

## Reviews

- Required reviews are controlled by this task folder's approved workflow.
- If spec review is required, reviewers are read-only and must not edit files or task tracking.
- If code-quality review is skipped, mark the task's `code-quality-review.md` artifact as skipped.
- If a task is an audit, research task, benchmark, source evaluation, or similar result-producing task, add a clearly named result file under `artifacts/task-XX/` and track it in `todo.md` and `history.md`.

## Testing

- TDD is required for backend behavior changes.
- Prefer focused API/service/unit tests near existing coverage.
- Run focused verification before marking a task done.
- Record exact commands, output summary, and exit code in `history.md`.

## Documentation

- Documentation is a required final task when behavior, setup, or API contracts change.
- Keep product/user-visible behavior separate from implementation details.
- Update docs index files when new docs are added.

## Scope Control

- Do not expand into unrelated features.
- Do not add frontend behavior unless explicitly included in the approved design or task approach.
- Do not silently change task scope; propose a compatibility plan first.
