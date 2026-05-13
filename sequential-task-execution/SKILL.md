---
name: sequential-task-execution
description: Use when a user asks to execute an approved feature plan with task folders, TDD expectations, review requirements, artifacts, pause points between main tasks, or lightweight controlled execution for smaller changes.
---

# Sequential Task Execution

## Overview

Execute an approved implementation plan in a controlled, auditable way.

This skill supports two execution modes:

- **Full Mode** for substantial, risky, multi-step, production-sensitive, or review-heavy feature work.
- **Lite Mode** for small or medium scoped changes where the full workflow would add unnecessary overhead, but basic control is still required.

The agent may recommend a mode, but the user must explicitly approve the execution mode before task files are created or execution begins.

Use this after a feature design, implementation plan, bug-fix plan, or clearly scoped task has been approved and the user wants disciplined execution rather than a single uncontrolled implementation pass.

## Required Skills

This skill relies on the following skills. Load the required skills before starting. If any required skill is missing from the agent's skill directory, install it from the [superpowers skills repository](https://github.com/obra/superpowers/tree/main/skills/) before proceeding.

| Skill | Required In | When |
|-------|-------------|------|
| `using-superpowers` | Full Mode and Lite Mode | At the start of every session |
| `brainstorming` | Full Mode | Before execution; design must be approved first |
| `writing-plans` | Full Mode | Per task; persist implementation details into `plan.md` before coding |
| `test-driven-development` | Full Mode and Lite Mode | For implementation tasks that change behavior |
| `verification-before-completion` | Full Mode and Lite Mode | Before marking any task done |
| `requesting-code-review` | Full Mode | After each implementation task; spec review is mandatory, code-quality review is optional |
| `receiving-code-review` | Full Mode | When review comes back with feedback |
| `systematic-debugging` | Full Mode and Lite Mode | When a test fails or unexpected behavior blocks progress |

Optional skills in Lite Mode:

| Skill | When |
|-------|------|
| `brainstorming` | Use if the approach is unclear or there are multiple reasonable implementation paths |
| `writing-plans` | Use if the implementation details are non-trivial |
| `requesting-code-review` | Use if the user asks for review or the task becomes riskier than expected |
| `receiving-code-review` | Use if a review is performed |

## Mode Proposal And Approval

Before creating task files or starting execution, propose an execution mode to the user and wait for explicit approval.

The agent must recommend one of:

- **Full Mode** for substantial, risky, multi-step, production-sensitive, or review-heavy work.
- **Lite Mode** for small or medium scoped work where full mode would create unnecessary overhead.

The recommendation must include:

- selected mode
- reason for the recommendation
- controls included
- controls omitted compared with the other mode
- whether escalation to Full Mode may be needed

Do not proceed until the user explicitly approves the selected mode.

Accepted approval examples:

- "Approved, use Full Mode"
- "Approved, use Lite Mode"
- "Go with Full Mode"
- "Lite Mode is fine"
- "Use your recommended mode"

General acknowledgments such as "ok", "noted", "sounds good", or "continue" are not enough unless they clearly approve the execution mode.

### Mode Proposal Template

```text
Recommended mode: Lite Mode

Reason:
This appears to be an isolated bug fix affecting one service and one nearby test file.

Controls included:
- task folder
- short plan.md
- todo.md checklist
- TDD where practical
- focused verification
- history.md update

Controls omitted compared with Full Mode:
- no per-task artifact folders by default
- no mandatory spec review
- no mandatory code-quality review
- no required final documentation task unless behavior, setup, or API docs change

Escalation:
If the task reveals broader behavior, schema changes, auth/payment impact, or public API contract changes, stop and propose switching to Full Mode.

Please approve the execution mode before I continue.
```

## Mode Selection Guidance

Recommend **Full Mode** when the task involves any of the following:

- multi-task or multi-phase feature work
- database migrations or schema changes
- authentication, authorization, billing, payments, payouts, or permissions
- data integrity, financial, legal, or compliance-sensitive behavior
- public API changes affecting frontend or external consumers
- broad refactors across several modules
- production-critical backend behavior
- unclear scope requiring formal user checkpoints
- user explicitly requests strict TDD, task folders, reviews, artifacts, or pause points

Recommend **Lite Mode** when the task is limited in scope, such as:

- isolated bug fixes
- small feature additions
- simple frontend tweaks
- small backend behavior changes with clear scope
- test additions
- documentation updates
- configuration changes
- simple refactors with low regression risk

If uncertain, recommend Full Mode.

## Shared Rules For Both Modes

These rules apply in both Full Mode and Lite Mode:

- Do not begin execution until the user approves the execution mode.
- Read `workflow.md` before execution when it exists; it is the task-local execution contract.
- Do not expand scope silently.
- Do not make unrelated refactors.
- Do not skip focused verification before completion.
- Record what changed and how it was verified.
- Use TDD when changing behavior, backend logic, validation, data processing, or bug fixes, unless impractical.
- If automated testing is skipped for a behavior change, record why and describe the manual verification used instead.
- If a new constraint appears during execution, stop, propose a compatibility plan, and wait for approval before continuing.
- If the task becomes broader or riskier than the approved mode allows, stop and propose escalation.

## Templates

This skill includes the following templates in the `templates/` folder. Load the relevant template when the corresponding step is reached:

- `templates/spec-review-request.md` — use when spawning a spec reviewer subagent
- `templates/code-quality-review-request.md` — use when spawning a code-quality reviewer subagent
- `templates/api-contract.md` — use when writing a frontend API contract doc

## Mode Instructions

Once the user has approved the execution mode, read the full mode instructions before creating task files or starting execution:

- Approved **Full Mode** → read `modes/full-mode.md`
- Approved **Lite Mode** → read `modes/lite-mode.md`

# Escalation From Lite Mode To Full Mode

If Lite Mode reveals that the task is broader or riskier than expected, stop and propose switching to Full Mode.

Escalate when you discover:

- multiple independent tasks
- schema or migration work
- auth, billing, payment, permission, or compliance impact
- public API contract changes larger than expected
- high regression risk
- unclear requirements requiring repeated approval gates
- need for formal review artifacts
- need for mandatory documentation as a final task

The escalation proposal must include:

- why Lite Mode is no longer sufficient
- what Full Mode controls will add
- what work has already been completed
- what must be redone or reorganized, if anything

Do not switch modes until the user explicitly approves escalation.

# Constraint Changes

If the user introduces a new constraint during a task:

1. Propose a compatibility plan for the current task.
2. Wait for explicit approval.
3. Update the appropriate tracking file:
   - Product/design constraint: update `design.md` after approved replanning, then update `plan.md` and `todo.md` as needed.
   - Project convention or workflow constraint in Full Mode: update `workflow.md` and current task checklist details in `todo.md`.
   - Project convention or workflow constraint in Lite Mode: update `workflow.md`, `plan.md`, and `todo.md` when the constraint is important enough to track separately; otherwise update `plan.md` and `todo.md`.
4. Implement the approved adjustment.
5. Record the change in `history.md`.

Do not proceed to the next task or completion until the current task respects the new constraint.

# Completion Standards

A task is complete only when:

- approved scope has been implemented
- tests or focused verification have been run, or skipped with a recorded reason
- tracking files are updated
- history records the commands and results
- any required review or documentation for the selected mode is complete
- unresolved limitations are clearly reported to the user

Do not imply that unverified work is complete.
