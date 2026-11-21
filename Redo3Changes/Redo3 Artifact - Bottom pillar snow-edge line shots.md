↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Bottom Pillar Snow Edge

claude.ai Artifact · gallery · [open the artifact](https://claude.ai/artifact/227oMbLbLvkANcD2wpWLNt)

The result of Talal's pick 'Under the snow edge' (2026-11-06 05:05 UTC) for the bottom pillars in M_SmallPillarLines and MI_SmallPillar_HexRock. A new (h) Snow edge line, with its own comment box, Custom node CN_InkSnowEdge, reroutes and parameters, draws navy ink on the rock just below the lower edge of the [SNOW] wall spill. It recomputes that edge for each open side from HexSnowDrift's own inputs through read-only taps, so it follows the snow's values, and measures depth in pixels so the line keeps its width at any distance. It is combined with the hidden rim line into CN_InkCombine.Edge and sits before the snow lerps and the fog. Six captures show the line at 5 m and 9 m, a zoom, the step C view and the top pillars' edge lines for reference.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Parameters: SnowEdgeWidthPx 8 (5 read thin, 10 read heavy), SnowEdgeLineAmount 1, FacetSilhouette 0 (flecks off); rim line still on but hidden.
- Inputs re-derived: open-side bits, local P/Z, the two noises, SnowApothem, SnowHangDepth and SnowEdgeMin; no [SNOW] node changed.
- The edge sits 3-34 mesh cm down, below where the sides-only mask stops (2.5 mesh cm).
- Instruction counts: UseOutline off 1757 / 343, on with rim line only 2041 / 361, on with snow edge 2303 / 361; the snow edge costs about 262 PS instructions.
- Backup Saved/ClaudeBackups/InkSnowEdge_20261106T042024Z/; shots and 5 / 10 px width tests in Saved/ClaudeTools/icelook/iter/shots/InkSnowEdge_var/.

## Commits it documents

- `fa0ee5e` Hex pillars: rock pillar bottoms, glacier ice and edge snow drifts · [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Look and Atmosphere]]

## Sections

- What changed

## Made by

- Thread: [[Redo3 Threads#Fix and extend the hex ink lines|Fix and extend the hex ink lines]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Ink lines in the hex pillar materials]]
- [[Redo3 Artifact - Ice look comparison page]]
- [[Redo3 Artifact - Ink lines under Nanite]]
- [[Redo3 Artifact - Props outline section layout shots]]
- [[Redo3 Artifact - Bottom pillar ink first pass shots]]
- [[Redo3 Artifact - Bottom pillar rim line shots]]
- [[Redo3 Artifact - Bottom bridge ink shots]]
