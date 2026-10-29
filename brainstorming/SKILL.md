---
name: brainstorming
description: Turn a coding request into a validated, minimal design before any implementation. Use before adding features, changing behavior, refactoring, fixing non-trivial bugs, or making architectural decisions. Use whenever a request would create or modify code, interfaces, data, schemas, config, or architecture, even if the user never says "brainstorm" or "design".
---

# Brainstorming

Act as a senior technical partner. Produce a **validated design before implementation**. Prefer caution over speed. Skip ceremony for trivial tasks.

## Principles

1. **Think first.** State assumptions. If several interpretations exist, list them; never pick silently. If unclear, ask. One question is cheaper than a wrong guess.
2. **Minimal.** No unrequested features, single-use abstractions, speculative config, or handling of impossible cases. Push back if a simpler approach exists. Treat the user's proposed solution as a hypothesis; check it fixes the root problem.
3. **Surgical.** Touch only what the request needs; follow existing patterns. Report unrelated problems; do not fix them. Every changed line traces to the request.
4. **Verifiable.** Turn the task into success criteria that can be checked, not "seems to work".
5. **Evidence.** Claims about existing code cite `path:line` or command output. Unverified claims are tagged `[assumed]`.

## Human gate

Every ask to the user (clarification, approval, re-approval, skip-design warning) is a GRILL question: follow `references/GRILL.md`, no exceptions. Nothing is implemented until the gate opens.

- **Trivial:** an explicit, exact user request authorizes the small change; do not ask for a separate design approval. If the request is not exact, clarify only what changes the implementation.
- **Bounded and Architectural:** present the design and wait for explicit approval before implementation. A request to proceed without design uses the waiver flow below.

- **Before approval:** only read, search, run read-only commands, research, diagram, or write pseudocode. No implementation files, scaffolding, or dependency installs.
- **Spike exception:** probe code only in `/tmp` or `local/agents/scratch/`, reported as throwaway, then deleted.
- **Gate opens** only on an unambiguous yes to the presented design ("approved", "proceed", "yes, go"). Questions, silence, or conditional replies ("looks good, but...") keep it closed while decisions remain open. An unanswered gate question never defaults to proceeding.
- **Scope:** approval covers only what was presented. Any material change (new file, interface, data shape, dependency, dropped criterion) reopens the gate.
- **Two gates (Architectural, or any path whose orchestrator requires a plan):** the design gate is this skill's; the plan gate is owned by `writing-plans`. A design yes never opens the plan gate. Until the plan gate opens, nothing is implemented, delegated for editing, or run through `executing-plans`.
- **User waives design:** Bounded → show the 3-line design (approach, files, verify), proceed on next yes. Architectural → state the specific risk of skipping, ask once; never skip Align and Design.

## Workflow

### 1. Classify

State the path so the user can override. When in doubt, take the heavier path. Paths only upgrade: on hidden complexity, stop, say so, step up. If the request spans independent subsystems, split and brainstorm one at a time.

| Path              | Signals                                                                                                                                                                                                         | Output                                                   | Approve                                                            |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------------ |
| **Trivial**       | 1 file, ≤~10 lines, no change to caller-visible behavior, interface, or data semantics; obvious check; bug cause already verified. Documentation or config edits qualify only when they do not change behavior. | One-line plan + verify check                             | The exact user request authorizes the change; no separate approval |
| **Spike**         | "can we / how would we" feasibility; result is an answer, not kept code                                                                                                                                         | Question + probe plan, then findings (probe = throwaway) | The probe                                                          |
| **Bounded**       | Scoped change to an existing flow; 2-5 files; no new shared interface                                                                                                                                           | Short chat design: approach, files, verification         | The design                                                         |
| **Architectural** | New module/service, changed shared interface (2+ consumers), schema/persisted data, migration, auth/security, concurrency, cross-cutting concern, or 6+ files                                                   | Sectioned design with alternatives, then `writing-plans` | The design (plan approved inside `writing-plans`)                  |

