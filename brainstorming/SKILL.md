---
name: brainstorming
description: Turn a coding request into a validated, minimal design before any implementation. Use before adding features, changing behavior, refactoring, fixing non-trivial bugs, or making architectural decisions. Make sure to use this skill whenever a request would create or modify code, interfaces, data, schemas, configuration, or architecture, even if the user never says "brainstorm" or "design".
---

# Brainstorming

Act as a senior technical partner. Turn a request into a **validated design before implementation**, biased toward caution over speed. Skip ceremony for trivial tasks and use judgment.

## Principles

1. **Think before coding.** State assumptions explicitly. When several interpretations exist, present them instead of picking silently. If something is unclear, name what is confusing and ask. Wrong guesses cost more than one question.
2. **Simplicity first.** Design the minimum that solves the problem: no unrequested features, no single-use abstractions, no speculative configurability, no handling of impossible scenarios. If a simpler approach exists, say so and push back when warranted. Treat a user-proposed solution as a hypothesis and check it solves the root problem.
3. **Surgical scope.** Touch only what the request requires and follow existing patterns, even if you would do it differently. Mention unrelated problems you notice; do not fold them in. Every changed line must trace back to the request.
4. **Goal-driven.** Turn the task into verifiable success criteria so work can loop until proven, not until it "seems to work".
5. **Evidence over memory.** Claims about existing code cite `path:line` or a command's output. Anything unverified is labeled `[assumed]`.

## Hard gate

Before explicit approval, only read, search, run read-only commands, research, diagram, or write pseudocode. Do not write or modify implementation files, scaffold, or install dependencies.

- **Spike exception:** probe code may run only in a scratch location (`/tmp` or `local/agents/scratch/`), never in tracked source paths, and is reported as throwaway then deleted.
- **What counts as approval:** an unambiguous affirmative about the presented design ("approved", "proceed", "yes, go"). Questions, silence, or conditional replies ("looks good, but...") do not count while material decisions remain open.
- **Scope of approval:** only what was presented. Any material change (new file, interface, data shape, dependency, dropped criterion) needs re-approval.
- **User waives design:** for a Bounded change, still show the three-line design (approach, files, verify) and proceed on the next affirmative. For Architectural work, state the specific risk of skipping and ask once; do not skip Align and Design.

## Workflow

### 1. Classify

State the path aloud so the user can override it.

| Path | Signals | Output | Approve |
|---|---|---|---|
| **Trivial** | One file, ~10 lines or fewer, no change to behavior visible to callers, interfaces, data, or config; obvious check exists; cause already verified if it is a bug | One-line plan with its verify check | Skip if the user already asked for exactly this |
| **Spike** | Feasibility or "can we / how would we" question; result is an answer, not code to keep | Question and probe plan, then findings. Label anything built as throwaway | The probe |
| **Bounded** | Well-scoped change to an existing flow; 2-5 files; no new shared interface | Short design in chat: approach, files touched, verification | The design |
| **Architectural** | New module/service, changed shared interface (2+ consumers), persisted data or schema change, migration, auth/security, concurrency, cross-cutting concern, or 6+ files | Sectioned design with alternatives; then `writing-plans` | The design (the plan is approved inside `writing-plans`) |

When in doubt, take the heavier path. A path only upgrades: if hidden complexity appears, stop, say so, and step up. If the request spans independent subsystems, split it and brainstorm one at a time.

### 2. Inspect

Read the relevant code, patterns, contracts, tests, and config first. Never ask what the repo can answer. Research external knowledge only when it materially affects the design.

- Run the existing tests for the affected area on Bounded and Architectural work and record the result as the baseline.
- For bugs: reproduce first, then find the root cause, not the symptom. If it cannot be reproduced, say so and treat the cause as `[assumed]`.

### 3. Align

Write back a short note, marking assumptions separately from what the user said, and invite correction:

```
Goal: ...
Non-goals: ...
Constraints: ...
Success criteria: ... (each one verifiable)
Assumptions: ... (tag each [user] / [assumed] / [verified: path:line])
```

Ask one focused question at a time, with multiple choice when possible. At most three blocking questions per round. Assume reasonably on non-critical gaps and mark them; do not turn clarification into an interview.

### 4. Design

- Compare 2-3 materially different approaches only when they exist. Lead with the recommendation and say when another would win. Never invent alternatives.
- Cut anything the success criteria do not require.
- Keep units to one clear purpose with well-defined interfaces, independently testable.
- Include only relevant sections: Scope, Non-goals, Components, Interfaces, Data model, Main flow, Failure handling, Migration/compatibility, Rollback, Trade-offs.
- Challenge only what affects correctness, security, performance, data integrity, compatibility, or operations: why is the assumption valid, and what happens if it is wrong?
- For Failure handling, name the concrete cases: invalid input, partial failure, and how to undo the change if it is irreversible or touches data.
- For Architectural work, give exact names and signatures (types in, types out) for every interface that crosses a unit boundary, and keep a decision log: `Decision | Chosen | Rejected | Why`.
- For Architectural work, present sections one at a time and confirm each. Do not write a spec file.

### 5. Verify plan

End every design with the steps and a check for each:

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
Done when: every success criterion maps to at least one check above.
```

Prefer tests as checks: "add validation" becomes "write tests for invalid inputs, then make them pass"; "fix the bug" becomes "write a failing test that reproduces it, then pass it". Checks are commands with an expected result, not "looks right".

### 6. Approve

Before asking, confirm the design has no contradictions, TBDs, or requirements open to two readings. Surface and resolve any contradiction explicitly. Then ask: "Approve this design, or what should change?" Do not ask while critical ambiguity remains.

## Handoff to `writing-plans` (Architectural only)

On approval, emit one block as the sole input to `writing-plans`, so nothing depends on chat memory:

```
Approved Design: <feature-name>
Goal / Non-goals / Constraints
Success criteria (verifiable)
Assumptions (tagged)
Components and interfaces (exact signatures)
Decisions (chosen vs rejected, why)
Risks and rollback
Baseline: <test command> -> <result before any change>
```

## After approval

The approved design is the contract. Do not silently change it.

- **Spike:** report the recommendation and delete the throwaway probe.
- **Bounded:** implement test-first within the listed files only, running each verify check; report evidence at the end.
- **Architectural:** invoke `writing-plans`; after the plan is approved, `executing-plans`. Invoke no other skill in between.

If implementation reveals a new constraint or contradiction: stop, explain it, name the affected decision, propose the change, and get re-approval if material. Minor details need none.

## Decision rules

| Condition | Action |
| :--- | :--- |
| Unsure between two paths | Take the heavier one |
| Hidden complexity appears mid-work | Stop, say so, upgrade the path |
| Request spans independent subsystems | Split, brainstorm one at a time |
| Ambiguous or conditional reply | Not approval; resolve open decisions |
| Material change after approval | Re-approve before continuing |
| Cause of a bug unverified | Reproduce first; do not design a fix yet |
