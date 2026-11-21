↑ [[Redo3 Artifacts - Hex PCG and Tile Props]] · [[Redo3 Artifacts Index]]

# Hex Tile Props 3: Structure and Flow

claude.ai Artifact · walkthrough · [open the artifact](https://claude.ai/code/artifact/3ee24f13-252e-4a40-8df8-d9147f276d0a)

A Claude Docs document dated 2026-10-18 describing PCG_HexTileProps3, the third hex tile props graph. It rings each hex platform's rim with ice-shard clusters copied from the L_Showcase lake, leaves the middle open and sizes the ice by the platform's tile count, running on BP_MyHexPlatform's HexTileProps2 PCG component in place of PCG_HexTileProps2, which is kept for rollback. It explains the project's custom Hex Platform Tile Scatter PCG node and its point attributes, then walks the graph's nine sections left to right (Tiles, Size, Rim, Front, Patterns, Copy, Clusters, Spawn, Path). It also lists the ice and snow mesh classes, assets, tuning knobs and build scripts, and known limits. Two diagrams are embedded: a structure overview and a runtime flow for startup and tile collapse.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- The scatter node is a project C++ class, UPCGHexPlatformTilesSettings (Source/Aura/.../PCG/PCGHexPlatformTiles), added in commit 379dc3b on 2026-10-16; 6444429 added its Path pin; stock palette tools cannot add it, so the graph keeps instance HexPlatformTiles_0.
- Point attributes: TileIndex (int32, packed into custom data 0), EdgeDepth, IsEdge, PathDistance (rings from the walkway); point scale HexRadius / 100 (3.0 at radius 300); one point per tile.
- Size section: SizeT = sqrt(N) mapped from 3..18 to 0..1 (9 tiles or fewer = 0, 324 or more = 1); SizeK = 0.6 + SizeT (0.6 to 1.6) multiplies each point's scale.
- Rim section: edge tiles not on the walkway are split by world noise into gaps, skirt, ridge and peak clusters; tall clusters need PathDistance 2 or more so bridge landings stay low. Front row only on platforms with SizeK 0.82 or more (about 40+ tiles).
- Patterns: five Create Points sets (Peak, Ridge, Skirt, FrontHigh, FrontLow) carry a class code in $Density: 0.9 spire, 0.7 shard, 0.5 clump, 0.3 snow, 0.1 flake.
- Meshes: SM_IceShard_01/02, SM_IceShardClump_01, SM_SnowMound_01 (weight 3)/02 (weight 1), SM_IceFlake_01/02 (no collision), copied from SM_Glaciar and SM_Snow_Pile; GCs use Voronoi fracture with 6 to 15 pieces and Nanite off; PropBreakables has 45 entries.
- Tuning knobs: Rim_KeepNoise 0.31, Rim_Noise scale 6, Rim_Peak 0.72, Rim_Ridge 0.45, Rim_OffPath 1, Rim_TallOK 2, Shard_Clear 2, Front_SizeGate 0.82, Front_Keep 0.55.
- Build scripts in Saved/ClaudeTools/icecliffs: props3_build.py + props3_params.json + props3_run.py rebuild the graph, props3_gc.py makes the GCs, props3_layout.py and run_layout2.py lay it out.
- Known limits: big clusters hang past the outer edge on larger platforms; platforms under about 20 tiles get only skirt clusters; late joiners can pick a different SizeK; meshes depend on the gitignored FrozenNorseEnvironment materials.

## Commits it documents

- `79bed11` Hex tile props 3: ice shard crown on the platform rim · [[Redo3 Hex Tile Props]]
- `f415ce4` PCG_HexTileProps3: remove stale comment boxes, update the description · [[Redo3 Hex Tile Props]]

## Sections

- Overview
- At a glance
- Runtime flow
- The Hex Platform Tile Scatter node
- Graph sections, left to right
- Assets
- Tuning knobs
- Known limits and open items

## Made by

- Thread: [[Redo3 Threads#Rocks and ruins on hex tiles|Rocks and ruins on hex tiles]]
- Explained on: [[Redo3 Hex Tile Props]]
- Group: [[Redo3 Artifacts - Hex PCG and Tile Props]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Hex tile break - ActivateHexLocal to BreakTileProps]]
- [[Redo3 Artifact - Hex Tile Props - Structure and Flow]]
