# Lite Mode

## Lite Mode Overview

Lite Mode executes a small or medium approved change in a controlled but lightweight way.

Use Lite Mode when Full Mode would be excessive, but the work still needs basic discipline: a short plan, trackable steps, tests where appropriate, focused verification, and a concise history entry.

## Lite Mode Core Workflow

```text
confirm approved scope
-> propose execution mode
-> wait for explicit Lite Mode approval
-> create lightweight task folder with design.md
-> write short implementation plan
-> add todo checklist
-> implement with tests where relevant
-> run focused verification
-> update history
-> report result
```

## Lite Mode Non-Negotiables

In Lite Mode, do not skip:

- execution mode approval
- creating `design.md`, `plan.md`, `todo.md`, and `history.md`
- writing or updating tests for behavior changes when practical
- explaining why automated tests were not added, if skipped for a behavior change
- running focused verification before completion
- recording verification results in `history.md`
- stopping before scope expansion

## Lite Mode Folder

Create one lightweight task folder:

```text
tasks/{YYYY-MM-DD-task-name}/
  design.md
  plan.md
  todo.md
  history.md
```

Do not create per-task artifact folders by default.

Only create an `artifacts/` folder if the task produces useful outputs, screenshots, logs, generated files, or review notes.

Optional structure:

```text
tasks/{YYYY-MM-DD-task-name}/
  design.md
  plan.md
  todo.md
  history.md
  workflow.md
  artifacts/
```

Add `workflow.md` in Lite Mode when the task has important local constraints, workflow overrides, approval gates, or project-specific deviations. For tiny tasks with no separate execution contract, omit it.

## Lite Mode Plan

Write a short `plan.md` before editing code.

If the task has an approved design or non-trivial requirements, persist them first in:

```text
tasks/{YYYY-MM-DD-task-name}/design.md
```

Keep `design.md` as the baseline and `plan.md` as the implementation plan. For very small tasks with no separate design discussion, `design.md` may be a short approved-scope summary.

The plan should include:

```md
# Plan

## Scope

Briefly describe the requested change.

## Files likely to change

- `path/to/file.ext`
- `path/to/test_file.ext`

## Approach

1. Step one.
2. Step two.
3. Step three.

## Verification

- Command or manual check 1
- Command or manual check 2
```

Keep this concise. Do not copy a large source design into `plan.md`.

## Lite Mode Todo

Create `todo.md` with a simple checklist:

```md
# Todo

- [ ] Locate relevant code
- [ ] Add or update test, if behavior changes
- [ ] Implement change
- [ ] Run focused verification
- [ ] Update history
```

Adjust the checklist to the actual task.

## Lite Mode TDD Rule

Use TDD when the task changes behavior, especially for:

- backend logic
- API behavior
- data processing
- validation
- bug fixes
- business rules

Before implementation, write or update a failing test where practical.

Declare the primary test layer before writing the test:

- unit
- service
- API
- integration
- browser/UI
- manual verification only

Prefer extending an existing nearby test file unless a new file is clearly justified.

If a failing automated test is not practical, explain why in `plan.md` and use focused manual verification instead.

## Lite Mode Implementation

Implement only the approved scope.

Avoid opportunistic refactors unless they are necessary for the task.

If you discover a larger issue, stop and ask before expanding the scope.

## Lite Mode Verification

Before completion, run focused verification.

Examples:

```text
pytest path/to/test_file.py
npm test -- relevant-test
npm run typecheck
npm run lint
php artisan test --filter SomeTest
```

Use the smallest reliable verification set first.

Run broader checks only when the change affects shared behavior.

Record the exact commands and results in `history.md`.

## Lite Mode History

Append a concise entry to `history.md` when complete:

```md
# History

## YYYY-MM-DD - Task completed

### Changed

- Updated `path/to/file.ext` to ...
- Added test coverage in `path/to/test_file.ext`.

### Verification

- `command here` - passed
- `manual check here` - passed

### Notes

- Any important limitation or follow-up.
```

## Lite Mode User Checkpoints

Lite Mode has fewer pauses than Full Mode.

Ask for user approval before implementation only when:

- the approach is ambiguous
- there are multiple reasonable implementation options
- the task may affect behavior outside the requested scope
- a shortcut or tradeoff is being considered
- tests cannot reasonably be added for a behavior change

Otherwise, proceed after writing the lightweight plan.

## Lite Mode Reviews

Spec review is not mandatory in Lite Mode.

Offer a review only when:

- the task touches important behavior
- the user asks for extra caution
- the implementation became more complex than expected
- the change affects public API behavior
- the change affects shared or risky logic

If a review is performed, save it as:

```text
tasks/{YYYY-MM-DD-task-name}/artifacts/review.md
```

If review finds issues, summarize them and ask before applying fixes.

## Lite Mode Documentation

Documentation is required only when the task changes:

- user-facing behavior
- public API behavior
- setup instructions
- configuration
- developer workflows

For backend API changes, update or create an API contract only if the endpoint contract changes.

For small internal fixes, documentation may be skipped.

## Lite Mode Completion Response

When done, summarize:

- what changed
- tests or checks run
- files created or updated
- limitations or follow-up needed

Do not claim completion unless verification has been run or the reason for not running it is clearly stated.
