↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Fog fade captures

claude.ai Artifact · report · [open the artifact](https://claude.ai/artifact/KtMBGsGDbNymvjjytsZtZb)

A capture report (StartupMap lower chain, PIE 1920x1080, 30 and 31 October 2026) on making the lower chain's pillars and the stone bridges between lower platforms dissolve into the void below them. The first part covers the 31 October fog floor applied to StartupMap: ExponentialHeightFog_1's second fog layer was made dense below the lower chain and the general fog thinned, so the upper chain sees about twice as far. It measures pillar-end and bridge-keel errors and neighbour visibility for floor densities G06, G12 (applied) and G24, explains the trade-off forced by the engine's falloff cap of 2, and lists ways past it. The second part shows the 30 October material fade: a FogFadeBelow static switch in M_Pillar2 and the *_HexBridge materials that blends towards a void-matching target colour rebuilt by MF_FogFadeTarget. Each view is shot with the fade off, with the plain fog colour and with the shipped target, with pixel checks and shader costs.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Fog floor (applied, saved 03:36 UTC on 10-31): layer 1 density 0.02 -> 0.01, start distances 1500 -> 0, second fog density 0.15 -> 1.0, falloff 1.0 -> 2.0 (engine maximum), height offset -300 -> -746.48.
- G12 roughly halves pillar-end errors (view d 19 -> 9, h 16 -> 9, e 12 -> 6) while neighbours of the lowest platforms stay readable; G24 gets ends to 5 codes or less but loses neighbours at 15-35 m.
- Cost of the floor: about +0.1 ms (ComputeVolumetricFog 0.57-0.60 -> 0.65-0.66 ms).
- HexPillarBottom re-imported: 618.7 units long (12.4 m in the world, was 7.4 m), about 1.8x wider, 15,038 triangles at LOD0; MeshLengthLocal set to 618.7.
- Material fade: from 1.5 m below the tile top, fully gone at 5 m (FogFadeStart 150, FogFadeLength 350); only MI_Pillar2_Bottom_V2_1 turns it on.
- Bottom bridges: MM_Cliffs_HexBridge and MM_SnowAssets_HexBridge copies, six *_HexBridge MIs via PCG_Bridge2 overrides; BP_MyHexBridge2 writes each deck line into custom primitive data (slopes up to 9 m).
- MF_FogFadeTarget rebuilds the void colour (sky abyss at 1.185 x fog colour plus cyan volumetric in-scatter within 75 m); constants in MPC_HexAtmosphere, synced by BP_HexFogFadeSync.
- Shader cost: bottom pillar MI shipped 1261 / 554 vs 1234 / 402 fade off; bridge cliff MIs 937 -> 1069 PS.
- Not covered: bottom-bridge collapse GC pieces show unfaded; red channel misses by up to 28 codes near the moon glow.

## Commits it documents

- `ac1b069` Fog fade: bottom pillars and stone bridges fade into the void · [[Redo3 Hex Look and Atmosphere]]
- `32a308e` Update StartupMap: ice look lighting, fog floor, fog fade sync · [[Redo3 Hex Look and Atmosphere]]

## Sections

- Fog floor: what changed
- Pillar ends and bridge keels
- Visibility on the low platforms
- Why this setting
- Cost
- Ways past the trade-off
- The material fade underneath
- What changed
- Pixel check
- Measured void colour
- Shader cost
- Known limitation: a faded end in front of a lit surface
- How the check sees the frame
- Not covered

## Made by

- Thread: [[Redo3 Threads#Optimize the hex atmosphere effects|Optimize the hex atmosphere effects]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Fog volumes under the lower platforms]]
- [[Redo3 Artifact - Hiding the bottom pillars in fog]]
- [[Redo3 Artifact - Lower-chain clouds]]
- [[Redo3 Artifact - Hex atmosphere costs]]
