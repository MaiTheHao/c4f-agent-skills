# Unit Execution

Execute one unit at a time. The plan fixes decisions; the repo supplies style and patterns.

## Execution

- Run units in `DependsOn` order, then ID order. Parallelize only when file scopes are disjoint and the environment supports it.
- Before each unit, state the key assumption in one line. If the plan is ambiguous or contradicts the code, stop and ask.
- Implement from `Spec` (decisions) and `Tests` (behavior). The plan has no code: read the repo and follow existing patterns and style.
- Testable unit: write each `Tests` item as a failing test (keep its `RF<n>` tag in the name or comment), confirm it fails for the expected reason, implement the minimum change, confirm it passes.
- `(no-test: reason)` unit: apply the change, then run `Verify`.
- Cross-unit contracts: implement `Produces` signatures exactly as written; consumers call them exactly as in `Consumes`.
- Touch only the unit's `Files`. Remove only artifacts your change orphaned.
- Run the unit's `Verify` and compare with its expected result. On failure read the full log, fix the root cause, rerun. After 3 failed attempts on the same failure, stop and report attempts, logs, and likely cause.
- Mark the unit done in `Progress` only after `Verify` evidence is recorded.

## Hard constraints

- **Evidence:** never claim completion without real command output or observed runtime behavior.
- **Tests:** never weaken assertions, skip tests, suppress errors, add dummy fallbacks, or edit a test to force a pass.
- **Scope:** do not modify files outside the current unit's `Files` or anything in `Out of Scope`. Stop if more files are needed.
- **Rules:** obey the plan's `Rules` section (version floors, no new deps, naming).
- **Side effects:** unless the plan or user says so, do not install dependencies, commit, push, reset, delete out-of-scope files, or run migrations or destructive commands against non-local environments.
- **Irreversible steps:** run only with the rollback noted in `Risks` or an explicit user go-ahead.
- **Secrets:** never expose them in logs or the walkthrough.
