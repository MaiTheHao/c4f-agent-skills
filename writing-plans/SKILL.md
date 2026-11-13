---
name: writing-plans
description: Use when the user requests an implementation plan or needs agreed requirements/design converted into executable work units. Produce a concise, decision-only, verifiable plan. Never code.
---

# Writing Plans

Rules for writing a plan: short, decision-only, verifiable. A plan fixes what cannot be guessed from the repo: files, cross-unit names, business values, and the tests that prove each unit. It is not the code. Write the plan in the user's language; keep keys and field names in English.

## Hard rules

- **No code, decisions only.** Names, business values, and public signatures go in `Spec`; everything else stays out.
- **Minimal and surgical.** Use the fewest units and files that meet the success criteria, and list only files the work requires. Unrelated problems go to `Risks` as notes, never units.
- **Verifiable.** Every Acceptance item has an observable `command -> expected` check.
- **Carry the design as-is.** Do not re-decide it. If Goal, Success Criteria, Assumptions, or Baseline is missing, or the design contradicts itself, stop and ask.
- **No placeholders, no secrets.** No `TODO`, `TBD`, "handle edge cases", or "add validation later"; reference env var names only.

Full rules, allowed/forbidden content, and size budgets: `references/plan-authoring-rules.md`.

## Flow

1. Start from an approved design or a requirement that already has Goal, Success Criteria, Assumptions, and Baseline.
2. Map files: one responsibility per file, follow existing patterns.
3. Anchor check: every `MODIFY`/`DELETE` path exists; every named symbol occurs (grep only names the plan contains). Fix mismatches before writing.
4. Fix cross-unit names, then group files into units.
5. Write each unit's `Tests` from Acceptance items and Review Focus.
6. Run the self-review checklist; fix inline.

Partitioning, `Spec`, `Tests`, and contracts: `references/partitioning-and-contracts.md`.
Verify commands, Review Focus, and self-review: `references/verification-and-review.md`.

## Location

- Plan: `local/agents/plans/YYYY-MM-DD-<feature-name>.md` (kebab-case)
- Fix plan: `local/agents/plans/YYYY-MM-DD-<feature-name>-fix<N>.md`, containing only the files that change
- Repo-relative paths only.

Template: `references/plan-template.md`. Keep section and field names exactly; never put code in any field.
