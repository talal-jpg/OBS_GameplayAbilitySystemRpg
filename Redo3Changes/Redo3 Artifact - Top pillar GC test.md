↑ [[Redo3 Artifacts - Hex Performance and Breaking]] · [[Redo3 Artifacts Index]]

# Top pillar GC test

claude.ai Artifact · report · [open the artifact](https://claude.ai/artifact/1YcBjoVBnpVsKK9v9YxBpH)

A test report (AuraRedo3, UE 5.8, StartupMap PIE, 25 Oct 2026) on breaking each collapsing top-platform tile into a Chaos Geometry Collection where the instance was, instead of spawning the skeletal mesh with baked RBD animation; bottom platforms keep the skeletal mesh. Version 3, after Talal's feedback, has no shrinking: pieces fall at full size, land on lower platforms and each pillar disappears whole 6 s after it spawns, like the tile prop debris. It explains the v1/v2 broken pieces (cut from an open render LOD1 with 1,424 border edges) and the v3 fix (cut from the closed source model), and the move from 12 GCs to 3 height variants. Frame, game-thread and GPU cost tables for v1, v2 and v3 show the GPU spike comes from Lumen re-capturing the ISM, not the pieces. It also documents the new UMyHexPillarBreakSubsystem, the knobs and console commands, and notes that nothing is committed.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- v3: 95 of 95 pillars disappear 6.00-6.05 s after spawning at full size; peak 50 of 64 active, 0 recycled.
- 3 GCs (GC_HexPillar_H030 / H070 / H160, Z/XY 0.30 / 0.70 / 1.60, 11-14 pieces, 20-26k tris, about 12 MB) replace 12 GCs of about 27 MB; heights picked by k-means over 2,073 top tiles (646 / 851 / 576 tiles).
- Leftover Z/XY scale at most 1.53x (mean 1.25x); bExactTopPillarHeight puts it in the Z scale.
- v3 cost during a 3-chain break: 0-3 s falling +19.9 ms GPU, 3-6 s lying +5.1 ms game thread and +2.9 ms GPU, back to baseline after the 6 s pop-out.
- Removing 45 instances adds 14-16 ms GPU for about 2 s, almost all in LumenSceneUpdate, even with no effects; the pieces add about 1.5-2 ms.
- Stress test Aura.BreakTopPlatform 0 (all 303 tiles) gave a 369 ms game-thread frame, caused by uncapped tile-prop GCs from SpawnBrokenProp.
- UMyHexPillarBreakSubsystem queues breaks (cap 64, 2 spawns per frame), crumbles after 0.08 s, pushes pieces out only through open sides; GCs are cosmetic, local and never replicated.
- M_Pillar2 got 'Used with Geometry Collections' turned on; GC sources HexPillar_Src_H### simplified to 14k tris.

## Commits it documents

- `613707b` Add UMyHexPillarBreakSubsystem and height-matched top pillar GCs · [[Redo3 Hex Pillars and Breaking]]

## Sections

- Top pillar GC test
- Fall, land, then gone whole after 6 s
- The mesh errors: what was wrong, and the fix
- Three GCs and the leftover scale
- Stretching and drift: none seen, not proven absent
- Removal, landing, memory
- Falling onto the lower platform, then shrinking (v2)
- An 11-tile top platform breaking (first version)
- Cost at the moment of the swap
- Changes in the working tree
- Knobs and test commands
- Caveats and follow-ups

## Made by

- Thread: [[Redo3 Threads#Add far-break ISMs for distant tiles|Add far-break ISMs for distant tiles]]
- Explained on: [[Redo3 Hex Pillars and Breaking]]
- Group: [[Redo3 Artifacts - Hex Performance and Breaking]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Far-break ISMs]]
- [[Redo3 Artifact - PCG layout - nodes added for fog and snow]]
- [[Redo3 Artifact - Hex platform optimization - what changed]]
- [[Redo3 Artifact - Hex platforms - LODs vs switching instances off]]
- [[Redo3 Artifact - Hex Platform FX Layout - PCG Nodes Added]]
- [[Redo3 Artifact - Atmospherics comparison]]
