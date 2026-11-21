↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Top Snow Build-up v3

claude.ai Artifact · comparison · updated 2026-11-05 · [open the artifact](https://claude.ai/artifact/5q3uHYnwrhXckadKeqw5T9)

A screenshot page from PIE on 2026-11-05 showing the rebuilt snow on the top platforms' material M_Pillar2. The snow piles are now capped and smooth like the bottom platforms', and they use the bottom's per-tile seam fade. The snow is the last layer before the fog fade, so it covers the ink outlines and glow. Paired shots compare the smooth-normal override on the piles only against all snow on the tops (SnowTopSmoothAllSnow), and the largest top platform against the largest bottom platform. The page ends with the layer order in M_Pillar2 and the Snow parameter group with defaults. The asset change is binary, so it can't be matched exactly; it most likely went in with fa0ee5e ('edge snow drifts', M_Pillar2, 2026-11-15).

## What it shows

- Piles capped at 50 cm (SnowTopPileHeight); before, they reached 180 cm with steep 20 cm walls at every tile seam.
- SnowSeamSoftness 31.15 (the bottom's value), SnowRimSoftness 30, SnowReachTiles 4.
- SnowTopDriftColor (0.85, 0.9, 1) and SnowTopRoughness 0.65, taken from the bottom's snow.
- SnowTopSmoothAllSnow 1: every snowy surface on the tops drops the rock normal and the depth dither, which removes the dark streaks. 0 limits the override to the piles. SnowTopMoundNormal 1 sets the pile shading strength.
- Layer order: rock/snow blend, depth dither, ink outlines, then snow (colour, roughness, normal, no ink or glow, subsurface from before the outlines), then the fog fade. The snow replaces any other height offset where it lies.
- Views captured: snow_low, edge_close, close_a, edge_inward. The camera follows the pawn, so runs don't line up exactly.
- Likely committed in fa0ee5e (Hex pillars: rock pillar bottoms, glacier ice and edge snow drifts), which changes M_Pillar2.

## Sections

- Piles: before and after
- Snow over everything, including the tops' own snow
- Top next to bottom
- Layer order in M_Pillar2
- Snow parameters (group Snow)

## Made by

- Thread: [[Redo3 Threads#Build material from ArtStation reference|Build material from ArtStation reference]]
- Explained on: [[Redo3 Hex Look and Atmosphere]], [[Redo3 Hex Pillars and Breaking]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Top Snow Drift Normals]]
- [[Redo3 Artifact - M_SmallPillarLines document]]
