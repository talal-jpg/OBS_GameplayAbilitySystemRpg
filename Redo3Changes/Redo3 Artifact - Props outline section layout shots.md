↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Props Outline Reorg

claude.ai Artifact · report · [open the artifact](https://claude.ai/artifact/L5CsgTcWRGMrUXb69GXLFw)

A before-and-after page for step B of the hex ink pass (2026-11-06 03:04 UTC): the Outline section of MM_Ice_Hex and MM_SnowAssets_Hex was laid out like the pillar material M_Pillar2. The outer Outline box grew to the pillar's 3180 x 2870 and a new '(c) Hex edges: none on props' box now holds the Edge constant 0 that feeds CN_InkCombine.Edge. The UseOutline switch and the root node were moved to the right of the section, as in the pillar. Nothing functional changed: no node was added or deleted and no link changed, and the instruction counts are the same before and after. Five pictures drawn offline from the saved .uasset files show M_Pillar2 as the reference and each prop material before and after.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Outer Outline box resized to 3180 x 2870; new (c) box at the pillar's OL_Edge spot labelled 'no hex edges on props'.
- (f)'s own PreSkinnedPosition and VertexInterpolator are labelled inside (f); the root node sits 400 units right of the UseOutline switch.
- MM_Ice_Hex: UseOutline on 1247 / 219 / 12 (PS / VS / tex), off 1045 / 200 / 12.
- MM_SnowAssets_Hex: on 1131 / 219 / 9, off 930 / 200 / 9; FrozenNorse boxes untouched.
- Backup: Saved/ClaudeBackups/InkB_20261106T030355Z/; each saved file was decoded again and every position, link, box and the root matched the plan.

## Commits it documents

- `cf4ec00` Ice look V2.1: in-material ink outlines, iteration MIs, blue mist · [[Redo3 Hex Look and Atmosphere]]

## Sections

- What changed
- M_Pillar2 reference layout
- MM_Ice_Hex before / after
- MM_SnowAssets_Hex before / after

## Made by

- Thread: [[Redo3 Threads#Fix and extend the hex ink lines|Fix and extend the hex ink lines]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Ink lines in the hex pillar materials]]
- [[Redo3 Artifact - Ice look comparison page]]
- [[Redo3 Artifact - Ink lines under Nanite]]
- [[Redo3 Artifact - Bottom pillar ink first pass shots]]
- [[Redo3 Artifact - Bottom pillar rim line shots]]
- [[Redo3 Artifact - Bottom bridge ink shots]]
- [[Redo3 Artifact - Bottom pillar snow-edge line shots]]
