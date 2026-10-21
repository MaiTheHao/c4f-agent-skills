---
name: executing-plans
description: Execute, resume, or continue an approved plan from `local/agents/plans/`, task by task with TDD, runtime verification, strict scope control, and a saved walkthrough. Use whenever the user asks to run or implement a saved plan.
---

# Executing Plans

The approved plan is the execution contract. Only an explicitly approved plan authorizes execution.

## 1. Preflight

1. Locate the plan in `local/agents/plans/`. If multiple plans match, ask which one.
2. Require `Status: APPROVED`, `Open Questions: None`, and no unresolved `TODO`/`TBD`. Otherwise stop and return to `writing-plans`.
3. Verify prerequisites and environment. Do not install or upgrade unlisted dependencies.
4. Run and record the baseline command. Treat existing failures as pre-existing; do not fix them.
5. Validate every `MODIFY`/`DELETE` anchor. Minor line shifts with unchanged symbols are acceptable; missing, renamed, or semantically changed symbols require stopping.
6. Record unrelated working-tree changes and preserve them.

**Resume:** Find the first unchecked step and re-run the last completed unit's `Verify` before continuing.

## 2. Execution

- Execute units in dependency order and ID order. Parallelize only when supported and file scopes are disjoint.
- Before each unit, state the key assumption in one line. If ambiguous or inconsistent with the code, stop and clarify.
- For each testable step: write a failing test, confirm it fails for the expected reason, implement the minimum change, then confirm PASS.
- For `(no-test)` steps, apply the change and run the specified verification.
- Follow the unit's `Files` and existing code style. Remove only artifacts orphaned by your changes; leave unrelated dead code untouched.
- Run every unit's `Verify` command. On failure, inspect full logs, fix the root cause, and rerun verification.
- After 3 failed fix attempts for the same failure, stop and report attempts, logs, and the likely cause.
- Check off a step only after its verification evidence exists.

## 3. Hard Constraints

- **Evidence:** Never claim completion without actual command output or observed runtime behavior. Verify every acceptance criterion.
- **Tests:** Never weaken assertions, skip tests, suppress errors, add dummy fallbacks, or change tests to force a pass. Modify tests only when explicitly required by the plan.
- **Scope:** Do not modify files outside the current unit's `Files`. Stop if additional files are required.
- **Plan edits:** Without approval, modify the plan only to check completed boxes and append a dated `## Deviation Log` entry.
- **Side effects:** Unless explicitly specified by the plan or requested by the user, do not install dependencies, commit, push, reset, force-push, delete out-of-scope files, or run migrations/destructive commands against non-local environments.
- **Secrets:** Never expose secrets in logs or walkthroughs.
- **Deviations:** If a blocker, scope deviation, or design contradiction arises, stop, document the affected decision, update the plan as permitted, and obtain approval before proceeding. Return to `brainstorming` if the design itself must change.

## 4. Stop Conditions

Stop and report when:
- The plan is unapproved, incomplete, ambiguous, or conflicts with the code.
- Anchor drift is material or a required file falls outside the unit scope.
- A verification command is invalid or unrunnable. Report its exact error; do not silently substitute a weaker check.
- The same failure persists after 3 fix attempts.
- Baseline failures overlap modified files and prevent reliable attribution.
- An irreversible action lacks rollback or explicit approval.

## 5. Completion

After all units pass:
1. Run the full regression command.
2. Verify every acceptance criterion with concrete evidence.
3. Save the walkthrough at `local/agents/walkthroughs/` using the exact plan filename.
4. Include relevant pre-existing failures and unrelated issues without fixing them.

Create the walkthrough directory if needed. Use repository-relative paths.

## 6. Walkthrough Template

Save as Markdown using the same filename as the plan.

# [Feature Name] Walkthrough

## Summary
[Goal and outcome]

## Changes Made
| File | Status | Description |
|---|---|---|
| `path/to/file` | CREATED / MODIFIED / DELETED | [Change] |

## Verification Evidence
Record each command actually executed, relevant output, and exit status.

```bash
# Actual command and relevant output
```

## Baseline vs Final
- Baseline: [Result]
- Final regression: [Result]
- Pre-existing failures: [List or None]

## Key Decisions & Notes
- [Edge cases, constraints, anchor drift]

## Deviations
- [Deviation log entries or None]

## Out-of-Scope Observations
- [Unrelated findings or None]

## Acceptance Checklist
- [x] [Criterion and evidence]
