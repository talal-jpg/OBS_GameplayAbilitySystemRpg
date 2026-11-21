↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Ink lines in the hex pillar materials

claude.ai Artifact · walkthrough · updated 2026-10-27 · [open the artifact](https://claude.ai/artifact/EMiF3ZxPxgmhrTJSwEAbvi)

A long Claude Docs walkthrough of the in-material ink outlines: the Outline comment box at the end of M_Pillar2, with the same HLSL in M_Pillar2_GC, MM_Ice_Hex and MM_SnowAssets_Hex. The section holds 75 nodes in six sub-boxes, (a) to (f), each around one Custom node, wired with 11 named reroutes prefixed OL_. Each pixel takes the strongest of an outline, a crease and a hex-edge line, fades it with distance, and pushes BaseColor toward the ink colour. The doc measures the HexPillar mesh the code relies on, then goes node by node with maths, node tables and code. It also covers how the GC break pieces and the ice and snow props differ, gives a full parameter table (defaults vs V2.1) and the seven V2.1 instances, and ends with debugging tips and known limits; it has 12 diagrams. The materials were committed in cf4ec00 ('Ice look V2.1: in-material ink outlines, iteration MIs, blue mist').

## What it shows

- UseOutline (static switch, default off) compiles the section in; off gives the original material (920 PS / 352 VS instructions in M_Pillar2). OutlineAmount 0 hides the lines but keeps the cost.
- HexPillar (/Game/Assets/MyPlatform/geo/SM/HexPillar): pivot at the top, hangs to z -200.4, apothem 83.64 mu, LOD0 14,346 triangles (Nanite was off). HexPillarBottom is 370.5 mu tall with apothem 87.1 and 1,272 triangles.
- HexPillarsISM custom data: 0 Falloff, 1-2 tile centre, 3 open-side bits (default 63). Faces with a neighbour at build time get no rim ink, and the bits are not rewritten when a neighbour falls.
- (d) CN_InkFade: FadeStart 50000 / FadeEnd 100000 cm, FarWidthScale 2.5, InkFarStart 3000 / InkFarEnd 15000. Lines widen from 30 m to 2.5x at 150 m, fade from 500 m and are gone at 1 km.
- (f) CN_InkHandDrawn: mesh-fixed value noise. InkWobbleAmount 0.28 swells lines by up to 28%, InkWobbleScale 0.03 (props 0.02), InkEdgeSoftness 0.5 feather; the line never gets thinner.
- (a) CN_InkSilhouette: |N.V|^2 over its fwidth, SilhouetteWidth 5 px; FacetSilhouette 0.45 on props only. (b) CN_InkCrease: facet vs vertex normal mismatch plus curvature, CreaseStrength 3, CreaseThreshold 0.04, CreaseRadius 5 cm.
- (c) CN_InkHexEdge (pillars only): rim and corner lines on open faces in the mesh frame through VertexInterpolators. EdgeWidthPx 5, PillarApothemLocal 83.64 (Bottom 87.1), CornerFade 100 mu, ShoulderLine off.
- (e) CN_InkCombine: alpha = OutlineAmount x InkOpacity x Fade x max(Sil, Crease, Edge). InkColor #003E77, InkMultiply 0.7, InkDarken 0.48 (props 0.55): about 0.3 x navy + 0.364 x the local colour. Emissive and Subsurface are dimmed under lines.
- GC pieces: M_Pillar2_GC uses CN_InkMeshP to divide z by GCHeightRatio (0.3 / 0.7 / 1.6 for GC_HexPillar_H030/H070/H160; 646, 851 and 576 tiles), and treats every side as open (constant 63). Props have no hex-edge node.
- Cost about 2 fps (49-50 vs 51-52 fps) at view C in StartupMap; far rims showed little ink and were not fixed; backups are in Saved/ClaudeBackups/IceLook_2026-10-25/ and TalalEdits_20261027T235453Z. The doc also lists six places where parameter or comment text disagrees with the code (for example CornerFade uses 40%, not a quarter, and the widths are render pixels, not 1080p).

## Sections

- How the ink lines work
- The HexPillar mesh the lines are drawn on (Faces, the outward turn and open sides)
- Node by node: (d) CN_InkFade, (f) CN_InkHandDrawn, (a) CN_InkSilhouette, (b) CN_InkCrease, (c) CN_InkHexEdge, (e) CN_InkCombine, Other materials: GC pieces and props
- Parameters
- Tweaking, debugging and limits

## Made by

- Thread: [[Redo3 Threads#Fix and extend the hex ink lines|Fix and extend the hex ink lines]]
- Explained on: [[Redo3 Hex Look and Atmosphere]], [[Redo3 Hex Pillars and Breaking]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Ice look comparison page]]
- [[Redo3 Artifact - Ink lines under Nanite]]
- [[Redo3 Artifact - Props outline section layout shots]]
- [[Redo3 Artifact - Bottom pillar ink first pass shots]]
- [[Redo3 Artifact - Bottom pillar rim line shots]]
- [[Redo3 Artifact - Bottom bridge ink shots]]
- [[Redo3 Artifact - Bottom pillar snow-edge line shots]]
