↑ [[Redo3 Artifacts - Hex Path, Bridges and Paving]] · [[Redo3 Artifacts Index]]

# Hex Path Setup Steps

claude.ai Artifact · guide · [open the artifact](https://claude.ai/artifact/Ek48vnAvDAjafyzAPBKCBD)

A nine-step setup checklist (tagged 'AuraRedo3 · steps.md') for wiring the growing hex path and bridge system into a level, in the order to do it. It covers cleaning up old MyHexBridgeBuilder actors, the platform Blueprint, a platform PCG graph that samples the segment spline, the segment Blueprint, a bridge PCG graph, the bridge Blueprint, the Hex Bridges project settings, placing the AMyHexPathSpawner, and testing as a listen server. Each step has tickable items whose state is saved only in the viewer's browser. The page mirrors steps.md at the project root, which was added in commit 17605e3. Asset names like BP_HexBridge are given as suggestions.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Delete any MyHexBridgeBuilder actors first or you get two sets of bridges; hand-placed platforms register with the bridge subsystem and get bridged too.
- Platform rough size is about 1.7 x GridSize x HexRadius from centre to grid edge, and edges usually sit around 1.3 x GridSize x HexRadius once the outline is trimmed.
- PCG_HexPlatforms: Get Spline Data (actor filter Self), Spline Sampler by Distance (for example 4000 cm), remove the last sampled point to avoid doubled platforms at segment joints, optional Transform Points, Spawn Actor BP_HexPlatform with no merging.
- BP_HexPathSegment (parent MyHexPathSegment) uses PCG_HexPlatforms with Generate On Demand; PCG_HexBridge samples BridgeSpline and spawns bridge meshes with collision on.
- BP_HexBridge (parent MyHexBridge): bDestroyWhenEndTileCollapses on by default, optional DestroyDelay, override Handle End Tile Collapsed or bind On End Tile Collapsed; GetEnds gives each end's platform, tile and location.
- NeighborRadiusMultiplier must exceed platform spacing / (GridSize x HexRadius): with 4000 cm spacing, GridSize 10 and HexRadius 100 that is 4, so use 5 (default 3 is too small); SpatialCellSize about 5000-10000; MaxBridgeLength 0 for unlimited.
- Path spawner: SegmentLength a multiple of spacing (4 x 4000 = 16000), PointsPerSegment 4, InitialSegments 2-3, ExtendDistance larger than SegmentLength (for example 20000), MaxYawDeviation 45-60, YawNoiseFrequency 0.0001 (a gentle bend about every 100 m).
- Test with Play as Listen Server and one client; look for 'LogHexBridgeSubsystem: Bridged N new platforms' and LogHexPath warnings; no bridges usually means NeighborRadiusMultiplier is too small or height noise breaks MaxSlopeDegrees.

## Commits it documents

- `17605e3` Add growing hex path, PCG platform segments and bridge subsystem · [[Redo3 Hex Path and Ranking]], [[Redo3 Hex Bridges]], [[Redo3 Hex Platform PCG]]

## Sections

- Clean up the level
- Platform Blueprint
- Platform PCG graph
- Segment Blueprint
- Bridge PCG graph
- Bridge Blueprint
- Project settings
- Path spawner
- Test

## Made by

- Thread: [[Redo3 Threads#Bring pinned artifacts into project|Bring pinned artifacts into project]]
- Explained on: [[Redo3 Hex Path and Ranking]], [[Redo3 Hex Bridges]]
- Group: [[Redo3 Artifacts - Hex Path, Bridges and Paving]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Hex Path and Bridges]]
- [[Redo3 Artifact - Aura Spell Menu Flow]]