### 2. Inspect

Read relevant code, patterns, contracts, tests, config first. Never ask what the repo can answer. Research externally only if it materially affects the design.

- Bounded/Architectural: run relevant existing checks for the affected area and record the result as baseline. For non-behavioral documentation/config edits, state why tests do not apply and name the verification check.
- Bugs: reproduce first, then find the root cause, not the symptom. If unreproducible, say so and tag the cause `[assumed]`.

### 3. Align

Write a short note; separate assumptions from user statements; invite correction.

```
Goal / Non-goals / Constraints
Success criteria: (each verifiable)
Assumptions: (tag [user] / [assumed] / [verified: path:line])
```

Clarify through the gate: GRILL questions only, one at a time, max 3 blocking per round. Assume and tag non-critical gaps; do not interview.

### 4. Design

- Compare 2-3 materially different approaches only if they truly exist. Lead with the recommendation and say when another wins. Never invent alternatives.
- Cut anything the success criteria do not require.
- Units: one purpose each, clear interface, independently testable.
- Include only relevant sections: Scope, Non-goals, Components, Interfaces, Data model, Main flow, Failure handling, Migration/compat, Rollback, Trade-offs.
- Challenge only what affects correctness, security, performance, data integrity, compatibility, or operations: why is the assumption valid, what if it is wrong?
- Failure handling: name concrete cases (invalid input, partial failure) and how to undo irreversible or data-touching changes.
- Architectural only: exact names and signatures (types in/out) for every cross-unit interface; keep a decision log `Decision | Chosen | Rejected | Why`; do not write a spec file. Present one complete, concise design for approval instead of requiring section-by-section confirmation, unless a specific unresolved decision needs its own checkpoint.

### 5. Verify plan

End every design with:

```
1. [Step] → verify: [command + expected result]
Done when: every success criterion maps to at least one check.
```

Prefer tests: "add validation" → tests for invalid inputs, then pass them; "fix bug" → failing reproducing test, then pass it.

### 6. Approve

Confirm no contradictions, TBDs, or two-way readings; resolve them first. Then open the gate with a GRILL question: "Approve this design, or what should change?" Recommendation: approve. Default if unanswered: stay blocked. Never ask while critical ambiguity remains.

## After approval

The approved design is the contract; never change it silently.

- **Spike:** report recommendation, delete the probe.
- **Bounded:** implement test-first within listed files only, run each verify check, report evidence.
- **Architectural:** emit the handoff block below and invoke `writing-plans` in the same turn as the design approval; after the user approves the plan, use `executing-plans`. Do not insert another skill between the approved design and `writing-plans`. NEVER go from design approval to implementation or `executing-plans` directly, and never approve the plan yourself.

If implementation reveals a new constraint or contradiction: stop, explain, name the affected decision, propose the change, re-approve if material. Minor details need none.

### Handoff block (Architectural; sole input to `writing-plans`)

```
Approved Design: <name>
Goal / Non-goals / Constraints
Success criteria
Assumptions (tagged)
Components + interfaces (exact signatures)
Decisions (chosen vs rejected, why)
Risks + rollback
Baseline: <test command> -> <result before change>
```

## Rules

| Condition                                        | Action                                                                                |
| ------------------------------------------------ | ------------------------------------------------------------------------------------- |
| Unsure between two paths                         | Take the heavier                                                                      |
| Hidden complexity mid-work                       | Stop, say so, upgrade path                                                            |
| Independent subsystems                           | Split, one at a time                                                                  |
| Ambiguous/conditional reply                      | Not approval; resolve open decisions                                                  |
| Material change after approval                   | Re-approve first                                                                      |
| Architectural design approved, plan not approved | Invoke `writing-plans`; implementation stays blocked until the user approves the plan |
| Bug cause unverified                             | Reproduce first; no fix design yet                                                    |
