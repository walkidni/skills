# Full Mode

## Full Mode Overview

Full Mode executes an approved plan one main task at a time. It keeps design, task tracking, TDD, implementation, reviews, and history separate so the work stays auditable and controlled.

Use Full Mode for substantial features, high-risk changes, or work where the user wants strong checkpoints.

## Full Mode Core Workflow

```text
feature brainstorm
-> propose execution mode
-> wait for explicit Full Mode approval
-> scaffold skeleton task folder with design.md
-> discuss and approve sequential TDD task split
-> expand task tracking for the approved split
-> execute one task
-> mandatory spec review
-> ask for optional code-quality review
-> update artifacts/history
-> pause and wait for user to manually review the code before continuing
-> repeat from execute one task
```

## Full Mode Non-Negotiables

In Full Mode, do not skip these because the user is in a hurry:

- execution mode approval
- task folder setup
- sequential main-task execution
- approved feature design persisted in `design.md`
- per-task brainstorm → persist implementation details to `plan.md` using writing-plans style → add todos → implement, in that order
- per-task brainstorms as user-facing review checkpoints
- explicit user approval of task-specific implementation approach before persisting it
- TDD for behavior/backend implementation tasks
- focused verification
- mandatory spec review
- `todo.md` and `history.md` updates
- pause before the next main task
- documentation as the final required task, including API contract documentation for backend features

Only the code-quality review is optional, and only after asking the user.

## Full Mode Design Baseline

Persist the approved feature design in a date-prefixed task folder:

```text
tasks/{YYYY-MM-DD-task-name}/design.md
```

Treat `design.md` as the source-of-truth baseline for drift checks. Do not mutate it during execution unless the user explicitly approves a design replan.

`docs/plans/...` is optional project documentation, not required by this workflow.

Task folder names must start with the current date at folder creation time, using `YYYY-MM-DD-` followed by a short kebab-case task name. Example: `tasks/2026-05-13-online-payment-status-flow/`.

## Full Mode Task Folder

First create only the skeleton task folder:

```text
tasks/{YYYY-MM-DD-task-name}/
  design.md
  plan.md
  todo.md
  history.md
  workflow.md
  artifacts/
```

At this stage:

- Copy the approved feature design into `design.md`.
- Do not copy the design into `plan.md`.
- Make `plan.md` a skeleton with a link/path to `design.md` and empty sections for future task details.
- Make `todo.md` a skeleton placeholder.
- Make `history.md` a short scaffold entry only.
- Make `workflow.md` the local execution contract for project convention constraints, workflow overrides, approval gates, pause rules, review rules, and project-specific deviations. It may point to `design.md` for product behavior.
- Do not create `artifacts/task-XX/` folders until the task split is approved.

## Full Mode Workflow File

`workflow.md` is the local execution contract for the task folder.

Use it for:

- project convention constraints;
- workflow overrides and precedence rules;
- approval gates and pause points;
- review requirements;
- project-specific deviations from the default skill workflow.

Do not use `workflow.md` as the product/design constraint store. Product behavior, requirements, and design constraints belong in `design.md`.

When `workflow.md` conflicts with the generic skill, follow `workflow.md` for that task folder unless it would violate system/developer instructions or explicit user instructions.

## Full Mode Split Into Tasks

After the skeleton exists, propose the sequential task split to the user and wait for approval.

Break the plan into small sequential main tasks. Each main task should have:

- target behavior
- primary test layer
- expected failing test
- implementation scope
- review criteria

After the user approves the split:

- Add task titles or empty task sections to `plan.md`.
- Add default workflow checklist steps to `todo.md` for every main task.
- Create per-task artifact folders and files.

Use `examples/full-mode/todo.md` as the checklist shape. Do not use a one-line-per-task todo for Full Mode. Each main task must expand into workflow checklist items after the split is approved. At this stage, use placeholders such as `Add approved implementation steps from Task N plan`; do not invent granular implementation steps before task-specific brainstorming is approved.

After the task-specific approach is approved and persisted into `plan.md`, replace the implementation-step placeholder in `todo.md` with the granular implementation steps from the approved plan. When the approved task plan has multiple implementation steps, mirror those steps in `todo.md`; do not collapse them into a single `Implement behavior` item.

Use the anonymous top-level examples in `examples/full-mode/` as file-shape references for `design.md`, `plan.md`, `todo.md`, `history.md`, and `workflow.md`.

Do not fill detailed implementation notes into `plan.md` upfront. Fill each task section as that task is brainstormed and approved during execution.

## Full Mode Per-Task Artifacts

After the task split is approved, create this structure for each main task:

```text
tasks/{YYYY-MM-DD-task-name}/artifacts/task-01/
  brainstorm.md
  implementation-notes.md
  spec-review.md
  code-quality-review.md
  <task-result>.md       # optional, for audit/research/benchmark/report outputs
```

Use the same pattern for `task-02`, `task-03`, and so on.

`code-quality-review.md` may stay empty or state that the user declined the optional review.

The task artifact folder may contain additional files when the task naturally produces a result artifact. Common examples include `analysis-report.md`, `audit-report.md`, `benchmark-results.md`, `research-notes.md`, screenshots, logs, generated reports, or other task outputs. For audit, research, benchmarking, source-evaluation, or similar tasks, create a clearly named result file in that task's artifact folder and include it in `todo.md` and `history.md`.

When explaining or scaffolding the workflow, list the artifact files explicitly. Do not collapse this to only `artifacts/task-XX/`; the four required files plus any task-specific result files are part of the required structure for that task.

## Full Mode Task Execution Loop

For each main task:

1. Brainstorm task-specific implementation details in chat with the user.
2. Wait for explicit user approval of the task-specific implementation approach.
3. Only after approval, save the approved brainstorm to `artifacts/task-XX/brainstorm.md`.
4. Persist the approved implementation plan into the task section of `plan.md` using writing-plans style: exact file paths, complete code where appropriate, and exact commands. Do not start implementation until this is written.
5. Replace that task's implementation placeholder in `todo.md` with the approved task implementation steps as granular checklist items. If implementation has multiple concrete steps, each step must have its own checklist item.
6. Write the failing test first.
7. Implement with the main agent.
8. Save implementation notes to `artifacts/task-XX/implementation-notes.md`.
9. Run focused verification.
10. Run mandatory spec review.
11. Save the review to `artifacts/task-XX/spec-review.md`.
12. If spec review passes, ask the user whether to run optional code-quality review.
13. If approved, run code-quality review and save it to `artifacts/task-XX/code-quality-review.md`.
14. Apply only approved fixes. If spec review already passed and a later code-quality review fails, rerun only code-quality review after the fix; do not rerun spec review unless the fix intentionally changes the task spec or user-approved scope.
15. Update `todo.md`.
16. Append `history.md`.
17. Pause for the user's review/approval before starting the next main task. This pause exists specifically to give the user time to manually review the code. Do not skip it or treat a general acknowledgment as a continue signal.
18. After the user reviews and approves the closed task, commit the task changes before starting the next task. Do not start the next task with uncommitted changes from the approved closed task.

## Full Mode Approval Gates

Phrases like "continue the workflow", "start the task", or "go ahead" authorize moving to the next workflow checkpoint only. They do not authorize silently inventing and persisting task-specific implementation details.

Before editing `artifacts/task-XX/brainstorm.md`, filling a task section in `plan.md`, or expanding implementation checklist items in `todo.md`, the agent must first present the task-specific approach in chat and receive explicit approval.

If the user has not approved the task-specific approach, stop after presenting it.

## Full Mode TDD Rule

For implementation tasks, write a failing test before implementation code.

Before writing the test, declare the primary test layer:

- unit
- service
- API
- HTTP/browser
- another repo-appropriate layer

Prefer extending an existing nearby test file unless a new file is clearly justified.

## Full Mode Spec Review

Spec review is mandatory after implementation and focused verification.

Mandatory spec review does not require additional user approval after the task-specific approach has been approved and focused verification is complete. Run it automatically. Only optional code-quality review requires asking the user.

Spawn spec reviewers with medium reasoning effort unless `workflow.md` says otherwise.

The spec-review subagent is read-only. Its prompt must state:

- it is not the main agent
- it must not edit files
- it must not update task tracking
- it must compare implementation against the relevant `plan.md` task section
- task closure authority belongs to the main agent

Use `templates/spec-review-request.md` as the prompt example for spawned spec reviewers, adapting placeholders to the current task.

Save the result in:

```text
artifacts/task-XX/spec-review.md
```

If spec review fails, stop. Explain the failure and ask the user before applying fixes or continuing.

Spec review is the gate for code-quality review:

- Do not start code-quality review until spec review has explicitly passed.
- If spec review fails, apply only approved spec fixes and rerun spec review until it passes.
- Once spec review has passed, treat the task spec as accepted for the current implementation scope.
- Do not rerun spec review for later code-quality-only fixes unless those fixes intentionally change behavior, task scope, or the accepted spec.

## Full Mode Optional Code-Quality Review

After spec review passes, ask the user before spawning a code-quality reviewer.

When the user approves code-quality review, spawn the reviewer with medium reasoning effort unless `workflow.md` says otherwise.

Use a prompt like:

```text
Spec review passed. Do you want me to run an optional code-quality review before closing this task?
```

If approved, spawn a read-only reviewer to check:

- maintainability
- repo convention fit
- unnecessary complexity
- duplication
- naming
- test quality
- error handling
- hidden regressions
- implementation simplicity

Use `templates/code-quality-review-request.md` as the prompt example for spawned code-quality reviewers, adapting placeholders to the current task.

Save the result in:

```text
artifacts/task-XX/code-quality-review.md
```

If the review finds issues, summarize them and ask before applying fixes.

Code-quality re-review loop:

- If code-quality review fails after spec review has passed, apply only approved code-quality fixes.
- After applying those fixes, rerun only the code-quality review.
- Do not rerun spec review for code-quality-only fixes.
- If a proposed code-quality fix would change behavior or scope, stop and treat it as a spec-impacting change before implementing it.

## Full Mode History And Pause

When a main task is accepted:

- mark the relevant `todo.md` items complete
- append a `history.md` entry
- include verification commands/results
- include spec-review result
- include code-quality review result if run
- suggest a commit command if the project workflow expects one
- pause before the next task

After the user reviews and approves the closed task, commit the task changes before starting the next task. The commit should contain only the approved closed task's changes and any related task-tracking updates. If there are unrelated dirty worktree changes, do not include them; ask if the separation is ambiguous.

Do not batch-close multiple main tasks at once.

## Full Mode Required Documentation Task

The last task in every Full Mode plan must be a documentation task. Do not omit it or merge it into implementation tasks.

For backend features, the documentation task must include:

- An API contract under `docs/contracts/{feature-name}-api-contract.md` when backend work changes frontend-facing request/response behavior. Use `templates/api-contract.md` in this skill folder unless `workflow.md` or an explicit project convention says otherwise.
- Updates to any existing implementation docs that the feature changes.
- An entry in the project doc index for any new doc files.

For non-backend features, such as frontend-only, tooling, or config changes, include whatever documentation is appropriate, but an API contract is not required.

The documentation task follows a lighter execution loop:

```text
brainstorm
-> persist to plan.md
-> add todos
-> implement
-> history update
-> pause
```

No spec review or code-quality review is required for the documentation task.
