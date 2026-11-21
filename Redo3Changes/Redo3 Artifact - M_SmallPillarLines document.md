↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# M_SmallPillarLines - bottom pillar material

claude.ai Artifact · guide · [open the artifact](https://claude.ai/artifact/DHgacdaeHb3jF66MNLfnw4)

A Claude Docs document (dated 2026-11-05) that explains M_SmallPillarLines in /Game/Assets/MyPlatform/geo/SmallPillars, the material of every bottom-platform pillar (mesh SM_HexRockPillar, instance MI_SmallPillar_HexRock). It layers four looks: baked line art or glacier ice on the sides, parallax ice on the flat top, snow drifts from the outline and along bridge paths, then MF_FogFadeBelow fades everything into the fog below, with Nanite displacement set after the fog. It documents the 5 per-tile custom-data floats written by AMyHexPlatform (two packed 24-bit words for edge and path data) and the HLSL that decodes them. It gives a parameter table with defaults and MI values, the order to run the four Python build scripts in Saved/ClaudeTools/smallpillars/, and gotchas such as the Aura.Mat* editor commands. A signal-flow diagram shows the layers stacking into Make Material Attributes, then fog fade, then displacement.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Per-instance custom data cut from 7 to 5 floats: 0 falloff, 1-2 tile X/Y, 3 edge word (open sides, rings in, direction), 4 path word, written by BuildHexGrid and WritePathCustomData in MyHexPlatform.cpp.
- Glacier sides (GlacierSides = 1): T_GlacierIce_BaseColor/_Normal/_Roughness/_Cavity/_Height, GlacierTileCm 400, per-face projection every 60 degrees.
- Parallax ice top: HLSL IcePOM ray-marches T_PillarIce_Height, IceDepthCm 120, IceTiling 0.6 gives 4.2 m cells and a 16.7 m repeat, fades 25 to 60 m.
- Snow: HLSL HexSnowDrift, SnowReachFraction 0.3, SnowReachMin 0.5, SnowReachTiles 3, SnowRefRings 5, SnowPileHeight 25, displacement magnitude 120; path snow SnowPathHalfWidth 100 / SnowPathBankWidth 130 cm.
- Displacement is remade after the fog blend (block 'Snow 3') because BlendMaterialAttributes lerped it with the fog factor and shattered the underside under Nanite.
- M_Pillar2 reuses the HexSnowDrift code as a WPO snow mound, SnowTopPileHeight 60 cm, because HexPillar isn't Nanite.
- Build order: build_ice_top.py, build_glacier_sides.py, build_snow_edge.py, organize_pillar_mat.py; nodes tagged [ICE], [GLACIER], [SNOW]; build_snow_top.py does M_Pillar2 ([SNOWTOP]).
- Editor commands Aura.MatAddComment, Aura.MatClearComments, Aura.MatLinkReroutes, Aura.MatResizeComment in AuraMaterialGraphCommands.cpp, since UE 5.8 Python can't size comment boxes.
- Open note: the HexRock MI still has MeshLengthLocal 487.69 from the Sculpt pillar; the hex pillar is 1000 cm tall.

## Commits it documents

- `fa0ee5e` Hex pillars: rock pillar bottoms, glacier ice and edge snow drifts · [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Look and Atmosphere]]
- `1e23b9d` Add the T_GlacierIce textures for the rock pillar glacier sides · [[Redo3 Hex Pillars and Breaking]]
- `ce1125d` Add editor console commands for scripted material edits · [[Redo3 Engine Tools and Assets]]
- `ac1b069` Fog fade: bottom pillars and stone bridges fade into the void · [[Redo3 Hex Look and Atmosphere]]

## Sections

- Overview
- Per-tile data from C++
- Sides: line art and glacier ice
- Top: parallax ice
- Snow
- Fog fade and output order
- Parameters
- Rebuilding and maintenance

## Made by

- Thread: [[Redo3 Threads#Build material from ArtStation reference|Build material from ArtStation reference]]
- Explained on: [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Top Snow Build-up v3]]
- [[Redo3 Artifact - Top Snow Drift Normals]]
