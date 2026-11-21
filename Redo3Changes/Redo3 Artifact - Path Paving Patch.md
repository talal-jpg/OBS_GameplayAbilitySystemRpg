↑ [[Redo3 Artifacts - Hex Path, Bridges and Paving]] · [[Redo3 Artifacts Index]]

# Path Paving Patch

claude.ai Artifact · patch notes · updated 2026-10-25 · [open the artifact](https://claude.ai/artifact/SsRdkqqLv5XdjYhbntoiWe)

The first bridge-path paving patch (the page heading reads 'Bridge path paving: C++ patch'): a unified diff against redo3 8d98fd7 for MyHexPlatform.h/.cpp and PCGHexPlatformTiles.h/.cpp, to be applied hunk by hunk. BuildBridgePaths now records each walk from a bridge tile as a route that stops where it joins an earlier one. A new BuildPathPaving fits a smoothed spline through each route's tile centres, cuts it by how much it bends, lays pattern copies of stones along it and halves stones that sit over a step. It then records one FHexPavingSlab per stone, on the tile under its centre, so the stone breaks with that tile. The Hex Platform Tile Scatter PCG node gets a Paving output pin that spawns slab meshes by attribute. All of this landed in 5ccdfef.

## What it shows

- New structs FHexPavingPatternSlab, FHexPavingPattern and FHexPavingSlab (Location, Yaw, Size, Tile, TileIndex, Pattern, Seed), plus GetPavingSlabs() and GetPavingCellSize() (PavingCellSize x HexRadius / 100).
- Three default 4x4 patterns are built in the AMyHexPlatform constructor (one big 2x2 stone each, plus long 2x1/1x2 and small 1x1 fillers). Each copy picks a pattern and maybe mirrors it from a deterministic hash.
- New settings (Hex Platform|Paving): bPaveBridgePaths, PavingCellSize 40, PavingCellsAcross 4, PavingPatternLength 4, PavingMaxBend 14 degrees, PavingSmoothing 10, PavingSmoothMaxOffset 0.75, PavingEdgeReach 0.92, PavingStepSplitHeight 4 cm, bDrawPavingDebug.
- HexPaving namespace: Directions, Hash, WorldToHex (cube rounding) and FRail (sampled spline with Eval, Turning and CorridorDistance). A later route stops where it runs into an earlier route's strip.
- Paved tiles join PathSet, so no props spawn on them. Debug draws the rail in red, cuts in cyan and stones in orange for 30 s.
- PCG Paving pin attributes: TileIndex, EdgeDepth, IsEdge, PathDistance, SlabShape (0 = 1x1, 1 = 2x1, 2 = 2x2, 3 = other), SlabSize, PavingPattern, Mesh (picked from SmallSlabMeshes / LongSlabMeshes / BigSlabMeshes).
- Per-stone jitter settings: PavingMeshCellSize 40, PavingZOffset -2, PavingHeightJitter 0.6, PavingYawJitter 1.5, PavingTiltJitter 0.6.

## Sections

- Bridge path paving: C++ patch (header)
- Unified diff

## Made by

- Thread: [[Redo3 Threads#Pave bridge paths with stone patterns|Pave bridge paths with stone patterns]]
- Explained on: [[Redo3 Path Paving]], [[Redo3 Hex Tile Props]]
- Group: [[Redo3 Artifacts - Hex Path, Bridges and Paving]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Paving Round 4 Patch]]
- [[Redo3 Artifact - Paving Round 3 Patch]]
- [[Redo3 Artifact - Plaza Paving Patch]]
- [[Redo3 Artifact - Bridge Path Paving in-game shots]]
