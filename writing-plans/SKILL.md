---
name: writing-plans
description: Use when an approved design needs a plan before any worker executes it, or when the user asks for a plan. Produces a short, decision-only plan. Never code.
---

# Writing Plans

Turn an approved design into a short, verifiable plan. The plan decides what the worker cannot decide alone: which files, which names that cross units, which business values, which tests prove each unit. It is not the code. The worker reads the repo and writes the code from intent and tests.

Design approval (from `brainstorming`) permits only this skill. Plan approval permits execution by a worker. The orchestrator dispatches the worker. Write the plan in the user's language. Keep keys and field names in English.

## Principles

1. **Human-gate first.** A human must be able to approve in one read.
2. **No code.** Intent, names, values, and tests in words.
3. **Minimal.** Fewest units and files that meet the success criteria.
4. **Surgical.** List only files the work requires. Unrelated problems go to `Risks` as notes, never units.
5. **Verifiable.** Every Acceptance item is observable and has a `command -> expected` check.
6. **Don't re-decide.** Carry the design's assumptions as-is. A design gap or contradiction stops the work and returns to `brainstorming`.
7. **Decisions in the plan.** Any name, business value, or public signature the worker would otherwise guess goes into `Spec`. Everything else is the worker's job.

## What a plan may and may not contain

**Allowed**

- Repo-relative file paths and symbol names (`Class.method`) as anchors.
- One-line signatures, only under `Produces` / `Consumes`, the cross-unit contracts.
- Business values in plain words or numbers.
- Test names with `input -> expected` in plain words.
- Shell commands in `Verify` and `Baseline`.
- The Mermaid diagram.

**Forbidden**

- Code blocks, except Mermaid and `Verify`/`Baseline` commands.
- Function bodies, pseudocode, assertion code, SQL, JSON or config snippets.
- Line ranges, copied existing code, long value tables.
- Step lists with checkboxes per line. Units use `Spec`, `Tests`, and one `Done` checkbox.
- Re-running steps like "run test -> FAIL, then PASS". The worker already does red/green.

If a bullet needs a paragraph to explain _how_, delete the _how_ and keep the _what_.

## Size budget (hard)

- Whole plan: about 120 lines. Over about 150 means cut or split.
- Unit: max about 10 lines.
- `Spec`: 2-5 bullets per unit.
- `Tests`: max 5 items per unit.
- At most 5 units and about 15 files. Otherwise split into sequential plans.
- `Needs Your Attention`: max 3 one-liners.
- `Rules`: max 5 one-liners.
- `Review Focus`: max 3 items.

## Workflow

1. Read the approved design. Confirm it contains Goal, Success criteria, Assumptions, and Baseline. If any is missing, return to `brainstorming`.
2. Map files. One responsibility per file. Follow existing patterns.
3. **Anchor check.** For each `MODIFY` or `DELETE` path, confirm the file exists. For each symbol the plan names, confirm it occurs. Use grep only for names the plan contains. A mismatch means fix the plan before presenting it.
4. Fix cross-unit names, then group files into units. Check parallel safety (see Partitioning).
5. Write each unit's `Tests` from the Acceptance items and Review Focus.
6. Self-review (checklist below). Fix inline.
7. Present the **approval card** in chat (format below). Nothing is saved yet.
8. On approval, save with `Status: APPROVED`, reply with the path, and let the orchestrator dispatch the worker.

## Approval card

Show this in chat, not the full plan. Max about 15 lines:

```
Plan: <name>
Goal: <one sentence>
Units:
U1 <name> - <one line: what it delivers> [files: n]
U2 ... [depends: U1]
Files: <N> (new a / modify b / delete c)
Needs your attention: <max 3 one-liners, or “none”>
Verify: <how we know it’s done, one line>
Approve this plan, or what should change? (Recommendation: approve)
```


Silence or conditional replies are not approval. If the user asks for changes, update the draft and show the card again.

## Hard gate

- Write no code during planning. The only file written is the plan, and only after approval.
- Wait for explicit approval before saving.
- No placeholders: `TODO`, `TBD`, "handle edge cases", "add validation later".
- Every plan embeds a valid Mermaid diagram.
- `Open Questions` is resolved with the user before approval. A saved plan says `None`.
- No secrets or credentials. Reference env var names only.
- Never overwrite an existing plan. If the path exists, ask: revision (`-fix<N>`) or replacement.
- After approval the plan is frozen. Only the worker may tick `Done` or append a `Deviation Log`.

