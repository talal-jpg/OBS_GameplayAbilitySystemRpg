↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Bottom Pillar Ink

claude.ai Artifact · gallery · [open the artifact](https://claude.ai/artifact/7NvBEuPhahqSz1jgRM2pYq)

A shot page for step C of the hex ink pass (2026-11-06 03:40 UTC): M_SmallPillarLines, the bottom-pillar material, got the V2.1 Outline section with the same boxes and named reroutes as the top pillars. Ink is applied after the [GLACIER]/[ICE] results and before the [SNOW] lerps, so snow covers the lines and the fog stays on top, and a new (g) side mask keeps the ink on the sides only. The MI_SmallPillar_HexRock values start from MI_Pillar2_Bottom_V2_1 with navy ink, FacetSilhouette 0.5 and SilhouetteWidth 6. The page explains why the lines can't match the top pillars yet: SM_HexRockPillar has sharp edges between its slabs, so crease and smooth-silhouette terms find nothing and only facet strokes draw. Five captures show the final values, the start values, the clean snowy top, a live-section test and an offline drawing of the new graph section.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Instruction counts for M_SmallPillarLines: before 1757 / 343, UseOutline on 1962 / 361, off 1757 / 343 (PS / VS).
- (g) side mask = 1 - R_flat_top x InkSidesOnly multiplies OutlineAmount; there is no (c) hex-edge part because the carved mesh has broken corners.
- The three ink WorldPosition nodes use camera-relative, excluding material shader offsets, from the start (the Nanite fix from step A).
- MI changes: InkMultiply 0, FacetSilhouette 0.5, SilhouetteWidth 6; the rest are V2.1 values (InkDarken 0.48, CreaseStrength 3, FadeStart/End 500/1000 m, FarWidthScale 2.5).
- SM_HexRockPillar was built with set_sharp_from_angle in build.py, so vertex normals equal face normals; real outlines need smoothed normals or a rim-only hex-edge line.
- Backup Saved/ClaudeBackups/InkC_20261106T030534Z/; shots in Saved/ClaudeTools/icelook/iter/shots/InkC_var/.

## Commits it documents

- `fa0ee5e` Hex pillars: rock pillar bottoms, glacier ice and edge snow drifts · [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Look and Atmosphere]]

## Sections

- What changed
- Values (MI_SmallPillar_HexRock)

## Made by

- Thread: [[Redo3 Threads#Fix and extend the hex ink lines|Fix and extend the hex ink lines]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Ink lines in the hex pillar materials]]
- [[Redo3 Artifact - Ice look comparison page]]
- [[Redo3 Artifact - Ink lines under Nanite]]
- [[Redo3 Artifact - Props outline section layout shots]]
- [[Redo3 Artifact - Bottom pillar rim line shots]]
- [[Redo3 Artifact - Bottom bridge ink shots]]
- [[Redo3 Artifact - Bottom pillar snow-edge line shots]]
