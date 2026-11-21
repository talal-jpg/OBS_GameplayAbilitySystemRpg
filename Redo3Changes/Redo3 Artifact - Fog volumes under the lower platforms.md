↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Fog volumes under the lower platforms

claude.ai Artifact · report · updated 2026-10-31 · [open the artifact](https://claude.ai/artifact/9Zk3rfWPuRXLDkL3jk7Nki)

A Claude Docs design proposal, not built, for giving each lower-chain hex platform its own Local Fog Volume. Each volume would have a flat top 3 m below that platform's tile top, hiding the pillar ends at every platform's own height so the global fog floor could go back to a softer setting. The doc explains that a UE 5.8 Local Fog Volume is a sphere (5 m times its largest scale) multiplied by its height layer, and gives the settings, colour source and neighbour clamp for each volume. It lists two engine defaults that would need changing in DefaultEngine.ini, and estimates about 0.3 ms extra GPU time in a typical view (up to 0.8 ms when crowded) on top of today's 0.73 ms of fog. It then compares a C++ route and a Blueprint route and lists next steps. No commit adds Local Fog Volumes: origin/redo3 has no LocalFogVolume code, and the thread went with the opaque fog fade from 'Hiding the bottom pillars in fog' (ac1b069).

## What it shows

- Status: nothing built; the expected cost is still to be measured. The middle fog setting stays, with its backup in Saved/ClaudeBackups/FogFloorMiddle_20261031T035520Z.
- Volume setup: centred on the footprint 3 m below the platform top. Height layer offset 0, density 1/m, about 1 m e-fold falloff; radial density 1/m (the volumetric voxel clamp).
- Radius is the footprint radius plus about 6 m: about 8 m for a single tile, 15 m for the median platform and 48 m for the biggest. A neighbour clamp keeps a high platform's bowl off lower neighbours (closest tile gap 8 m, median 12.5 m).
- Colour: albedo 0, with emissive set to the void colour (FogFadeSkyAbyss or a new FogFadeLFVColor) from the MPC. Risk: the horizon is about 1.7x brighter than looking down, and there may be a seam at 60 m where volumetric fog ends.
- Global floor: soften the second fog layer back to offset -1246.48, with FogFadeFog2 moved to -982.48 to match.
- Engine settings needing approval: r.LocalFogVolume.GlobalStartDistance 1000 -> about 100 cm, and r.LocalFogVolume.TileMaxInstanceCount 32 -> 64 (there are 29 lower platforms today).
- Cost estimate (editor PIE): upper chain 0.1-0.3 ms, typical lower platform 0.2-0.45 ms, crowded 0.4-0.8 ms; at full 1080p up to 1.1 ms. Today's fog is 0.73 ms (height 0.07, volumetric 0.66) of a 26.5 ms frame.
- Lower platforms on screen: median 4 / max 7 within 60 m, 8 / 17 within 120 m, 21 / 27 within 250 m.
- Build routes: C++ AMyHexPlatform::UpdateBottomFogVolume() after UpdateTileFootprint() in GenerateTileProps (around MyHexPlatform.cpp:4398), or Blueprint in BP_MyHexPlatformBottom's BeginPlay (preferred, to avoid C++ that other work has open).

## Sections

- Summary
- How each volume is set up
- Colour, and how it fits the fog you have
- Engine limits and cost
- Where it goes, and the next step

## Made by

- Thread: [[Redo3 Threads#Optimize the hex atmosphere effects|Optimize the hex atmosphere effects]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Hiding the bottom pillars in fog]]
- [[Redo3 Artifact - Fog fade captures]]
- [[Redo3 Artifact - Lower-chain clouds]]
- [[Redo3 Artifact - Hex atmosphere costs]]
