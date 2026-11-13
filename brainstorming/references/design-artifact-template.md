# Design Artifact and Template

Only **Architectural** designs are saved, and only after approval. The design file is the only artifact written. Trivial, Spike, and Bounded stay in chat.

## File rules

- Design: `local/agents/designs/YYYY-MM-DD-<feature-name>.md` (kebab-case)
- Revision: `local/agents/designs/YYYY-MM-DD-<feature-name>-rev<N>.md`, containing only the sections that change
- Spike scratch: `local/agents/scratch/`, deleted after the findings are reported
- Create the directory if missing. Repo-relative paths only.
- Never overwrite an existing design: if the path exists, ask: revision or replacement.
- No secrets or credentials; reference env var names only.
- Never put code in the design.

## Template

Keep section names exactly. `writing-plans` reads Goal, Success Criteria, Assumptions, and Baseline (from the base design, plus any `-rev<N>` on top).

```markdown
# [Feature Name] Design

**Approved:** YYYY-MM-DD

## Goal

[One sentence: the outcome.]

## Non-goals / Constraints

- [Non-goals and constraints]

## Success Criteria

- SC1: [verifiable criterion]

## Assumptions

- [user | assumed | verified: path:line] [assumption]

## Baseline

- `command` -> [result before any change, including known failures]

## Design

[Components + interfaces (exact names and signatures), data model / main flow, failure handling / rollback.]

## Decisions

| Decision | Chosen | Rejected | Why |
| :------- | :----- | :------- | :-- |

## Verify Plan

1. [Step] -> verify: `command` -> [expected]

## Risks

- [Risk and mitigation, rollback for irreversible steps]
```
