---
name: executing-plans
description: "Use when an approved plan from `writing-plans` is ready to execute, typically in a separate session with review checkpoints. Runs TaskUnits step by step with TDD, keeps changes surgical, verifies at runtime, and saves a walkthrough. Make sure to use this whenever the user asks to run, implement, resume, or continue a saved plan."
---

# Executing Plans

Execute an approved plan task by task with strict TDD, runtime verification, and a walkthrough on completion. The approved plan is the contract: execution permission comes only from plan approval in `writing-plans`.

## Principles

1. **Think before editing.** State assumptions before each unit. If a step is ambiguous or the plan conflicts with the code, stop and ask; do not pick an interpretation silently.
2. **Simplicity first.** Write the minimum logic that satisfies the step. Add no extras, abstractions, or error handling the spec does not call for.
3. **Surgical changes.** Edit only the current unit's `Files`, matching existing style. Remove imports, variables, or functions that your own change orphaned. Leave unrelated dead code alone and list it in the walkthrough.
4. **Goal-driven.** A step is done only when its verification passes. Loop on failure, within the retry limit below.
5. **Evidence, not claims.** Every "done" points to real command output from this session, never a paraphrase or an expectation.

## Preflight (before the first edit)

1. Load the plan from `local/agents/plans/`. If several match, ask which; never guess.
2. Confirm it is approved: `Status` is APPROVED, `Open Questions` is `None`, no `TODO`/`TBD` remains. Otherwise stop and return to `writing-plans`.
3. Check `Prerequisites` (branch, dependencies, environment). Do not install or upgrade anything the plan does not list.
4. Run the `Baseline` command and record the result. Failures that exist before any edit are **pre-existing**: list them in the walkthrough, do not fix them.
5. Validate anchors: every `MODIFY`/`DELETE` file exists and its named symbol and line range match. Minor drift (lines shifted, symbol unchanged): proceed and note it. Material drift (symbol missing, renamed, or changed meaning): stop.
6. Note uncommitted changes unrelated to the plan; never revert or touch them.

**Resuming:** if boxes are already checked, find the first unchecked step and re-run the last completed unit's `Verify` to confirm the state before continuing.

## Workflow

1. Run units whose `DependsOn` are all done, in ID order. Run sequentially unless the environment supports parallel agents; unit `Files` are disjoint, so parallel is safe.
2. For each step: state the assumption in one line, write the failing test, run it and confirm it fails **for the expected reason**, implement minimally, run it and confirm PASS. For `(no-test)` steps: apply the change, then run the step's verification command.
3. Run the unit's `Verify` commands. On failure, read the full untruncated logs and fix the root cause. After 3 failed fix attempts on the same failure, stop and report: what was tried, the logs, and your hypothesis.
4. Tick `- [x]` for a step or unit only after its evidence exists.
5. After all units pass, run the full regression command, check every `Acceptance` item with evidence, and save the walkthrough.

## Hard gate

- Never mark a task done without concrete runtime evidence (passing tests, build, clean logs). If an Acceptance item describes runtime behavior, run it and observe it.
- Never mask errors with dummy fallbacks, swallowed exceptions, broad catches, skipped or ignored tests, loosened assertions, or disabled checks. Fix root causes.
- Never edit a test to make it pass. Change tests only when a plan step says so.
- No scope expansion beyond the unit's `Files` and the plan spec.
- Without approval, plan edits are limited to ticking checkboxes and appending a `## Deviation Log` (date, step, what happened, what was decided).
- Do not, unless the plan explicitly says so: install dependencies, commit/push/reset/force-push, delete files outside `Files`, run migrations or destructive commands against any non-local environment, or print secrets into logs or the walkthrough. Commit only if the plan or the user asks.
- If a scope deviation, blocker, or contradiction with the approved design appears: stop, explain it, name the affected decision, update the plan, and get approval before continuing. Return to `brainstorming` if the design itself must change.

## Stop conditions

Stop and report (do not push through) when any of these occur:

- Anchor drift is material, or the plan conflicts with the code.
- A step needs a file outside the unit's `Files`.
- The same failure survives 3 fix attempts.
- A verification command in the plan is wrong or not runnable (report the exact error; do not substitute a weaker check silently).
- Baseline failures overlap the files the plan changes, so results cannot be attributed.
- An irreversible step has no rollback and no recorded approval.

## Locations

- Plan: `local/agents/plans/YYYY-MM-DD-<feature-name>.md` (or `...-fix<N>.md`)
- Walkthrough: `local/agents/walkthroughs/` with the **same filename as the plan** (so fix plans get `...-fix<N>.md` walkthroughs and never collide). Create the directory if missing.
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
# command executed, with the real output (trimmed to relevant lines) showing PASS and exit status
```

## Baseline vs Final
- Baseline: [result before changes]
- Final: [full regression result after changes]
- Pre-existing failures (untouched): [list or "None"]

## Key Decisions & Notes
- [Edge cases handled, constraints respected, anchor drift noted]

## Deviations
- [Entries from the Deviation Log, or "None"]

## Out-of-Scope Observations
- [Unrelated issues noticed but not touched, or "None"]

## Acceptance Checklist
- [x] [Item verified, with the command or observation that proved it]
````

## Decision rules

| Condition | Action |
| :--- | :--- |
| Plan not approved or has placeholders | Stop, return to `writing-plans` |
| Verification fails | Read full logs, fix root cause, re-verify |
| Same failure after 3 attempts | Stop and report with logs and hypothesis |
| All units pass | Run regression, check Acceptance, save walkthrough |
| Scope deviation needed | Pause, log it, update plan, request approval |
| Ambiguity or design conflict | Stop and clarify before editing |
| Pre-existing failure found | Record it, do not fix it |
| Resuming a partial run | Re-verify last completed unit, continue from first unchecked step |
