# Verification and Review

A plan is only as good as the evidence it demands. Every criterion needs a check a different person could run.

## Verify and Review Focus

- **Verify** is `command -> expected`: non-interactive, deterministic (exit code or matched line). Never "looks correct".
- Unit `Verify` is targeted; the full regression command from Baseline appears only in `Acceptance`.
- Runtime-behavior criteria are verified by running it, not only by unit tests. If it cannot run, say so in `Risks`.
- **Review Focus:** up to 3 inputs/failure modes the design implies and no Acceptance item covers (empty input, duplicates, boundary, partial failure), as `input -> expected`. Pin each to a test as `(RF<n>)`. Empty is fine.

## Self-review

Run this checklist before handing the plan over; fix issues inline.

1. **Coverage:** every success criterion maps to a unit and an Acceptance item.
2. **No code, decisions only:** nothing forbidden in `plan-authoring-rules.md`; every `Spec` bullet is a decision.
3. **Partitioning:** files disjoint, `DependsOn` acyclic, structural budgets considered.
4. **Contracts:** every `Consumes` matches its producer's `Produces` exactly.
5. **Anchors:** all `MODIFY`/`DELETE` paths and named symbols confirmed.
6. **Verification:** every AC names a verifying unit; every `Verify` is runnable; regression command present; every behavioral unit has `Tests` or `(no-test: reason)`.
7. **Header:** Baseline present; Mermaid diagram valid.
