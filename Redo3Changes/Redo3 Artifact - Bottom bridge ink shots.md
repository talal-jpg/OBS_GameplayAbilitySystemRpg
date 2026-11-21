↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Bottom Bridge Ink

claude.ai Artifact · gallery · [open the artifact](https://claude.ai/artifact/Qmun4KS5XTtdxXx3XxEDy1)

A shot page for step D of the hex ink pass (2026-11-06 03:40 UTC): ink outlines were added to the bottom (stone) bridge materials MM_Cliffs_HexBridge and MM_SnowAssets_HexBridge and their 7 instances. In MM_Cliffs_HexBridge the Outline sits between MF_Cliffs and the snow blend, so snow stays above the lines and the fog part is untouched; in MM_SnowAssets_HexBridge it hooks in before FF_AttrIn like the V2.1 prop snow material. The landing got a new MI_Cliffs_Tiles_HexBridge, applied through an override on PCG_Bridge2's Ends_Spawn entry, leaving the FrozenNorse asset untouched. The page lists the prop V2.1 ink values used on all MIs and the instruction counts before and after. Five captures show a bridge after and before the change, a span side, a snow pile and the landing.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- MM_Cliffs_HexBridge: before 937 / 200, UseOutline on 1136 / 219, off 937 / 200 (PS / VS).
- MM_SnowAssets_HexBridge: before 930 / 200, on 1131 / 219, off 930 / 200; MIs with FogFadeBelow on compile to 1265-1268 / 360 (cliffs) and 1185 / 360 (snow).
- MIs: MI_Cliffs_Asset01-05_HexBridge, MI_SnowPiles_HexBridge, MI_Cliffs_Tiles_HexBridge with OutlineAmount 1, InkMultiply 0.7, InkDarken 0.55, SilhouetteWidth 5, FacetSilhouette 0.45, ink #003E77.
- PCG_Bridge2 Ends_Spawn overrides its two slots with [MM_SnowAssets_HexBridge, MI_Cliffs_Tiles_HexBridge].
- GC debris and the shared torch material M_BridgeWood are untouched; no hex-edge part on bridges.
- Backup Saved/ClaudeBackups/InkD_20261106T033144Z/ (10 files).

## Commits it documents

- `ac1b069` Fog fade: bottom pillars and stone bridges fade into the void · [[Redo3 Hex Look and Atmosphere]]

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
- [[Redo3 Artifact - Bottom pillar snow-edge line shots]]
