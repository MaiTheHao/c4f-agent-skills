---
name: executing-plans
description: "Use when an approved plan from `writing-plans` is ready to execute, typically in a separate session with review checkpoints. Runs TaskUnits step by step with TDD, keeps changes surgical, verifies at runtime, and saves a walkthrough. Make sure to use this whenever the user asks to run, implement, or continue a saved plan."
---

# Executing Plans

Execute an approved plan task by task with strict TDD, runtime verification, and a walkthrough on completion. The approved plan is the contract: execution permission comes only from plan approval in `writing-plans`.

## Principles

1. **Think before editing.** State assumptions before each unit. If a step is ambiguous or the plan conflicts with the code, stop and ask; do not pick an interpretation silently.
2. **Simplicity first.** Write the minimum logic that satisfies the step. Add no extras, abstractions, or error handling the spec does not call for.
3. **Surgical changes.** Edit only the current unit's `Files`, matching existing style. Remove imports, variables, or functions that your own change orphaned. Leave unrelated dead code alone and list it in the walkthrough.
4. **Goal-driven.** A step is done only when its verification passes. Loop on failure until it does.

## Workflow

1. Load the plan from `local/agents/plans/`; confirm it is approved (`Open Questions` is `None`) and prerequisites (files, branch, dependencies) are in place.
2. Run units in `DependsOn` order. For each step: write the failing test, run it to confirm failure, implement minimally, run it to confirm PASS.
3. Run the unit's `Verify` commands. On failure, read the full untruncated logs and fix the root cause.
4. Check off finished steps and units in the plan (`- [x]`).
5. After all units pass, check every `Acceptance` item and save the walkthrough.

## Hard gate

- Never mark a task done without concrete runtime evidence (passing tests, build, clean logs).
- Never mask errors with dummy fallbacks, swallowed exceptions, or disabled assertions. Fix root causes.
- No scope expansion beyond the unit's `Files` and the plan spec.
- If a scope deviation, blocker, or contradiction with the approved design appears: stop, explain it, name the affected decision, update the plan, and get approval before continuing. Return to `brainstorming` if the design itself must change.

## Locations

- Plan: `local/agents/plans/YYYY-MM-DD-<feature-name>.md` (or `...-fix<N>.md`)
- Walkthrough: `local/agents/walkthroughs/YYYY-MM-DD-<feature-name>.md`
- Use repo-relative paths in all artifacts.

## Walkthrough template

````markdown
# [Feature Name] Walkthrough

## Summary
[What was built and the goal achieved]

## Changes Made
| File | Status | Description |
| :--- | :--- | :--- |
| `path/to/file` | CREATED / MODIFIED | [summary] |

## Verification Evidence
```bash
# command executed, with the output showing PASS
```

## Key Decisions & Notes
- [Edge cases handled, constraints respected]

## Out-of-Scope Observations
- [Unrelated issues noticed but not touched, or "None"]

## Acceptance Checklist
- [x] [Item verified]
````

## Decision rules

| Condition | Action |
| :--- | :--- |
| Verification fails | Read full logs, fix root cause, re-verify |
| All units pass | Check Acceptance, save walkthrough |
| Scope deviation needed | Pause, update plan, request approval |
| Ambiguity or design conflict | Stop and clarify before editing |