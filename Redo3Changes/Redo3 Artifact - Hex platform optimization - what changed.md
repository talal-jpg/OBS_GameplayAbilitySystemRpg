↑ [[Redo3 Artifacts - Hex Performance and Breaking]] · [[Redo3 Artifacts Index]]

# Hex platform optimization: what changed

claude.ai Artifact · patch notes · updated 2026-10-24 · [open the artifact](https://claude.ai/artifact/RfrQAUVQLySq3RjPCVhp8C)

A Claude Docs page listing what was changed on 24 Oct 2026 to speed up the hex platforms, and the measured result. In the same scene as that morning's playtest, GPU time fell from 60-68 ms to 33-36 ms a frame; on the user's own map (HexRadius 200, path about 400 m ahead) it was 26 ms, about 37 fps. Top and bottom platforms were split: a new child Blueprint, BP_MyHexPlatformBottom, sets bIsBottomPlatform, and SpawnActor_1 in PCG_SplineIHexPlatformsFromSpline spawns it. The page then lists eleven optimization steps, six tuned FrozenNorse Niagara copies for fog and snow, the backups, the code changes per file and what still needed checking. Nothing was committed at the time; the work later shipped in b5ff0ef and 3d05747.

## What it shows

- GPU pass times, morning vs now: shadows 12-14 ms to 3 ms, base pass 11.5-13 to 2.9, velocity 10 to 0.7, depth prepass 7 to 1.2. Draw calls fell from 4,219 to about 1,500. The like-for-like run used HexRadius 300 with the path 1.8 km ahead (170-190 platforms). The user's map runs 74 platforms with a 7.8 ms game thread and about 1,400 draw calls.
- Bottom-chain platforms build only HexPillarBottom tiles, with M_Pillar2 and one Z scale for every tile (HexPillarBottomScaleZ x XY scale, which is 2.0 at HexRadius 200). On collapse a bottom tile spawns HexPillarBottomSkeletalMesh playing BottomRbdAnimSeq; both were still unset, so bottom tiles just disappeared. A PIE check showed 44 bottom and 30 top platforms.
- LODs on all 23 platform, prop and bridge meshes, four levels each: HexPillar 4,782/2,390/1,196/478 triangles, HexPillarBottom 1,272/636/318/128, screen sizes 1/0.25/0.10/0.04. Nanite fallback at 10% for HexPillar and 15% for HexPillarBottom. Both pillar meshes got simple box collision.
- M_Pillar2's WPO no longer writes velocity, which took the velocity pass from 10 ms to 0.7 ms. Draw distances: platforms 1 km, tile props 600 m, bridges 800 m, and small props stop drawing per instance at 200-400 m. Torch point lights no longer cast shadows and stop drawing at 60 m with a 15 m fade.
- Stone bridge collapse spawns no debris for pieces more than 6 m below the deck or under 60 cm, with at most 24 pieces per stone bridge and 40 per wooden one. All 11 bridge GCs were rebuilt from *_Lite meshes in Props/Bridge2/GCSource.
- Code cleanup: the per-tile PrintString was removed, platforms no longer build their grid twice, fog actors are removed with their platform or segment, and bridge bDrawDebug was set to False. ExtendDistance was set to 20000 on the map's spawner.
- Fog and snow copies in /Game/Assets/MyPlatform/VFX: NS_HexFogWall (from NS_Mist01, 4 walls 165 m apart at the path end), NS_HexMistBank (35% of bottom-chain platforms, culled past 900 m), and NS_HexGroundFog, NS_HexDust, NS_HexSoftSnow and NS_HexSnowGust (placed by PCG_HexTileProps3, culled past 80 m). They are capped at 6 per system and 24 in total. Soft snow went from 500 to 120 particles a second and snow gusts from 5,500 to 400.
- Code changes: MyHexPlatform, MyHexBridge, MyHexPathSpawner (path-end fog banks), MyHexPathSegment, and the new MyHexAtmosphereFX (one Niagara system per actor for PCG to spawn, plus a replicated variant). Backups are in Saved/Backups/2026-10-24 (manifest.json with 42 entries) and /Game/Backups/2026-10-24.
- Still to check: hard-edged ground fog cards seen from inside, a session with a second client, and assigning the bottom collapse assets. Later commits: b5ff0ef (bottom chain, BP_MyHexPlatformBottom, MyHexAtmosphereFX) and 3d05747 (thinner bridge debris, lighter GCs, prop LODs).

## Sections

- Result
- Top and bottom platforms
- Optimization steps taken
- Fog and snow
- Backups and how to undo
- Code changes
- Still to check

## Made by

- Thread: [[Redo3 Threads#Add far-break ISMs for distant tiles|Add far-break ISMs for distant tiles]]
- Explained on: [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Platform PCG]], [[Redo3 Hex Look and Atmosphere]], [[Redo3 Hex Bridges]], [[Redo3 Hex Tile Props]]
- Group: [[Redo3 Artifacts - Hex Performance and Breaking]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Far-break ISMs]]
- [[Redo3 Artifact - PCG layout - nodes added for fog and snow]]
- [[Redo3 Artifact - Hex platforms - LODs vs switching instances off]]
- [[Redo3 Artifact - Hex Platform FX Layout - PCG Nodes Added]]
- [[Redo3 Artifact - Atmospherics comparison]]
- [[Redo3 Artifact - Top pillar GC test]]
