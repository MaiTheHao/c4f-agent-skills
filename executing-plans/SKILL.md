---
name: executing-plans
description: Use when the user asks to run, resume, or continue a plan written with writing-plans. Executes units test-first, proves each with its Verify command, and records a walkthrough.
---

# Executing Plans

The plan is the execution contract. The user's request to execute a named plan is the authorization. The plan file is never edited; progress and deviations go in the walkthrough. Reply and write the walkthrough in the user's language; keep keys in English.

## Core flow

1. **Preflight.** Locate the plan, validate it, verify prerequisites, record **Baseline**, run the anchor check, and create the walkthrough with every unit pending. Details: `references/preflight-and-resume.md`.
2. **Execute** units in `DependsOn` order, then ID order. Test-first per unit, minimum change, then the unit's `Verify`. Details: `references/unit-execution.md`.
3. **Stop** on ambiguity, anchor drift, scope escape, an unrunnable check, or the same failure after 3 fix attempts. Report the blocker and wait. Details: `references/stop-recovery-and-completion.md`.
4. **Complete.** Run the full regression from `Acceptance`, verify every Acceptance item and Review Focus test, finish the walkthrough. Details: `references/stop-recovery-and-completion.md`.

## Hard constraints

- **Evidence:** never claim completion without real command output or observed runtime behavior.
- **Tests:** never weaken assertions, skip tests, suppress errors, add dummy fallbacks, or edit a test to force a pass.
- **Scope:** touch only the current unit's `Files` and nothing in `Out of Scope`. Stop if more files are needed.
- **Rules:** obey the plan's `Rules` section (version floors, no new deps, naming).
- **Side effects:** unless the plan or user says so, do not install dependencies, commit, push, reset, delete out-of-scope files, or run migrations or destructive commands against non-local environments.
- **Irreversible steps:** run only with the rollback noted in `Risks` or an explicit user go-ahead.
- **Secrets:** never expose them in logs or the walkthrough.

## Resume

Read the walkthrough's `Progress`, re-run the last completed unit's `Verify`, then continue from the next unit. If the walkthrough is missing, restart at preflight.

## Walkthrough

Path: `local/agents/walkthroughs/<same filename as the plan>`. Template: `references/walkthrough-template.md`.
