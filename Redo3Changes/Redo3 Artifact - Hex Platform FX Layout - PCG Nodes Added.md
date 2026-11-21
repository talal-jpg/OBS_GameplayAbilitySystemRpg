↑ [[Redo3 Artifacts - Hex PCG and Tile Props]] · [[Redo3 Artifacts Index]]

# Hex Platform FX Layout - PCG Nodes Added

claude.ai Artifact · walkthrough · [open the artifact](https://claude.ai/artifact/UexDxPmyWaXUWpc7JFNGcP)

A Claude Docs document (dated 2026-10-24) listing the six new PCG node chains that place fog, mist, snow and dust around the hex platforms: five in PCG_HexTileProps3 and one (mist banks) in PCG_SplineIHexPlatformsFromSpline. Every chain is FX_<Name>_Pick (Random Choice) -> FX_<Name>_Place (Transform Points) -> FX_<Name>_Spawn (Spawn Actor of MyHexAtmosphereFX, whose System property holds the Niagara system). Each node is described with its wiring, seed and offsets plus a 'Visual' line, with counts from a PIE run on StartupMap (74 platforms). It also covers the C++ path-end fog wall on AMyHexPathSpawner, the six tuned Niagara copies in /Game/Assets/MyPlatform/VFX/ with their effect-type culling, and the shadow, distance-field and cull-distance changes to existing Static Mesh Spawners. A side-view diagram shows where each effect sits around one platform.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- PCG_HexTileProps3 chains: FX_Dust (NS_HexDust, Z +250 to +300 cm), FX_Snow (NS_HexSoftSnow, +1800 cm), FX_RimFog (NS_HexGroundFog, ratio 0.02), FX_RimGust (NS_HexSnowGust, ratio 0.007), FX_SideMist (NS_HexGroundFog at 1.5x, -1200 to -600 cm); 15 nodes at x 300-900, y 3500-4300.
- FX_MistBank in PCG_SplineIHexPlatformsFromSpline: ratio 0.35 (17 of 44 bottom platforms), offset up to 90 m sideways and 6-25 m below, replicated MyHexAtmosphereFXNet with NS_HexMistBank.
- SpawnActor_1 template changed from BP_MyHexPlatform to BP_MyHexPlatformBottom, so bottom-chain platforms build every tile from HexPillarBottom.
- Fog wall: NS_HexFogWall, count 4, spacing 5500 (layout, 110 m apart), Z offset -1000 cm, move delay 6 s, move speed 6000 cm/s.
- FXT_HexAtmosphereNear culls past 80 m, at most 6 of each system and 24 in total; FXT_HexAtmosphereFar 900 m, 40 at once.
- Niagara copies: NS_HexSoftSnow 120/s (was 500), NS_HexSnowGust 400/s in 15 m (was 5500 in 40 m); originals untouched, backups in /Game/Backups/2026-10-24/.
- Existing spawners (Spawn_Snow, Spawn_Flake, Path_Spawn, Depth_Spawn, Dress_RockSpawn, Dress_SnowSpawn, Lamps_StickSpawn) lost shadows and distance-field lighting and gained cull distances such as 25000/30000.
- Rim fog and side mist came to 181 actors on 74 platforms in the PIE run.

## Commits it documents

- `b5ff0ef` Hex platforms: bottom chain, atmosphere variants, GC and far breaks · [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Look and Atmosphere]], [[Redo3 Hex Platform PCG]]
- `3d05747` Hex optimisation: thinner bridge debris, lighter GCs, prop LODs · [[Redo3 Hex Bridges]], [[Redo3 Hex Tile Props]], [[Redo3 Hex Pillars and Breaking]]
- `ff2437c` Hex path PCG: floating small top platforms, bottom chain class · [[Redo3 Hex Platform PCG]]

## Sections

- Overview
- PCG_HexTileProps3, on and around each platform
- PCG_SplineIHexPlatformsFromSpline, mist beside and below the path
- Path-end fog wall and the FX actor
- The Niagara systems
- Existing nodes changed

## Made by

- Thread: [[Redo3 Threads#Add far-break ISMs for distant tiles|Add far-break ISMs for distant tiles]]
- Explained on: [[Redo3 Hex Platform PCG]], [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex PCG and Tile Props]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Far-break ISMs]]
- [[Redo3 Artifact - PCG layout - nodes added for fog and snow]]
- [[Redo3 Artifact - Hex platform optimization - what changed]]
- [[Redo3 Artifact - Hex platforms - LODs vs switching instances off]]
- [[Redo3 Artifact - Atmospherics comparison]]
- [[Redo3 Artifact - Top pillar GC test]]
