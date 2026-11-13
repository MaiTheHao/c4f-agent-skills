# Preflight and Resume

Preflight proves the plan is executable before any code changes. Skipping it turns a plan defect into a failed implementation.

## Preflight

1. Locate the plan in `local/agents/plans/`. If several match, ask which. A `-fix<N>` plan runs after its base plan and touches only the files it lists.
2. Reject the plan and send it back to `writing-plans` if it has `TODO`/`TBD`/placeholders, a non-empty open question, a cyclic `DependsOn`, a `Consumes` that differs from its producer's `Produces`, or a file owned by two units.
3. Verify prerequisites. Install nothing unlisted in `Rules`/`AffectedFiles`.
4. Run the `Baseline` command and record the result. Existing failures are pre-existing: do not fix them.
5. Anchor check: every `MODIFY`/`DELETE` path exists and every named symbol occurs. Minor line shifts with unchanged symbols are fine; missing, renamed, or semantically changed symbols mean stop.
6. Record unrelated working-tree changes and preserve them.
7. Create `local/agents/walkthroughs/<plan filename>` from `references/walkthrough-template.md`, with `Progress` listing every unit as pending.

## Resume

Read the walkthrough's `Progress`, re-run the last completed unit's `Verify`, then continue from the next unit. If the walkthrough is missing, restart at preflight.
