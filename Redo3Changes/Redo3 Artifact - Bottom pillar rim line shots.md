↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Bottom Pillar Rim Line

claude.ai Artifact · gallery · [open the artifact](https://claude.ai/artifact/NdJZ2eUM7jHjcpTKH8PgSR)

A follow-up to step C (2026-11-06 04:40 UTC) for M_SmallPillarLines and MI_SmallPillar_HexRock. The facet flecks were turned off (FacetSilhouette 0) as Talal asked, and a rim line was added using M_Pillar2's CN_InkHexEdge code on the exact hex top rim, open sides only. The rim line is built but does not show: the [SNOW] wall spill hangs 6-60 cm over it, and the sides-only mask (g) removes the band where a 5-8 px line would draw at 5-9 m. The page sets out three options for Talal without applying any: a snow-edge line (recommended), a thinner spill with an unmasked rim, or leaving it. Five captures show the close, 9 m and rim-height views and a test with the spill off.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Rim line parameters: RimLineAmount, EdgeWidthPx 6, PillarTopLocalZ 0, PillarApothemLocal 86.6025, with its own reroutes OL_MeshP and OL_Edge in the (c) box.
- Instruction counts: UseOutline on 2041 / 361 (was 1962), off 1757 / 343 unchanged; the rim line costs about 79 PS instructions.
- The remaining dark diamonds in the close view come from the material's own [ICE] line art, not the ink.
- Lowering the line does not help: below the rim the carved wall sits inside the exact hex; a 16 px line 120 cm down drew nothing.
- Options: snow-edge line under the spill (recommended), SnowHangDepth 6-8 instead of 25 with the rim line skipping mask (g), or leave RimLineAmount at 1 or 0.
- Backups Saved/ClaudeBackups/InkRim_20261106T034450Z/ and InkC_20261106T030534Z/; shots in Saved/ClaudeTools/icelook/iter/shots/InkRim_var/.

## Commits it documents

- `fa0ee5e` Hex pillars: rock pillar bottoms, glacier ice and edge snow drifts · [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Look and Atmosphere]]

## Sections

- Done
- Why the rim line does not show

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
- [[Redo3 Artifact - Bottom bridge ink shots]]
- [[Redo3 Artifact - Bottom pillar snow-edge line shots]]
