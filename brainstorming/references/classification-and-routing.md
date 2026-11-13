# Classification and Routing

Pick the lightest path that still protects the user from a wrong or irreversible change. Classify in the first line of your reply so the user can override.

## Paths

Size follows **impact** (behavior, interface, data, risk), not file or line counts. When in doubt, take the heavier path. Paths only upgrade: on hidden complexity, stop, say so, and step up.

| Path              | Signals                                                                                                                                                                                      | Output                                                  | Approve                           |
| :---------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------ | :-------------------------------- |
| **Trivial**       | No change to caller-visible behavior, interface, or data semantics; small, obvious, locally checkable; bug cause already verified. Docs/config edits qualify only when behavior is unchanged | Short plan + verify check                               | The exact request is the approval |
| **Spike**         | "can we / how would we" feasibility; result is an answer, not kept code                                                                                                                      | Question + probe plan, then findings                    | The probe                         |
| **Bounded**       | Scoped change to an existing flow; behavior changes but no new shared interface or data shape                                                                                                | Short design, **chat only**                             | The design                        |
| **Architectural** | New module/service, changed shared interface (2+ consumers), schema/persisted data, migration, auth/security, concurrency, cross-cutting concern                                             | Full design, saved after approval, then `writing-plans` | The design                        |

## Routing

If the request spans independent subsystems, split it and brainstorm one at a time.

- **Trivial:** answer or implement only what the exact request names. No design file, no plan file.
- **Spike:** state the question and the probe plan, get approval for the probe, run it in `local/agents/scratch/` (or `/tmp`), report findings, delete the probe. A spike produces an answer, not kept code.
- **Bounded:** present a short design in chat, get approval, implement test-first within the listed files, report evidence.
- **Architectural:** present the full design, get approval, save it, then hand off to `writing-plans`. Never go from design approval straight to implementation.

## Decision rules

| Condition                      | Action                                           |
| :----------------------------- | :----------------------------------------------- |
| Unsure between two paths       | Take the heavier                                 |
| Hidden complexity mid-work     | Stop, say so, upgrade path                       |
| Independent subsystems         | Split, one at a time                             |
| Ambiguous or conditional reply | Not approval; resolve open decisions             |
| Material change after approval | Re-approve first (Architectural: save `-rev<N>`) |
| Bug cause unverified           | Reproduce first; no fix design yet               |
| Design file exists             | Never overwrite; ask revision or replacement     |
