↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Lower-chain clouds

claude.ai Artifact · report · [open the artifact](https://claude.ai/artifact/45raEdu1DAzZZEyqrSBViD)

A capture and cost report (StartupMap lower chain, PIE 1920x1080, 2026-10-31) on PS_Clouds_Wind clouds drifting around the lower platforms, in the void beside them and along the bottom bridges, scattered per platform by the live atmosphere graph. A new 77-node subgraph PCG_HexAtmo_LowerClouds, called by one node from PCG_HexAtmo_C, places clouds from four sources (side, near, void, bridge) on every rendering machine but never on a dedicated server; the top chain gets none unless UpperChainClouds is on. Light copies NS_HexCloudsWind, FXT_HexCloudsWind and M_HexCloudWind cut sprites per cloud and pixel instructions. Two later updates add 8-vertex sprite cutouts (13:45 UTC) and GPU simulation (23:30 UTC), each with cost tables. The page has a knobs table, a prototype quantity test at 1x/2x/3x and before/after shots of the shipped graph, and says nothing is committed.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Shipped knobs: CloudDensity 1.0 (204 clouds on StartupMap), SideDensity 1.0 (58), NearDensity 1.0, VoidDensity 0.5 (54), BridgeDensity 1.0, CloudSeed 7001, UpperChainClouds off.
- Placement: side clouds 5-9 m out and 4.5-8 m below the tiles; void banks 25-70 m out owned by the nearest lower platform; bridge clouds 6-9 m to the side and 7-11 m under the deck.
- NS_HexCloudsWind uses about 40 sprites per cloud instead of about 160 with a 24 s warm-up; M_HexCloudWind uses vertex fog, 388 instead of 1010 pixel instructions.
- Culling: FXT_HexCloudsWind at 250 m and up to 400 instances; per source side 90 m, near and bridge 120 m, void 180 m.
- Cost of the shipped graph looking down from the upper chain: +0.5 to +2.1 ms; at 235 clouds (VoidDensity 0.8) it was +4.2 ms.
- Sprite cutouts (8-vertex outline from TX_Cloud_RAHM red channel, threshold 0.02) halve the cloud translucency pass: +0.28 ms to +0.13 ms.
- GPU sim: fixed bounds of +-15 m across and +-7 m tall, 16 s warm-up; looking down went from +1.1 ms (CPU) to +-0.0 ms.
- Not built: one Niagara system per platform could cut about 255 systems to about 29.

## Commits it documents

- `08a123a` Lower-chain clouds as one cloud bank per platform; torches lose fire · [[Redo3 Hex Look and Atmosphere]], [[Redo3 Hex Bridges]]

## Sections

- What was built
- Knobs
- Cost
- Quantity test (prototype)
- Shipped graph in PIE
- Sprite cutouts
- GPU sim
- Notes

## Made by

- Thread: [[Redo3 Threads#Optimize the hex atmosphere effects|Optimize the hex atmosphere effects]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Fog volumes under the lower platforms]]
- [[Redo3 Artifact - Hiding the bottom pillars in fog]]
- [[Redo3 Artifact - Fog fade captures]]
- [[Redo3 Artifact - Hex atmosphere costs]]
