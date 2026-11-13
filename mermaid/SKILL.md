---
name: mermaid
description: Create, render, validate, or debug Mermaid diagrams. Output one valid fenced block; default to no styling.
---

# Mermaid

## Pick type

- Process/decisions → `flowchart`; messages over time → `sequenceDiagram`; types/inheritance → `classDiagram`; states → `stateDiagram-v2`; entities → `erDiagram`; schedule → `gantt`; git history → `gitGraph`; hierarchy → `mindmap`; chronology → `timeline`; 2×2 → `quadrantChart`; numeric trends → `xychart-beta`; layers → `block-beta`; flow quantities → `sankey-beta`; packets → `packet-beta`; infra/services → `architecture-beta`; task board → `kanban`; multi-axis → `radar-beta`; hierarchical proportions → `treemap`; system context → `C4Context`; UX stages → `journey`; requirements → `requirementDiagram`.
- Default to `flowchart` if uncertain.
- Architecture: logical deps → `flowchart`; infra → `architecture-beta`; system context → `C4Context`.
- Too vague → ask: process, structure, or timeline?
- `TD` for hierarchies/layers; `LR` for pipelines.

## Syntax pitfalls (models get these wrong)

- Fence with ` ```mermaid ` — never `~~~`.
- Node IDs: camelCase, no spaces. **Never use `end` as an unquoted ID.** Never start an ID with `o` or `x` (collides with `--o` / `--x` edge markers).
- `<br/>` for label line breaks — never `\n`.
- Quote labels with spaces or special chars: `A["Total (USD)"]`.
- Markdown labels: ``A["`**Bold**`"]``.
- Edge labels: `A -->|"Label"| B`.
- Valid edges only: `-->`, `---`, `-.->`, `==>`, `--o`, `--x`, `<-->`, `~~~`. **Never emit `-.-X`, `-.-x`, `-.-o`.**
- Subgraphs: ≤ 3 levels.
- Comments: `%% ...`.

## Styling

- Default: none. Omit `init`, `style`, `classDef`, `linkStyle`, inline colors. Host theme controls appearance.
- If styling requested: ask light vs dark theme. Never convey meaning by color alone.

## Output

Return exactly one ` ```mermaid ` block with only the diagram. At most one short caption outside. No prose unless requested.
