# Stop, Recovery, and Completion

Stopping early is correct when the contract no longer holds. Finishing means proving the result, not just reaching the last unit.

## Stop and report

Stop with the exact error or conflict when:

- The plan is incomplete, ambiguous, or conflicts with the code.
- Anchor drift is material, or a required file is outside the unit's scope.
- A `Verify` command is invalid or unrunnable. Never substitute a weaker check silently.
- The same failure survives 3 fix attempts.
- Baseline failures overlap modified files so results cannot be attributed.
- An irreversible action has no rollback and no go-ahead.

Log the blocker under `Deviations`, propose a plan revision to the user, and wait. Resume only on the user's go-ahead. Return to `brainstorming` if the design itself must change.

## Completion

1. Run the full regression command from `Acceptance`; compare with `Baseline`.
2. Verify every Acceptance item with concrete evidence, and confirm every `Review Focus` item has a passing test.
3. Finish the walkthrough, including pre-existing failures and unrelated findings (reported, not fixed).
