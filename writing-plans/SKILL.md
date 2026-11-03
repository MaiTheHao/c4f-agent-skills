---
name: writing-plans
description: Use when an approved design or clear spec needs a multi-step implementation plan before touching code, or when the user asks for a plan.
---

# Writing Plans

Turn an approved design into a **short, readable, verifiable plan**. This skill is the single source of truth for plan format.

A plan decides only what the executor cannot decide alone: which files, which names that cross units, which business values, which tests prove each unit. **It is not the code.** The executor reads the repo and writes the code from intent + test.

Design approval (from `brainstorming`) permits only this skill. Plan approval permits `executing-plans`.

Write the plan in the user's language. Keep keys and field names in English.

## Principles

1. **Human-gate first.** A human must be able to approve in one read. If it needs scrolling and re-reading, it is too long.
2. **No code.** Intent, names, and values in words. Never implementation.
3. **Minimal.** Fewest units and steps that meet the success criteria. No extras, no speculative config.
4. **Surgical.** List only files the work requires. Unrelated problems go to `Risks` as notes, never steps.
5. **Verifiable.** Every Acceptance item is observable and has a `command -> expected` check.
6. **Don't re-decide.** Carry the design's assumptions as-is. Design gap or contradiction -> stop, return to `brainstorming`.
7. **No open decisions.** If the executor would have to guess a business value or a name, put it in the plan. Everything else is the executor's job.

## What a plan may and may not contain

**Allowed**

- Repo-relative file paths and symbol names (`Class.method`) as anchors.
- One-line signatures, only under `Produces` / `Consumes` (cross-unit contracts).
- Business values in plain words or numbers ("priority 1..100", "only when status is New").
- Test names + one-phrase `input -> expected`.
- Shell commands in `Verify` and `Baseline`.
- The Mermaid diagram.

**Forbidden**

- Code blocks (except Mermaid and `Verify`/`Baseline` commands).
- Function bodies, pseudocode, assertion code, SQL/JSON/config snippets.
- Line ranges, copied existing code, long lists of values (e.g. i18n key tables).
- Multi-sentence logic inside one step.
- Re-running steps ("run test -> FAIL", "run test -> PASS"). `executing-plans` already does red/green.

If a step needs more than one sentence to explain _how_, delete the _how_; keep the _what_.

## Size budget (hard)

- Whole plan: about 120 lines. Over ~150 means cut or split.
- Step: one sentence, max ~25 words, one action.
- Unit: max ~10 lines. At most 5 units, about 15 files; otherwise split into sequential plans.
- `Needs Your Attention`: max 3 bullets, one line each.
- `Rules`: max 5 one-line bullets.
- `Review Focus`: max 3 items, one line each.

## Workflow

1. Read the approved design; read code only to fill gaps. Confirm it has Goal, Success criteria, Assumptions, Baseline. Missing -> return to `brainstorming`.
2. Map files: one responsibility per file, follow existing patterns.
3. Fix cross-unit names, then group into TaskUnits (rules below) and draw the diagram.
4. Add `Review Focus` and pin each item to a test step.
5. Self-review (checklist below), fix inline.
6. Present the **approval card** in chat (format below). Nothing is saved yet.
7. On approval: save with `Status: APPROVED`, reply with the path, hand off to `executing-plans`.

## Approval card (what the human reads at the gate)

Show this in chat, not the full plan. Max ~15 lines:

```
Plan: <name>
Goal: <one sentence>
Units:
  U1 <name> - <one line: what it delivers>   [files: n]
  U2 ...                                     [depends: U1]
Files: <N> (new a / modify b / delete c)
Needs your attention: <max 3 one-liners, or "none">
Verify: <how we know it's done, one line>
Approve this plan, or what should change? (Recommendation: approve)
```

Silence or conditional replies are not approval. User asks for changes -> update the draft and show the card again.

## Hard gate

