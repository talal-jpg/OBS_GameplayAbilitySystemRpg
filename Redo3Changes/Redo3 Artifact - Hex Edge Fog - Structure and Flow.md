↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Hex Edge Fog: Structure and Flow

claude.ai Artifact · diagram/map · updated 2026-10-20 · [open the artifact](https://claude.ai/artifact/NnwavvPonDrmuK7XvmHViw)

A Claude Docs page explaining how volumetric fog gathers on the open edges of both hex platform chains, only inside a fog cube that follows the local player's mech. C++ works out each tile's open-side bits once (BuildHexGrid writes them into HexPillarsISM custom data 3). M_Pillar1's HexEdgeMask turns them into a gradient and draws it from above into one Runtime Virtual Texture per chain: RVT_EdgeFog_A for the lower chain, RVT_EdgeFog_B for the upper. M_EdgeFogVolume on the EdgeFogVolume cube in BP_MyCharMech reads both by world position, and AMyCharPlayer moves the RVT volumes along with the mech. The page has a structure mind map, a data-flow diagram, the runtime flow, a full reference table of assets and settings, and known limits. When it was written, the C++ mover was not built and StartupMap was unsaved; the system was committed in 9b3df08 (2026-11-15).

## What it shows

- Lower chain tile tops sit at Z -1036 to 300 and go into RVT_EdgeFog_A through RVTV_EdgeFog_A (Z -2500 to 2800). Upper chain tile tops sit at Z 5579 to 6966 and go into RVT_EdgeFog_B through RVTV_EdgeFog_B (Z 2800 to 9200). Both volumes are 12288 square, Movable and tagged EdgeFogRVT.
- M_Pillar1's HexEdgeMask (EdgeApothem 83.64, EdgeWidth 167.27) feeds the RVT Output Mask4 pin: R = edge mask, G = tile exists, B = tile-top height. The RVTs are Mask4, uncompressed, 1024 squared, about 12 units per texel.
- MPC_EdgeFog: HeightEncodeMin -2000, HeightEncodeRange 10000, ChainA_TopZ -190, ChainB_TopZ 6050. Because B is 8-bit, tile-top height is stored in steps of about 39 units.
- M_EdgeFogVolume is a Volume-domain, Additive material. FogDensity is 1.0 because the fog pass divides extinction by 100; SpillDepth is 300 and FogHeight 250, plus noise and BoxFade parameters. Density = height band x noise x box fade, taking the max of A and B.
- The EdgeFogVolume cube on BP_MyCharMech is 4000 x 4000, with Z pinned to -2000..8200 by C++, absolute rotation and Only Owner See. AMyCharPlayer settings: EdgeFogVolumeTag 'EdgeFogRVT', EdgeFogSnapStep 2048, EdgeFogUpdateInterval 0.1 s, EdgeFogCubeName 'EdgeFogVolume', EdgeFogCubeMinZ -2000, EdgeFogCubeMaxZ 8200.
- Runtime: UpdateEdgeFogVolumes runs when control is gained (BeginPlay or NotifyControllerChanged), then on a looping 0.1 s timer. It finds the volumes by tag and re-centres one on the 2048 grid when the mech is more than 2048 from it (an RVT volume's pivot is its min corner). Moving a volume invalidates its RVT, which can show as a brief flicker.
- Limits: the height bands are fixed, and the chains are about 6000 apart, which equals ExponentialHeightFog_1's volumetric view distance of 6000. Open-side bits are not updated when tiles collapse, and Only Owner See is ignored in a plain editor viewport. Backups include M_Pillar1_BeforeRVT, BP_MyHexPlatform_BeforeRVT and BP_MyCharMech_BeforeCppFog.
- Committed in 9b3df08 'Edge fog: per-chain RVTs from M_Pillar1, RVT volumes follow the mech' (2026-11-15). The EdgeFogVolume component itself came with 9d6fb14.

## Sections

- Overview
- Structure
- Data flow
- Runtime flow
- Reference
- Limits and checks

## Made by

- Thread: [[Redo3 Threads#Fog on platform edges|Fog on platform edges]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Hex Edge Fog - earlier draft]]
