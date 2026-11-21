↑ [[Redo3 Artifacts - Hex PCG and Tile Props]] · [[Redo3 Artifacts Index]]

# Hex tile break: ActivateHexLocal to BreakTileProps

claude.ai Artifact · walkthrough · [open the artifact](https://claude.ai/code/artifact/78ad00e7-ae30-4961-8f8d-7c5aa8a207e7)

A Claude Docs document dated 2026-10-20 explaining how a hex tile's rocks and ice props break when the tile collapses. ActivateHexLocal broadcasts OnHexTileActivated, HandleTileActivated turns the tile coordinate into a TileIndex, and BreakTileProps removes every PCG prop instance stamped with that TileIndex and spawns a Geometry Collection in its place. The link between a tile and its props is a number (MakeTileIndex) written into custom data slot 0 of each prop instance, not a pointer. Only the tile collapse (ActivatedTiles) is replicated; props, the TileIndex map and debris are rebuilt on each machine, and a dedicated server removes instances without spawning debris. The doc covers structure (members on AMyHexPlatform), a nine-step flow from the server's collapse decision to debris, networking and timing cases, edge cases and gotchas, and a code index with line numbers. Three diagrams are embedded: a mind map, a structure map and a server/client flow chart.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- MakeTileIndex(Coord) with G = GridSize: TileIndex = (X + G) x (2G + 1) + (Y + G); GridSize is replicated so every machine computes the same numbers.
- AMyHexPlatform members: PropTileIndexCustomDataSlot (default 0), TilePropISMs (TileIndex -> prop ISMs, built by OnTilePropsGenerated), PropBreakables (45 mesh -> Geometry Collection entries on BP_MyHexPlatform), HexTileProps2 PCG component running PCG_HexTileProps3, TileInfos, LocalActivatedTiles, replicated ActivatedTiles.
- Collapse flow: NotifyBridgesReady / NotifyLeadLanded -> TryStartCollapsing -> StartCollapsing; first chain after FirstCollapseDelay 0.5 s (20 s on platforms of 15 tiles or fewer); chains are random walks of up to 15 tiles, one tile every 0.1 s, next chain 5 s later at the lead down to 1.5 s far behind.
- ActivateHex (server) adds the tile to ActivatedTiles; clients follow via OnRep_ActivatedTiles -> ApplyActivatedTiles(true); ActivateHexLocal removes the pillar instance (RemoveHexInstance), updates HexMap and broadcasts OnHexTileActivated.
- BreakTileProps walks each ISM's instances last to first, spawns SpawnBrokenProp with the matching Geometry Collection at each world transform, then removes them in one RemoveInstances call; debris is an unreplicated AGeometryCollectionActor living BrokenPropLifeSpan 6 s with bCrumbleBrokenProps true.
- Props freeze (AreTilePropsFrozen) once generated and the path froze or any tile fell; first generation is PropsBridgeWait 3 s after BeginPlay, regeneration at least PropsRegenerateDelay 0.5 s after a bridge change; OnTilePropsGenerated runs a catch-up break without debris.
- Likely bug from reading engine source (untested): BrokenPropImpulse (0, 0, 150) cm/s probably never reaches the debris because SpawnActorDeferred registers the GC component and SetRestCollection builds the physics proxy before InitialLinearVelocity is set; damage thresholds have the same ordering problem.
- Code index (working tree 20 Oct 2026): MyHexPlatform.cpp ActivateHex :1444, ActivateHexLocal :1524 (broadcast :1585), MakeTileIndex :1954, HandleTileActivated :2379, BreakTileProps :2393, SpawnBrokenProp :2454; PCGHexPlatformTiles.cpp:221 writes the TileIndex point attribute.

## Commits it documents

- `379dc3b` Hex platform tile props: PCG scatter of rocks/ruins, break into Geometry Collections on tile collapse · [[Redo3 Hex Tile Props]]
- `6a10353` Hex platform: turn each pillar so +X faces away from the centre · [[Redo3 Hex Pillars and Breaking]]

## Sections

- Overview
- Structure
- Flow
- Networking and timing
- Edge cases and gotchas
- Where to look in the code

## Made by

- Thread: [[Redo3 Threads#Rocks and ruins on hex tiles|Rocks and ruins on hex tiles]]
- Explained on: [[Redo3 Hex Tile Props]]
- Group: [[Redo3 Artifacts - Hex PCG and Tile Props]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Hex Tile Props - Structure and Flow]]
- [[Redo3 Artifact - Hex Tile Props 3 - Structure and Flow]]
