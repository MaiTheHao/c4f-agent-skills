# Approval and GRILL

The gate protects the user from implementation before agreement. Questions protect the design from silent guesses. Keep both minimal: ask only when the answer changes the work.

## Approval gate

Nothing is implemented until the gate opens.

- **Trivial:** an exact user request authorizes the change; no design approval. If the request is not exact, clarify only what changes the implementation.
- **Bounded and Architectural:** present the design in chat and wait for explicit approval.
- **Before approval:** only read, search, run read-only commands, research, diagram. No implementation files, scaffolding, or dependency installs. The design file is written only after approval. Spike probes go in `local/agents/scratch/` or `/tmp`, are reported as throwaway, and are deleted.
- **Opens** only on an unambiguous yes ("approved", "proceed", "yes, go"). Questions, silence, or "looks good, but..." keep it closed. An unanswered gate question never defaults to proceeding.
- **Scope:** approval covers only what was presented. A material change (new file, interface, data shape, dependency, dropped criterion) reopens the gate.

### Waiving the design

When the user says "skip design" or "just do it", ceremony is waived, never the gate.

- **Bounded:** show a short design anyway, then ask once.
- **Architectural:** name the specific risk of skipping, present the design anyway, then ask once.

## GRILL

Ask only when the answer can materially change the design and the repo or context cannot resolve it. One question at a time. State the decision and why it matters; give your recommendation. If the user may not answer immediately, state the safe assumption or say what is blocked.

Include trade-offs only when options really differ; use the shortest format that makes the decision clear.

## Gate question

Before asking, confirm there are no contradictions, TBDs, or two-way readings; resolve them first. Never ask while critical ambiguity remains.

Ask: "Approve this design, or what should change?" Recommendation: approve.

Silence is not approval. A conditional or ambiguous reply means the gate stays closed; resolve the open decisions first.
