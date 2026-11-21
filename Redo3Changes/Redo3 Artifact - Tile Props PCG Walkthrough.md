↑ [[Redo3 Artifacts - Hex PCG and Tile Props]] · [[Redo3 Artifacts Index]]

# Tile Props PCG Walkthrough

claude.ai Artifact · walkthrough · updated 2026-10-22 · [open the artifact](https://claude.ai/artifact/EHjsQsqahN8HLqRLxLPRxB)

A node-by-node walkthrough of PCG_HexTileProps3, the graph on BP_MyHexPlatform's HexTileProps2 component that places ice clusters, snow mounds and the walkway floor on every platform. It was read from the live graph on 22 Oct (122 nodes, 53 of them reroutes) without changing it. Eighteen numbered steps each list the nodes, the attributes they write and an SVG diagram. The steps run from the C++ Hex Platform Tile Scatter node, through the SizeK size factor, rim noise and the peak/ridge/skirt choice, the front row, the five hand-placed cluster patterns, Copy Points stamping, pruning and class sorting, to the per-class spawners, TileIndex in custom data 0 and the walkway floor. A 'Worth checking' box lists defects found: no flakes spawn, spire and shard jitter is large, AttributeMathsOp_1 has no second input, and the asset was untracked on redo3.

## What it shows

- Run chain: BeginPlay schedules ScheduleTileProps (after 3 s), then RebuildPathAndTileProps/BuildBridgePaths, GenerateLocal, and OnTilePropsGenerated, which builds TilePropISMs. When a tile collapses, HandleTileActivated calls BreakTileProps(TileIndex), which finds instances with custom data 0 = TileIndex, spawns PropBreakables[mesh] (a Geometry Collection) and removes the instances.
- UPCGHexPlatformTilesSettings (PCGHexPlatformTiles.cpp) outputs one point per tile with TileIndex, EdgeDepth, IsEdge and PathDistance, point scale HexRadius/100, and edge-tile points turned toward an open side (bFaceOutward). Its Path pin gives one point per walkway tile.
- SizeK = 0.6 + remap(sqrt(N), 3..18 -> 0..1): 0.6 for 9 tiles or fewer, 1.6 for 324 or more. Point scale = HexRadius/100 x SizeK, so 1.8 to 4.8 at HexRadius 300.
- Rim tiles have EdgeDepth 0 and PathDistance >= 1. World Perlin noise goes into $Density and tiles at <= 0.31 are dropped; above 0.72 a tile gets a peak, above 0.45 a ridge, otherwise a skirt, and tiles with PathDistance 1 always get a skirt. The front row keeps EdgeDepth 1 tiles with PathDistance >= 2 and Pick < 0.55, only on platforms with SizeK >= 0.82 (about 40 tiles).
- The five patterns are copied from the ice cluster in the L_Showcase lake: Peak 7 points, Ridge 6, Skirt 6, FrontHigh 3, FrontLow 3. Each piece's class code sits in $Density: 0.9 spire, 0.7 shard, 0.5 clump, 0.3 snow, 0.1 flake. Inheritance is TargetFirst, so every piece keeps its tile's TileIndex.
- Meshes and collision: SM_IceShard_01 (spire), SM_IceShard_02 (shard), SM_IceShardClump_01, SM_SnowMound_01 x3 / _02 x1 and SM_PathFloor_01..03 are BlockAll; SM_IceFlake_01/02 are NoCollision and never spawn. Each mesh has a GC_* entry in PropBreakables, and every spawner packs TileIndex into custom data float 0.
- Problems found: Jitter_Flake's output isn't wired to Spawn_Flake, so no flakes spawn. Jitter_Spire, Jitter_Shard and Jitter_Clump move pieces about +/-1.6 m sideways, and spires sink 0.7-1.1 m. AttributeMathsOp_1's InB is unwired. Nine dead or disconnected nodes remain.
- PCG_HexTileProps3 was untracked on redo3. Its committed copies were 79bed11 (ice shard crown) and f415ce4 (comment cleanup), both on 2026-10-18 on claude/project-thread-pca24g and later merged into redo3. Later graph edits went into 5ccdfef (path paving).

## Sections

- Worth checking
- 01 One point per tile
- 02 Measure the platform
- 03 Scale every tile point
- 04 Pick rim tiles off the walkway
- 05 Break the rim into runs and gaps
- 06 Choose peak, ridge or skirt
- 07 A sparse front row one tile in
- 08 The five cluster patterns
- 09 Stamp patterns on the tiles
- 10 Merge and prune
- 11 Sort pieces by class
- 12 Spires
- 13 Shards
- 14 Clumps and snow mounds
- 15 Flakes (not connected)
- 16 Tag every instance with its tile
- 17 Walkway floor
- 18 Dead and disconnected nodes
- Meshes and what they break into

## Made by

- Thread: [[Redo3 Threads#Hex PCG graph walkthroughs|Hex PCG graph walkthroughs]]
- Explained on: [[Redo3 Hex Tile Props]], [[Redo3 Hex Platform PCG]]
- Group: [[Redo3 Artifacts - Hex PCG and Tile Props]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Hex Platform PCG Walkthrough]]
