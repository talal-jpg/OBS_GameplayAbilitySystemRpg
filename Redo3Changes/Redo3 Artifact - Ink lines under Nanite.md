↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Ink Lines Under Nanite

claude.ai Artifact · report · [open the artifact](https://claude.ai/artifact/MKdcuZQ1vEsRdRBTzHUnM8)

A short report on step A of the hex ink pass (StartupMap PIE, 2026-11-05): the ink lines were lost under Nanite and are now back. The cause was each material's PixelDepthOffset dither going into Nanite's depth buffer, so WorldPosition rebuilt from that depth jittered and the crease and silhouette lines flooded or vanished. The fix switches the WorldPosition nodes feeding CN_InkFade, CN_InkCrease and CN_InkSilhouette to Camera Relative excluding material shader offsets, a property change only. A table lists the instruction counts of the four materials with UseOutline on and off, before and after. Five captures show ice shards, a snow mound, top pillars, the platform rim and a forced tile break.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- PixelDepthOffset dither values: ice 12, top pillar 4, snow 1; the 'Used with Nanite' flags were already on and not the cause.
- Fix: WorldPosition nodes for CN_InkFade, CN_InkCrease and CN_InkSilhouette now use 'Camera Relative, excluding material shader offsets'; no nodes added, deleted or rewired.
- HexPillar has Nanite on again with the same settings (fallback tris 27,674).
- UseOutline on: MM_Ice_Hex 1247/219, MM_SnowAssets_Hex 1131/219, M_Pillar2 2073/1025, M_Pillar2_GC 972/353 (PS/VS); off counts unchanged before and after.
- Backup: Saved/ClaudeBackups/InkNanite_20261105T050804Z/ (MANIFEST.tsv, 22 files).
- Break shot used console command Aura.BreakTopPlatform 0; a falling M_Pillar2_GC piece was not checked up close.

## Commits it documents

- `cf4ec00` Ice look V2.1: in-material ink outlines, iteration MIs, blue mist · [[Redo3 Hex Look and Atmosphere]]
- `fa0ee5e` Hex pillars: rock pillar bottoms, glacier ice and edge snow drifts · [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Look and Atmosphere]]

## Sections

- Cause
- Fix
- Not verified

## Made by

- Thread: [[Redo3 Threads#Fix and extend the hex ink lines|Fix and extend the hex ink lines]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Ink lines in the hex pillar materials]]
- [[Redo3 Artifact - Ice look comparison page]]
- [[Redo3 Artifact - Props outline section layout shots]]
- [[Redo3 Artifact - Bottom pillar ink first pass shots]]
- [[Redo3 Artifact - Bottom pillar rim line shots]]
- [[Redo3 Artifact - Bottom bridge ink shots]]
- [[Redo3 Artifact - Bottom pillar snow-edge line shots]]
