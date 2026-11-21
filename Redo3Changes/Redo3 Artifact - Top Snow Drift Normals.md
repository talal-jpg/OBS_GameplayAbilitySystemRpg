↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Top Snow Drift Normals

claude.ai Artifact · comparison · updated 2026-11-05 · [open the artifact](https://claude.ai/artifact/FFPLMSYYgP3A4Bu36BPEdp)

Before and after PIE screenshots for an earlier 2026-11-05 fix to the snow drifts on the top platforms in M_Pillar2. The drift used to show the pillar's rock normal and wet-rock tint; it now uses its own smooth normal, worked out from the snow height. Each pair comes from one PIE run with the same camera, with only the drift normal switched at runtime. The page explains what is still visible: triangle facets on near mounds are dynamic shadows on the raised snow geometry, and vertical streaks are the top texture stretched by the mound height. It ends with the three new Snow-group material parameters.

## What it shows

- SnowTopNormalBlend 1.0: how much the drift replaces the pillar normal (0 = the old look).
- SnowTopMoundNormal 1.0: strength of the mound shading (0 = flat snow).
- SnowTopNormalMinFeature 10 cm: the smallest drift detail the normal follows. Finer detail fades to flat with distance so far tile seams don't flicker.
- Facets on near mounds disappear with dynamic shadows off, so they come from shadows, not the normal.
- Vertical streaks on tall mound sides come from the top texture stretched upward. Blending steep sides to plain snow colour would hide them.
- Views: a zoomed close-up, snow_low, edge_close, low_across, close_a, plus wider edge_inward and snow_mid finals.
- Superseded later the same day by 'Top Snow Build-up v3'. The material change most likely went into fa0ee5e.

## Sections

- Close-up of the drift
- Same camera, four views
- What's still visible, and why
- Wider views, final
- New material parameters

## Made by

- Thread: [[Redo3 Threads#Build material from ArtStation reference|Build material from ArtStation reference]]
- Explained on: [[Redo3 Hex Look and Atmosphere]], [[Redo3 Hex Pillars and Breaking]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Top Snow Build-up v3]]
- [[Redo3 Artifact - M_SmallPillarLines document]]
