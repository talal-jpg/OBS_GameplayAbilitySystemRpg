↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Atmospherics comparison

claude.ai Artifact · comparison · [open the artifact](https://claude.ai/artifact/KEHABpy1DxnX1jo8d8hrvu)

A comparison page of hex-platform atmosphere layouts shot from the same five cameras in StartupMap PIE (74 platforms, HexRadius 200) on 24 Oct 2026. It sets the morning's layout (BEFORE) against three new ones built from the L_Showcase ground fog card and five Niagaras (SnowGPU, SoftSnowing, Mist01, DustParticles, NS1_Fog_01): A reference match per platform, B player-local snow, and C natural clustering with Perlin-pooled fog. Five judges scored C highest (6.30 against 5.68 for B and 5.63 for A), and C was tuned through C-final, C-final2 and C-final3 to C-final4, the shipped default. Each pass lists its changes and a GPU and frame-time table against targets. The page ends with the L_Showcase reference measurements, an extra view between the two levels and a full numbers table of GPU time, draw calls, particles, actors and memory.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Weighted scores: C natural clustering 6.30, B camera snow 5.68, A reference per platform 5.63; C-final became the default (AtmosphereVariant 2), with A and B still switchable.
- C-final4: 1.5x C-final's snow (13,500/s around the camera, 240/s flakes), fog noise tiling 0.45, particles culled inside 3 m with a 3-6 m fade, snow volumes beyond 60 m not drawn.
- C-final4 GPU ms: view 1 24.7 (target <= 25.4), view 2 23.0, between the levels 25.9 (target <= 27); 398 Niagara actors and 147 fog cards.
- BEFORE baseline: view 1 23.4 GPU ms, view 2 22.6; C before tuning had 491 Niagara actors and 702 fog cards.
- Tuning from C: camera-following blizzard 9,000/s and flakes 160/s, fog pools only 2+ rings in from any rim, cards cut from 702 to 147 (54 pool + 93 skirt), 35 far haze banks added.
- L_Showcase reference per 100 x 100 m: 4.2 SnowGPU emitters, 2.8 Mist01 emitters, 5.6 ground fog cards of 10-18 m covering 8.7% of the ground.
- Numbers are 10 s csvprofile averages with r.GPUCsvStatsEnabled; particle counts are estimates.

## Commits it documents

- `b5ff0ef` Hex platforms: bottom chain, atmosphere variants, GC and far breaks · [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Look and Atmosphere]], [[Redo3 Hex Platform PCG]]

## Sections

- Atmospherics comparison
- C-final4: shipped default
- C-final3
- C-final2: the last pass
- Winner: C (tuned)
- What changed from C
- Reference: L_Showcase outside the cave
- Extra view: between the two levels
- Numbers

## Made by

- Thread: [[Redo3 Threads#Add far-break ISMs for distant tiles|Add far-break ISMs for distant tiles]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Far-break ISMs]]
- [[Redo3 Artifact - PCG layout - nodes added for fog and snow]]
- [[Redo3 Artifact - Hex platform optimization - what changed]]
- [[Redo3 Artifact - Hex platforms - LODs vs switching instances off]]
- [[Redo3 Artifact - Hex Platform FX Layout - PCG Nodes Added]]
- [[Redo3 Artifact - Top pillar GC test]]
