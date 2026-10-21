---
name: mermaid
description: Create, render, validate, or debug Mermaid diagrams for workflows, architecture, sequences, ERDs, timelines, and other visual models.
---

# Mermaid

## 1. Choose Diagram

Select by intent:

| Intent | Type |
|---|---|
| Process, decisions | `flowchart` |
| Messages over time | `sequenceDiagram` |
| Types, inheritance | `classDiagram` |
| States, transitions | `stateDiagram-v2` |
| Entities, relationships | `erDiagram` |
| Schedule, dependencies | `gantt` |
| Proportions | `pie` |
| Git history | `gitGraph` |
| Hierarchy, brainstorming | `mindmap` |
| Chronology | `timeline` |
| 2×2 prioritization | `quadrantChart` |
| Numeric trends | `xychart-beta` |
| Layered structure | `block-beta` |
| Flow quantities | `sankey-beta` |
| Packets, protocols | `packet-beta` |
| Infrastructure, services | `architecture-beta` |
| Task board | `kanban` |
| Multi-axis comparison | `radar-beta` |
| Hierarchical proportions | `treemap` |
| System context | `C4Context` |
| User experience stages | `journey` |
| Requirements traceability | `requirementDiagram` |

Rules:
- Default to `flowchart` if uncertain.
- Architecture: logical dependencies → `flowchart`; infrastructure → `architecture-beta`; system context → `C4Context`.
- If the request is too vague, ask whether it concerns a process, structure, or timeline.
- Use `TD` for hierarchies/layers and `LR` for pipelines. Other types use their native layouts.

## 2. Syntax

- Use fenced ` ```mermaid ` blocks, never `~~~`.
- Comments: `%% ...`. Use `<br/>` for label line breaks.
- Quote labels containing spaces or special characters: `A["Total (USD)"]`.
- Markdown labels: `` A["`**Bold** _italic_`"] ``.
- Use camelCase node IDs without spaces. Never use `end` as an unquoted ID or start IDs with `o` or `x`.
- Prefer `A -->|"Label"| B` for labeled edges.
- Valid edges include `-->`, `---`, `-.->`, `==>`, `--o`, `--x`, `<-->`, and `~~~`. Never emit `-.-X`, `-.-x`, or `-.-o`.
- Group related nodes with subgraphs; nesting ≤ 3 levels.

## 3. Styling

Default: no custom styling. Omit `init`, `style`, `classDef`, `linkStyle`, and inline colors unless explicitly requested. Let the host theme control appearance.

If styling is requested, ask whether the target IDE uses a light or dark theme. Never convey meaning through color alone.

## 4. Validate

Before output, verify:
- Correct diagram type and direction.
- Valid fences, node IDs, labels, and edge syntax.
- Special characters are quoted; line breaks use `<br/>`.
- No unnecessary styles, themes, or comments.

## 5. Output

Return one ` ```mermaid ` block containing only the diagram. Add at most one short caption outside it. Include no explanatory prose unless requested.