## Partitioning

- A unit is the smallest piece with its own test cycle that a reviewer could reject independently. Fold setup, config, and docs into the unit that needs them.
- Unit `Files` are disjoint. Every `AffectedFiles` entry belongs to exactly one unit.
- `DependsOn` is `none` or unit IDs, acyclic, used only for real ordering. Never split just to create parallelism.
- A cross-unit signature is declared once in the producer's `Produces`. Consumers copy it exactly into `Consumes`.
- A unit's `Verify` must pass with only its `DependsOn` units done.
- Parallel plans: `AffectedFiles` must be disjoint across plans. If the plans' verify commands write to the same build output, they must run in separate worktrees. State this in the card's `Needs your attention` when relevant.

## Spec, Tests, Done

- **Spec** = decisions only. Each bullet pins one of: a name, a business value, a public signature (repeated from `Produces` if needed), or an edge behavior the design fixed. No "how".
- **Tests** = behavior to prove, one line each: `testName`: input -> expected, in plain words. Tag review items as `(RF<n>)`. Test-first for behavior.
- Non-behavioral change (config, docs, rename, move, delete): `Tests: (no-test: reason)`.
- Irreversible or data-touching step: give a rollback command, or list it under `Needs Your Attention`.
- `Done` is a single checkbox per unit. The worker ticks it only after `Verify` evidence exists.

Good Spec: `BangChanDoanPage` save only when the slip status is New; otherwise show the "only editable when New" warning.
Bad Spec: a method body, a try/catch outline, or "call X, then loop over Z".

## Verify rules

- `command -> expected`, non-interactive, deterministic: exit code or a matched line. Never "looks correct".
- The last unit or `Acceptance` includes the full regression command from the Baseline.
- If an Acceptance item describes runtime behavior, verify it by running it, not only by unit tests. If it cannot run here, say so in `Risks`.

## Review Focus

Up to 3 inputs or failure modes that the design implies and no Acceptance item covers (empty input, duplicates, boundary, partial failure). One line each: `input -> expected behavior`. Pin each to a test as `(RF<n>)`. Empty is fine if none are found.

## Locations

- Plan: `local/agents/plans/YYYY-MM-DD-<feature-name>.md` (kebab-case)
- Fix plan: `local/agents/plans/YYYY-MM-DD-<feature-name>-fix<N>.md`, containing only the files that change
- Create the directory if missing. Repo-relative paths only.

## Template

USE TEMPLATE DEFINITION: `references/template.md`

## Self-review

1. **Coverage:** every success criterion maps to a unit and an Acceptance item.
2. **No code:** no code blocks beyond Mermaid and commands; no bodies, pseudocode, or line ranges.
3. **Decisions only:** each `Spec` bullet is a decision, not a procedure; each bullet is 2 lines or fewer.
4. **Partitioning:** files disjoint, `DependsOn` acyclic, at most 5 units, parallel plans disjoint.
5. **Contracts:** every `Consumes` matches its producer's `Produces` exactly.
6. **Anchors:** every MODIFY/DELETE path exists and every named symbol was confirmed.
7. **Verification:** every AC names a verifying unit; every `Verify` is runnable `command -> expected`; regression command present; every behavioral unit has `Tests`, or `(no-test: reason)`.
8. **Gate-readable:** approval card fits about 15 lines; plan about 120 lines.
9. **Header:** Baseline present; `Open Questions` is `None`.

## Decision rules

| Condition | Action |
|:--|:--|
| Spec bullet contains code, pseudocode, or a "how" paragraph | Cut to the decision before presenting |
| Plan over about 150 lines | Cut, or split into sequential plans |
| Anchor missing or ambiguous | Fix the plan before the card |
| `Open Questions` not empty | Ask the user, merge the answer |
| Design gap or contradiction | Stop, return to `brainstorming` |
| More than 5 units or about 15 files | Split into sequential plans |
| Parallel plans share build output | Separate worktrees, or serialize |
| Plan file exists | Never overwrite. Ask: revision or replacement |
| User approves | Save with `Status: APPROVED`, reply with the path, hand off for dispatch |
| User requests changes | Update the draft, show the card again |
| Worker returns `NEEDS_PLAN_UPDATE` | Revise the plan through this skill, get re-approval, then resume the same worker session |