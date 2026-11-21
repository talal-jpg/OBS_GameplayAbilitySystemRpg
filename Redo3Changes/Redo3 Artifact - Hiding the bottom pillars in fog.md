↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Hiding the bottom pillars in fog

claude.ai Artifact · comparison · updated 2026-10-30 · [open the artifact](https://claude.ai/artifact/HuY1BRsREXufy8jaAT3Aro)

A Claude Docs answer to the user's question about the cheapest way to hide the bottom of the lower-chain pillars in fog. It recommends keeping the pillars opaque and blending their lower few metres to the exact fog colour inside M_Pillar2, behind a static switch turned on only in MI_Pillar2_Bottom_V2_1. It explains why the match is exact: with BaseColor, Metallic and Specular at 0 the pixel equals its Emissive F, and the height fog output pixel*(1-f) + F*f stays F at any distance. Six options are compared on cost and coverage, followed by a six-step setup and the places where the fade can still show. The user approved the build, which landed as ac1b069 (MF_FogFadeBelow, BP_HexFogFadeSync and fog-fade stone bridge materials), with the StartupMap fog fade sync in 32a308e.

## What it shows

- Cost of the recommended opaque fade: about 15 extra pixel-shader instructions (about 1.5% of M_Pillar2's 920 to 1,060), with no extra passes, draw calls, render targets or VRAM.
- Condition: height fog, the sky below the horizon and the fade must share one fog colour. Height fog was not linked to MPC_HexAtmosphere.FogColor. The build brief that followed used a new FogFadeColor MPC parameter written by BP_HexFogFadeSync.
- The fade is measured down from each pillar's own top (HexPillarBottom's pivot is at the tile top), with a second term on local Z / 370.5 so short pillars finish. It goes after the Outline (ink) section.
- Options table: 1 opaque fade (use this); 2 masked dithered opacity (fallback, 0.05-0.4 ms, dither crawls under TSR); 3 translucent fog cards (decoration only); 4 height fog floor (keep for atmosphere); 5 Local Fog Volume per platform (optional veil, 32 per view, none within 20 m); 6 volume boxes or denser volumetric fog (no).
- Setup: one fog colour source, flat fog settings (Fog Max Opacity 1, no cubemap, Override Light Colors with Fog Inscattering Colors), MF_FogFadeBelow (Material Attributes in and out, FadeStart and FadeLength), a FogFadeBelow static switch in M_Pillar2, the same function in M_Pillar2_GC and the lower-chain ice and snow props, then a Pixel Inspector check in PIE.
- Limits: the fade shows as a fog-coloured shape in front of non-empty backgrounds. The sky's #026AB5 is 23-44% brighter than the fog's #0359a5. Volumetric fog within about 60 m can drift a few sRGB values, and Lumen hardware ray-traced reflections may still see the hidden part.

## Sections

- Recommendation
- Why it hides completely
- Options compared
- Setup if we build it
- Limits and checks

## Made by

- Thread: [[Redo3 Threads#Optimize the hex atmosphere effects|Optimize the hex atmosphere effects]]
- Explained on: [[Redo3 Hex Look and Atmosphere]], [[Redo3 Hex Pillars and Breaking]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Fog volumes under the lower platforms]]
- [[Redo3 Artifact - Fog fade captures]]
- [[Redo3 Artifact - Lower-chain clouds]]
- [[Redo3 Artifact - Hex atmosphere costs]]
