# Partitioning and Contracts

Units are the execution and review boundaries. Good units can be rejected independently, and their contracts are fixed before any code is written.

## Partitioning

- A unit is the smallest piece with its own test cycle that can be rejected independently. Fold setup, config, and docs into the unit that needs them.
- Unit `Files` are disjoint; every `AffectedFiles` entry belongs to exactly one unit.
- `DependsOn` is `none` or acyclic unit IDs, only for real ordering. Never split just to create parallelism.
- A unit's `Verify` must pass with only its `DependsOn` units done.
- Parallel plans need disjoint `AffectedFiles`; if their verify commands write the same build output, state that they need separate worktrees.

## Spec

`Spec` is decisions only. Each bullet pins a name, a business value, a public signature, or an edge behavior the design fixed. No "how".

- Good: `BangChanDoanPage` saves only when slip status is New; otherwise show the "only editable when New" warning.
- Bad: a method body, a try/catch outline, or "call X, then loop over Z".

## Tests

- One line each: `testName`: input -> expected, in plain words.
- Tag review items `(RF<n>)`.
- Non-behavioral change (config, docs, rename, move, delete): `(no-test: reason)`.
- Irreversible or data-touching step: give a rollback command or note it in `Risks`.

## Contracts

A cross-unit signature is declared once in the producer's `Produces`; consumers copy it exactly into `Consumes`. Every `Consumes` must match its producer's `Produces` exactly; a mismatch blocks execution.
