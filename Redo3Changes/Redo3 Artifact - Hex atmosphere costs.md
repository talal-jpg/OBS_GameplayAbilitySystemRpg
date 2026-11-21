↑ [[Redo3 Artifacts - Hex Performance and Breaking]] · [[Redo3 Artifacts Index]]

# Hex atmosphere costs

claude.ai Artifact · report · [open the artifact](https://claude.ai/artifact/YCtZP4eT7dWwdBVVsVTJ5c)

A cost analysis of the live hex atmosphere graph PCG_HexAtmo_C (variant C) in StartupMap editor PIE at 1920x1080, dated 2026-11-01. An After section at the top records what changed that day on the 107-platform layout: Niagara fire removed from the bridge torches, stable sorting and a pre-aged burst for the GPU clouds, one cloud system per platform (543 systems down to 62), a cloud graph gate, and cheaper blizzard, flakes, dust, mist, haze, fog walls and fog cards. It then gives the morning's analysis on 47 platforms: an inventory of every source with counts and culling, per-asset facts, measured cost and a ranked list of seven optimisation options. The summary finds the whole atmosphere costs about 1-1.8 ms of frame time, with no single source over about 0.6 ms, while the 126 bridge-torch fires cost 2.9-3.9 ms of GPU simulation. Captures compare all-on, all-off and single sources from four views.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Bridge torches: Niagara fire removed from B_BridgeTorchFire, point light and flicker kept; 3.5-4.0 ms GPU sim gone, frame 1.7-3.4 ms faster.
- One cloud system per platform via PCG Apply On Object -> BP_MyHexPlatform arrays -> NS_HexCloudBank: 543 systems -> 62, GPU sim 2.2-4.7 ms -> 0.2-0.4 ms, same placement.
- Cloud stutter fix: worst frame when clouds come into view 108 ms -> 40 ms; cloud graph gate cut atmosphere PCG startup time 1064 -> 339 ms.
- Mist cull distance 150 -> 850 m (fog hides it at about 818 m), cap 60; fog cards moved to unlit M_HexGroundFogUnlit.
- Atmosphere cost on minus off after step 3: 0.89 ms looking down, 1.63 ms across, 2.08 ms wide (before 1.18 / 2.00 / 2.33).
- Inventory (47 platforms): blizzard NS_HexAtmo_Blizzard on every platform up to 60,000/s plus a camera one at 13,500/s; FXT_HexAtmoVolume culls at 120 m.
- Top ranked option was giving PS_Torch_Fire_Niagara an effect type or a cheaper fire (about 2.5-3.5 ms GPU); CloudDensity stays at 0 in the saved map.

## Commits it documents

- `08a123a` Lower-chain clouds as one cloud bank per platform; torches lose fire · [[Redo3 Hex Look and Atmosphere]], [[Redo3 Hex Bridges]]
- `b5ff0ef` Hex platforms: bottom chain, atmosphere variants, GC and far breaks · [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Look and Atmosphere]], [[Redo3 Hex Platform PCG]]

## Sections

- After: what changed on 2026-11-01
- Atmosphere cost, before vs after step 3
- Clouds: one system per platform vs one per cloud
- Notes
- Summary
- Inventory
- Per-asset facts
- Measured cost
- Optimisation options, ranked
- All on vs all off
- Single sources

## Made by

- Thread: [[Redo3 Threads#Optimize the hex atmosphere effects|Optimize the hex atmosphere effects]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Performance and Breaking]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Fog volumes under the lower platforms]]
- [[Redo3 Artifact - Hiding the bottom pillars in fog]]
- [[Redo3 Artifact - Fog fade captures]]
- [[Redo3 Artifact - Lower-chain clouds]]
