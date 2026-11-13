# Design Method

The goal is a design the user can approve without guessing: verified context, explicit assumptions, one coherent recommendation, and checks that prove it.

## 1. Inspect

Read relevant code, patterns, contracts, tests, and config first. Never ask what the repo can answer. Research externally only if it materially affects the design. Greenfield: skip.

- Bounded/Architectural: run the relevant existing checks and record the result as **Baseline**. For non-behavioral docs/config edits, state why tests do not apply and name the verification check.
- Bugs: reproduce first, then find the root cause, not the symptom. If unreproducible, say so and tag the cause `[assumed]`; no fix design until the cause is verified.

## 2. Align

Write a short note that separates user statements from assumptions and invites correction:

```
Goal / Non-goals / Constraints
Success criteria: (each verifiable)
Assumptions: (tagged)
```

Clarify through GRILL: one question at a time. Assume and tag non-critical gaps; do not interview.

## 3. Design

Compare 2-3 materially different approaches only if they truly exist; lead with the recommendation and say when another wins. Never invent alternatives. Cut anything the success criteria do not require.

Challenge only what affects correctness, security, performance, data integrity, compatibility, or operations: why is the assumption valid, what if it is wrong? Name concrete failure cases (invalid input, partial failure) and how to undo irreversible or data-touching changes.

- **Bounded:** Approach, Files, Verify.
- **Architectural:** Components with exact names and signatures for every cross-unit interface, data model/main flow, failure handling/rollback, decision log `Decision | Chosen | Rejected | Why`, verify plan.

Present one complete, concise design for approval; add a checkpoint only for a specific unresolved decision.

## 4. Verify plan

End every design with:

```
1. [Step] -> verify: [command + expected result]
Done when: every success criterion maps to at least one check.
```

Prefer tests: "add validation" -> tests for invalid inputs, then pass them; "fix bug" -> failing reproducing test, then pass it.

## 5. After approval

The approved design is the contract; never change it silently.

- **Spike:** report the recommendation, delete the probe.
- **Bounded:** no file. Implement test-first within the listed files only, run each verify check, report evidence in chat.
- **Architectural:** save the design (`references/design-artifact-template.md`), reply with the path, then invoke `writing-plans` with it. Never go from design approval straight to implementation.

If implementation reveals a new constraint or contradiction: stop, explain, name the affected decision, propose the change, re-approve if material (Architectural: save a `-rev<N>`). Minor details need none.
