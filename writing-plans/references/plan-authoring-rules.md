# Plan Authoring Rules

A plan fixes decisions the repo cannot answer. If a bullet needs a paragraph to explain _how_, delete the _how_ and keep the _what_.

## Writing rules

1. **No code.** Express intent, names, values, and tests in words.
2. **Minimal.** Use the fewest units and files that meet the success criteria. Do not impose an arbitrary line-count limit.
3. **Surgical.** List only files the work requires. Unrelated problems go to `Risks` as notes, never units.
4. **Verifiable.** Every Acceptance item has an observable `command -> expected` check.
5. **Carry assumptions as-is.** Do not re-decide the design. If the design is missing Goal, Success criteria, Assumptions, or Baseline, or contradicts itself, stop and ask.
6. **Decisions only.** Any name, business value, or public signature that would otherwise be guessed goes into `Spec`. Everything else stays out.
7. **No placeholders.** No `TODO`, `TBD`, "handle edge cases", or "add validation later".
8. **No secrets.** Reference env var names only.

## Allowed vs forbidden

**Allowed:**

- repo-relative paths and symbol names (`Class.method`)
- one-line signatures only under `Produces`/`Consumes`
- business values in words or numbers
- test names with `input -> expected`
- shell commands in `Verify`/`Baseline`
- Mermaid diagrams

**Forbidden:**

- code blocks (except Mermaid and commands)
- function bodies, pseudocode, assertion code, SQL, JSON/config snippets
- line ranges, copied code, long value tables
- per-line checkbox steps
- "run test -> FAIL then PASS" steps

## Size guidance

There is no maximum line count for a plan. Keep it as concise as the requirements allow, without sacrificing decisions, contracts, verification, or traceability.

Use these structural budgets as guidance:

| Item                 | Guideline                                           |
| :------------------- | :-------------------------------------------------- |
| Units / files        | Max 5 Units / approximately 15 files where feasible |
| Unit                 | Approximately 10 lines; `Spec` 2-5 bullets          |
| Tests                | Max 5 per Unit                                      |
| Rules / Review Focus | Max 5 / 3 one-liners                                |

These are structural guidelines, not reasons to omit necessary information or split a coherent plan artificially.

Split a plan into sequential plans only when the work represents genuinely independent implementation phases, not to satisfy a document-length threshold.
