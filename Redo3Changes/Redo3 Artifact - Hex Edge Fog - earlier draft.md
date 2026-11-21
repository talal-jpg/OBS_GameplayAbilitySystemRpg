↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Hex Edge Fog (RVT): Structure and Flow

claude.ai Artifact · walkthrough · [open the artifact](https://claude.ai/code/artifact/ee10fa73-b10a-449d-bcb0-6e9e6fc05ffa)

A Claude Docs document dated 2026-10-20 describing the hex edge fog system. Each hex platform chain writes its open edges into its own Runtime Virtual Texture, and a volumetric fog cube carried by the local mech reads both RVTs so fog is drawn only along platform edges. It explains the two-chain, two-RVT split by Z band, what M_Pillar1 writes into each RVT channel, how M_EdgeFogVolume turns that into density, and how a C++ timer on AMyCharPlayer snaps the RVT volumes to follow the player in 2048 cm steps. It also covers the data encoding and density formula, measured heights and ranges, tuning knobs, status and next steps, gotchas, backups and captures. Two diagrams are embedded: a structure mind map (4 groups, 10 parts) and a runtime flow across the game thread and renderer. When it was written nothing was committed and the C++ was not yet built into the editor.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Lower chain HexLower (125 platforms, tile tops Z -1036 to 300) writes RVT_EdgeFog_A; upper chain HexUpper (92 platforms, tile tops Z 5579 to 6966) writes RVT_EdgeFog_B; the volumes RVTV_EdgeFog_A (-2500 to 2800) and RVTV_EdgeFog_B (2800 to 9200) meet at Z 2800.
- Both RVTs are Mask4, uncompressed 8 bits per channel, 1024 x 1024 over a 12288 cm square (about 12 cm per texel).
- Channels written by M_Pillar1 (EdgeFogRVTPack): R = HexEdgeMask (1 at open sides), G = tile-exists flag, B = encoded tile-top height, A unused; M_EdgeFogVolume (EdgeFogDensity) recovers height as HeightEncodeMin + (B / G) x HeightEncodeRange to undo bilinear blending at outlines.
- Density per chain = edge mask x height band x noise x box fade, chains combined with max; the band rises from SpillDepth (300 cm) below the tile top and fades FogHeight (250 cm) above it.
- MPC_EdgeFog height encoding -2000 to 8000 (39 cm per 8-bit step), ChainA_TopZ -190 and ChainB_TopZ 6050 as void-fog fallbacks; fog cube Z -2000 to 8200.
- Material knobs: FogDensity 1.0 (fog pass divides by 100), FogColor (0.72, 0.79, 0.86), NoiseScale/Speed/Strength 600/40/0.6, BoxFade 0.25, VoidFog 0, EdgeBlurMip 2, Emissive 0.
- BP_MyCharMech knobs (C++ category Edge Fog): EdgeFogSnapStep 2048 cm, EdgeFogUpdateInterval 0.1 s, EdgeFogVolumeTag EdgeFogRVT, EdgeFogCubeName EdgeFogVolume; moving an RVT volume rebuilds its whole RVT, which may flicker briefly.
- Next steps: save StartupMap, rebuild AuraEditor (new UPROPERTYs, hot reload unsafe), tune in PIE, raise Volumetric Fog View Distance on ExponentialHeightFog_1 above 6000 if needed.
- Backups: M_Pillar1_BeforeRVT, BP_MyHexPlatform_BeforeRVT, BP_MyCharMech_BeforeFog, BP_MyCharMech_BeforeCppFog, M_EdgeFogVolume_BeforeUpperFix, MPC_EdgeFog_BeforeUpperFix; captures in Saved/EdgeFog.

## Commits it documents

- `9b3df08` Edge fog: per-chain RVTs from M_Pillar1, RVT volumes follow the mech · [[Redo3 Hex Look and Atmosphere]]

## Sections

- Overview
- Structure
- Runtime flow
- Data encoding
- Heights and ranges
- Tuning knobs
- Status and next steps

## Made by

- Thread: [[Redo3 Threads#Fog on platform edges|Fog on platform edges]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Hex Edge Fog - Structure and Flow]]
