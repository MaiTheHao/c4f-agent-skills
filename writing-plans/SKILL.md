---
name: writing-plans
description: "Use when an approved design from `brainstorming` (Architectural path) or a clear spec needs a multi-step implementation plan, before touching code. Defines the canonical plan format (AffectedFiles, TaskUnits, Acceptance) that reviewers check and `executing-plans` runs. Make sure to use this whenever a written plan is needed, even if the user does not say \"plan\"."
---

# Writing Plans

Turn an approved design into a minimal, self-sufficient, verifiable plan. This skill is the single source of truth for plan format: planners write to it, reviewers check against it, `executing-plans` runs it.

Design approval (from `brainstorming`) permits only this skill. Plan approval, obtained here, permits `executing-plans`.

## Principles

1. **Think before planning.** The approved design (Goal, Constraints, Success criteria, Assumptions) is the input. Carry its assumptions into the plan; do not re-decide the design. If the design has a gap or contradiction, stop and return to `brainstorming` instead of guessing. If a step has several readings, present them.
2. **Simplicity first.** Plan the fewest units and steps that meet the success criteria. No unrequested features, single-use abstractions, or speculative configurability. If the plan could be half as long, rewrite it.
3. **Surgical scope.** List only files the work requires and follow existing conventions. Record unrelated problems under `Risks` as notes, never as steps. Every `AffectedFiles` entry and step must trace to the Goal or an Acceptance item.
4. **Goal-driven.** Every Acceptance item is observable and covered by a `Verify` command with an expected result. Write tests first wherever behavior is testable.
5. **Executor-proof.** Assume the executor has no chat history and will not rediscover scope. If a decision is not in the plan, the executor must stop and ask, so leave none open.

## Workflow

1. Read the approved design; read code only to fill gaps. Confirm the design block has Goal, Success criteria, Assumptions, and a Baseline. If any is missing, return to `brainstorming`.
2. Fix exact files, signatures, and types. Anchor every existing-code reference with a symbol name and a line range.
3. Group work into TaskUnits per the partitioning rules and draw the diagram.
4. Run the self-review checklist and fix issues inline.
5. Present for approval in the calling agent's checkpoint format (default: AffectedFiles table, one line per unit, 3-6 key changes). Nothing is saved yet.
6. Save the approved plan with `Status: APPROVED` and hand off to `executing-plans`.

## Hard gate

- Write no code during planning. The only file ever written is the plan, and only after approval.
- Wait for explicit approval before saving and before any execution.
- No placeholders (`TODO`, `TBD`, "add validation later"). Every step carries explicit logic or commands.
- Every plan embeds a valid Mermaid diagram.
- Resolve `Open Questions` with the user before approval. A saved plan states `None`.
- Be self-sufficient: the executor will not rediscover scope. Use exact repo-relative paths, symbol names, and line ranges.
- Never put secrets, tokens, or real credentials in the plan; reference env var names instead.
- Never overwrite an existing plan file. If the path exists, ask whether this is a revision (new `-fix<N>` or suffixed name) or a replacement.
- After approval the plan is frozen. Only `executing-plans` may tick checkboxes or append a `Deviation Log`; any other change goes back through approval.

## Partitioning

- At most 5 TaskUnits; a small change may be one. If the work needs more than 5 units or about 15 files, split it into sequential plans, each with its own approval.
- Unit `Files` sets are disjoint, and every `AffectedFiles` entry belongs to exactly one unit.
- `DependsOn` is `none` or unit IDs, acyclic, used only for real ordering. Prefer units that can run in parallel, but never split a unit just to create parallelism.
- Units that share a signature declare it once under `Shared Interfaces`; the producing unit runs first.
- Every unit has a non-empty `Spec` and `Verify`; every file in `Files` appears in at least one step.
- No step touches a file outside its unit's `Files`.

## Step granularity

- 2-5 minutes each, independently testable.
- Name the exact file path, method signature with parameter and return types, and the logic.
- Write logic as precise prose or pseudocode (max ~15 lines). Full implementations are the executor's job.
- Test-first: failing test (name, inputs, expected result), then minimal logic, then verification.
- **No-test exception:** for non-behavioral changes (config, docs, rename, move, generated files, migration scripts) mark the step `(no-test: reason)` and give a verification command instead of a failing test. Behavioral code never gets this exception.
- Irreversible or data-touching steps (migration, delete, config in shared environments) include a rollback command or state `irreversible` under `User Review Required`.

