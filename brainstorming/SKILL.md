---
name: brainstorming
description: Use before non-trivial changes to behavior, interfaces, data, configuration, or architecture. Explore alternatives, constraints, and trade-offs; produce a validated design before implementation. Skip trivial exact edits.
---

# Brainstorming

Act as a senior technical partner. Produce a **validated design before implementation**. Prefer caution over speed; skip ceremony only for trivial tasks. Reply in the user's language; keep keys and field names in English.

## Principles

1. **Think first.** State assumptions. If several interpretations exist, list them; never pick silently. If unclear, ask: one question is cheaper than a wrong guess.
2. **Minimal.** No unrequested features, single-use abstractions, speculative config, or handling of impossible cases. Push back if a simpler approach exists. Treat the user's proposed solution as a hypothesis; check it fixes the root problem.
3. **Surgical.** Touch only what the request needs; follow existing patterns. Report unrelated problems; do not fix them.
4. **Verifiable.** Turn the task into success criteria that can be checked, never "seems to work".
5. **Evidence.** Claims about existing code cite `path:line` or command output. Tag every assumption: `[user]` (user said it), `[assumed]` (unverified), `[verified: path:line]`.

## Approval gate

Nothing is implemented until the gate opens.

- **Trivial:** an exact user request authorizes the change; no design approval. If the request is not exact, clarify only what changes the implementation.
- **Bounded and Architectural:** present the design in chat and wait for explicit approval.
- **Before approval:** only read, search, run read-only commands, research, diagram. No implementation files, scaffolding, or dependency installs. The design file is written only after approval. Spike probes go in `local/agents/scratch/` or `/tmp`, are reported as throwaway, and are deleted.
- **Opens** only on an unambiguous yes ("approved", "proceed", "yes, go"). Questions, silence, or "looks good, but..." keep it closed. An unanswered gate question never defaults to proceeding.
- **Scope:** approval covers only what was presented. A material change (new file, interface, data shape, dependency, dropped criterion) reopens the gate.
- **User waives design** ("skip design", "just do it"): ceremony is waived, never the gate. Bounded: show a short design anyway, then ask once. Architectural: name the specific risk of skipping, present the design anyway, then ask once.

Waivers, approval wording, and questioning rules: `references/approval-and-grill.md`.

## Workflow

State the path in your first line so the user can override. Size follows **impact** (behavior, interface, data, risk), not file or line counts. When in doubt, take the heavier path. Paths only upgrade.

| Step           | What you do                                                                 | Detail                                    |
| :------------- | :-------------------------------------------------------------------------- | :---------------------------------------- |
| 1. Classify    | Choose Trivial / Spike / Bounded / Architectural; split independent systems | `references/classification-and-routing.md` |
| 2. Inspect     | Read code, patterns, contracts, tests, config; record **Baseline**          | `references/design-method.md`             |
| 3. Align       | State Goal / Non-goals / Constraints / Success criteria / Assumptions       | `references/design-method.md`             |
| 4. Design      | Present one complete, concise design; compare real alternatives only        | `references/design-method.md`             |
| 5. Verify plan | Map every success criterion to `command -> expected`                        | `references/design-method.md`             |
| 6. Approve     | Resolve contradictions, then ask the gate question                          | `references/approval-and-grill.md`        |

## Artifacts

Only **Architectural** designs are saved, and only after approval; the design file is the only artifact written. Trivial, Spike, and Bounded stay in chat: no design file.

- Design: `local/agents/designs/YYYY-MM-DD-<feature-name>.md` (kebab-case)
- Revision: `local/agents/designs/YYYY-MM-DD-<feature-name>-rev<N>.md`, containing only the sections that change
- Spike scratch: `local/agents/scratch/`, deleted after the findings are reported

File rules and the design template: `references/design-artifact-template.md`.

## After approval

The approved design is the contract; never change it silently.

- **Spike:** report the recommendation, delete the probe.
- **Bounded:** no file. Implement test-first within the listed files only, run each verify check, report evidence in chat.
- **Architectural:** save the design as above, reply with the path, then invoke `writing-plans` with it. Never go from design approval straight to implementation.

If implementation reveals a new constraint or contradiction, stop, explain, name the affected decision, propose the change, and re-approve if material (Architectural: save a `-rev<N>`). Minor details need none.
