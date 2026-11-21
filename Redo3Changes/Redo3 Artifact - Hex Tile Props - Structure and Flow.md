↑ [[Redo3 Artifacts - Hex PCG and Tile Props]] · [[Redo3 Artifacts Index]]

# Hex Tile Props: Structure and Flow

claude.ai Artifact · walkthrough · [open the artifact](https://claude.ai/code/artifact/3fba40ce-fd8d-4925-988b-bc7890d8a2ee)

A Claude Docs document dated 2026-10-17 describing the first hex tile props system, merged on redo3 as e71e986. Every hex platform scatters rocks, cliffs and ruined pillars on its own tiles with PCG, and when a tile collapses its props break into Geometry Collections. Three parts make it work: AMyHexPlatform in C++ (grid, per-tile FHexTileInfo cache, PCG component, TileIndex-to-ISM tracking), the C++ PCG node Hex Platform Tile Scatter, and the PCG_HexTileProps graph that picks positions, sizes and meshes and tags each instance with its tile. Everything runs locally and deterministically on each machine; nothing about the props is replicated. The doc covers the C++ structure, the five-section graph, the prop classes, assets and folders, tuning knobs, networking, gotchas and open items. Two diagrams are embedded: a mind map of 5 parts around the platform and a runtime flow for startup and tile collapse.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- FHexTileInfo fields: Coord (cube Q, R, S), TileIndex = (Q + GridSize) * (2 * GridSize + 1) + (R + GridSize) stored as an exact float for ISM custom data, LocalTopCenter, LocalBounds, Apothem, EdgeDepth (rings in from the outline), OpenSideBits.
- AMyHexPlatform additions: TilePropsPCG (UPCGComponent, GenerateOnDemand, not replicated), BuildTileInfos, GenerateTileProps, OnTilePropsGenerated, HandleTileActivated, BreakTileProps, SpawnBrokenProp, PropBreakables (20 mesh -> GC entries).
- Hex Platform Tile Scatter (UPCGHexPlatformTilesSettings) has no inputs, emits candidate points with TileIndex, EdgeDepth and IsEdge, scale HexRadius / 100, seeds from the PCG seed plus TileIndex.
- Graph sections: Tiles (4 candidates per tile, about 7 m Perlin patches, rim bonus 0.22 fading over 3 rings, keep $Density above 0.62), Classes (large above 0.88, 12% of huge rocks become ruins, medium/small split at 0.74), Size, Prune (large to small), Spawn (one Static Mesh Spawner per class, TileIndex packed into custom data 0).
- Classes: Cliffs SM_Cliff_01/02 (0.25-0.42), Huge rocks SM_HugeRock_01/02 and SM_Boulder_01-05 (0.2-0.36), Ruins SM_RuinedPillar (0.3-0.42), Medium SM_Rock_01/02/04 (0.6-1.2), Small SM_Rock_01-04 (0.3-0.65, no collision), Patches SM_RockPatch_01-03 (0.5-0.8).
- In game HexRadius is 200 so tiles are about 3.5 m across (1.75 m at the class default of 100).
- Assets under /Game/Assets/MyPlatform: Props/SMs, Props/GCs (Dataflow uniform Voronoi, 6-16 pieces), M_PropRock with MI_PropRock_Cliff/_Boulder/_Patch, T_PropRock_BC/_N, PCGs/PCG_HexTileProps; no dependency on the AncientTechCity pack.
- A new prop mesh needs a spawner entry, a GC and a PropBreakables entry, or it just disappears when its tile falls.
- Open items: rocks can land on a bridge's end tile and block the landing; the AncientTechCity pack has no materials and 10 truncated textures.

## Commits it documents

- `379dc3b` Hex platform tile props: PCG scatter of rocks/ruins, break into Geometry Collections on tile collapse · [[Redo3 Hex Tile Props]]
- `ab708f7` Hex tile props: prop meshes, fractured GCs, PCG_HexTileProps graph wired into BP_MyHexPlatform · [[Redo3 Hex Tile Props]]
- `ca33354` Hex tile props: consistent prop names and a laid-out PCG_HexTileProps · [[Redo3 Hex Tile Props]]
- `7f31267` Hex tile props: natural rock layout with AncientTechCity rocks · [[Redo3 Hex Tile Props]]

## Sections

- Overview
- Mind map
- Runtime flow
- C++ structure
- PCG_HexTileProps graph
- Assets and folders
- Tuning knobs
- Networking, gotchas and open items

## Made by

- Thread: [[Redo3 Threads#Rocks and ruins on hex tiles|Rocks and ruins on hex tiles]]
- Explained on: [[Redo3 Hex Tile Props]]
- Group: [[Redo3 Artifacts - Hex PCG and Tile Props]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Hex tile break - ActivateHexLocal to BreakTileProps]]
- [[Redo3 Artifact - Hex Tile Props 3 - Structure and Flow]]