## Verify rules

- `Verify` lists exact commands with expected results, in the form `command -> expected`.
- Commands are non-interactive and deterministic: exit code or a matched output line, never "looks correct".
- A unit's `Verify` must pass when only its `DependsOn` units are done.
- The final unit or `Acceptance` includes the full regression command from the design's Baseline, expected to match the baseline result except for intended changes.
- Runtime behavior (HTTP endpoint, CLI, job) is verified by running it, not only by unit tests, whenever an Acceptance item describes runtime behavior.

## Locations

- Plan: `local/agents/plans/YYYY-MM-DD-<feature-name>.md` (kebab-case)
- Fix plan (from a work review): `local/agents/plans/YYYY-MM-DD-<feature-name>-fix<N>.md`, containing only files that need changes
- Create the directory if missing. Use repo-relative paths inside the plan.

## Template

The builder parses `Goal`, `AffectedFiles`, `TaskUnits`, `Spec`, `Verify`, `Acceptance`, and the unit fields `Files` and `DependsOn`. Never rename or omit them.

````markdown
# [Feature Name] Implementation Plan

**Status:** APPROVED (YYYY-MM-DD)

## Goal
[One sentence]

## Scope
- In: [...]
- Out: [non-goals from the design]

## Architecture
[Approved approach in 2-4 lines]

```mermaid
graph TD
    A[Component A] --> B[Component B]
```

## Shared Interfaces
[Signatures used by 2+ units, or "None"]

## Assumptions
- [Carried from the approved design, marked as assumptions]

## Prerequisites
- Branch / dependencies / environment: [what must already be true]
- Baseline: `command` -> [result before any change, including known pre-existing failures]

## User Review Required
> [!IMPORTANT]
> [Breaking changes, irreversible steps, key decisions, risks needing approval, or "None"]

## Open Questions
None

## AffectedFiles
| File | Action | Why |
| :--- | :--- | :--- |
| `path/to/file` | NEW / MODIFY / DELETE | [reason tied to Goal or Acceptance] |

## Constraints
- [Limits the executor must respect]

## Conventions
- [Existing naming, style, patterns to follow]

## TaskUnits

### U1: [Component Name]
- **Files:** `path/to/file`, `path/to/other`
- **DependsOn:** none
- **Spec:**
  - [ ] **Step 1:** `path/to/file` (`symbol`, L10-40) - write failing test `name`: input -> expected
  - [ ] **Step 2:** `path/to/file` - implement `Type method(ParamType p)`: [logic]
  - [ ] **Step 3:** run verification and confirm PASS
- **Verify:** `command` -> [expected result]

## Acceptance
- [ ] AC1: [Observable, checkable criterion] (verified by U1)

## Risks
- [Risk and mitigation; unrelated issues noticed, as notes only]
````

## Self-review

- No placeholders or ambiguous wording.
- Partitioning rules hold (disjoint files, acyclic `DependsOn`, at most 5 units).
- Every `AffectedFiles` entry has a step and traces to the Goal or an Acceptance item; no step leaves its unit's `Files`.
- Nothing exceeds the design's success criteria (simplicity check).
- Signatures and types match across units sharing an interface and appear under `Shared Interfaces`.
- Every Acceptance item names the unit that verifies it and is covered by at least one `Verify`.
- Every `Verify` is a runnable `command -> expected`; the regression command is present.
- Every behavioral step is test-first; every `no-test` step states its reason.
- Anchors (symbol and line range) exist in the current code.
- The diagram is valid and matches the units.
- `Prerequisites` has a Baseline; `Open Questions` is `None`.

## Decision rules

| Condition | Action |
| :--- | :--- |
| Self-review finds ambiguity or placeholders | Fix inline before presenting |
| `Open Questions` not empty | Ask the user, merge answers, continue |
| Design gap or contradiction found | Stop, return to `brainstorming` |
| More than 5 units or about 15 files | Split into sequential plans |
| Independent review returns `CHANGES_REQUIRED` | Fix the draft, re-review; after 3 rounds, escalate to the user |
| Plan file already exists | Never overwrite; ask revision vs replacement |
| User approves | Save with `Status: APPROVED`, reply with the path, hand off to `executing-plans` |
| User requests changes | Update the draft, request approval again |