- Write no code during planning. The only file ever written is the plan, and only after approval.
- Wait for explicit approval before saving and before any execution.
- No placeholders (`TODO`, `TBD`, "handle edge cases", "add validation later").
- Every plan embeds a valid Mermaid diagram.
- `Open Questions` is resolved with the user before approval. A saved plan says `None`.
- No secrets or credentials; reference env var names.
- Never overwrite an existing plan file. If the path exists, ask: revision (`-fix<N>`) or replacement.
- After approval the plan is frozen. Only `executing-plans` may tick checkboxes or append a `Deviation Log`.

## Partitioning

- A unit is the smallest piece with its own test cycle that a reviewer could reject independently. Fold setup, config, docs into the unit that needs them.
- Unit `Files` are disjoint; every `AffectedFiles` entry belongs to exactly one unit.
- `DependsOn` is `none` or unit IDs, acyclic, only for real ordering. Never split just to create parallelism.
- A cross-unit signature is declared once in the producer's `Produces`; consumers copy it exactly into `Consumes`.
- A unit's `Verify` must pass with only its `DependsOn` units done.
- No step touches a file outside its unit's `Files`.

## Steps

- One sentence: **file or symbol + what changes + the business value if one is pinned.**
- Test steps name the test and say `input -> expected` in plain words. Test-first for behavior.
- Non-behavioral change (config, docs, rename, move, delete): mark `(no-test: reason)`.
- Irreversible or data-touching step: give a rollback command, or list it under `Needs Your Attention` as irreversible.

Good: `BangChanDoanPage` - save only when the slip is in status New; otherwise show the "only editable when New" warning.
Bad: a method body, a try/catch outline, or "call X, then Y, then loop over Z".

## Verify rules

- `command -> expected`, non-interactive, deterministic (exit code or matched line). Never "looks correct".
- The last unit or `Acceptance` includes the full regression command from the Baseline.
- If an Acceptance item describes runtime behavior, verify it by running it, not only by unit tests. If it cannot run here, say so in `Risks`.

## Review Focus

Up to 3 inputs or failure modes the design implies but no Acceptance item covers (empty input, duplicates, boundary, partial failure). One line each: `input -> expected behavior`. Pin each to a test step tagged `(RF<n>)`. Empty is fine if none found.

## Locations

- Plan: `local/agents/plans/YYYY-MM-DD-<feature-name>.md` (kebab-case)
- Fix plan: `local/agents/plans/YYYY-MM-DD-<feature-name>-fix<N>.md`, only files that need changes
- Create the directory if missing. Repo-relative paths only.

## Template

USE TEMPLATE DEFINITION: references/template.md

## Self-review

1. **Coverage:** every success criterion maps to a unit and an Acceptance item.
2. **No code:** no code blocks beyond Mermaid and commands; no bodies, pseudocode, or line ranges.
3. **One-sentence steps:** each step <= ~25 words, one action, no _how_.
4. **Partitioning:** files disjoint, `DependsOn` acyclic, <= 5 units, steps stay in unit `Files`.
5. **Contracts:** every `Consumes` matches its producer's `Produces` exactly.
6. **Verification:** every AC names a verifying unit; every `Verify` is runnable `command -> expected`; regression command present; every behavioral step is test-first or `no-test` with a reason.
7. **Gate-readable:** the approval card fits ~15 lines; the plan is ~120 lines.
8. **Header:** Baseline present; `Open Questions` is `None`.

## Decision rules

| Condition                                            | Action                                                                           |
| :--------------------------------------------------- | :------------------------------------------------------------------------------- |
| Step contains code, pseudocode, or a "how" paragraph | Cut to intent before presenting                                                  |
| Plan over ~150 lines                                 | Cut, or split into sequential plans                                              |
| `Open Questions` not empty                           | Ask the user, merge the answer                                                   |
| Design gap or contradiction                          | Stop, return to `brainstorming`                                                  |
| More than 5 units or ~15 files                       | Split into sequential plans                                                      |
| Independent review returns `CHANGES_REQUIRED`        | Fix, re-review; after 3 rounds escalate to the user                              |
| Plan file exists                                     | Never overwrite; ask revision vs replacement                                     |
| User approves                                        | Save with `Status: APPROVED`, reply with the path, hand off to `executing-plans` |
| User requests changes                                | Update the draft, show the card again                                            |
