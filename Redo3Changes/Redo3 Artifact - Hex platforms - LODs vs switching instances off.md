↑ [[Redo3 Artifacts - Hex Performance and Breaking]] · [[Redo3 Artifacts Index]]

# Hex platforms: LODs vs switching instances off

claude.ai Artifact · comparison · updated 2026-10-24 · [open the artifact](https://claude.ai/artifact/155GEhSrhCafnhHmYzHMud)

A Claude Docs analysis answering whether the hex platforms should get LODs or have far instances switched off while Nanite is disabled (r.Nanite.ProjectEnabled=False, which in 5.8 also turns Virtual Shadow Maps off). Its answer is to use both and to keep fewer platforms alive. HLOD cannot work for runtime-spawned PCG platforms, and Nanite is left for a later test. It records the state on 24 Oct, when every mesh had one LOD and no cull distances were set. It reports a PIE playtest in StartupMap: 175 platforms, 230 bridges, 207 torches and about 96k instances, GPU-bound at about 64 ms, with the instanced meshes taking about 37-40 ms of that. It then compares how UE 5.8 treats each option, gives a recommended order of changes and measurement advice, and lists what the changes won't fix. Many of its recommendations were carried out the same day (see 'Hex platform optimization: what changed').

## What it shows

- Playtest A/B results: hiding every instanced mesh saved about 40 ms (shadows 12, base pass 12, velocity 9, depth prepass 7). Culling past 1 km saved about 13 ms and past 500 m about 17 ms, halving the shadow distance about 3 ms, and turning dynamic shadows off about 15 ms. About 23 ms (shadows inside the 200 m cascades and geometry within 500 m) is out of reach of any draw distance.
- Triangle shares: pillars 12,577 instances and 54.5M triangles (about a third); ice and snow props plus path floor 21,648 instances and 60.4M (35-38%); stone bridge kit 14,117 instances and 40-55M; wooden bridge cubes 47,630 instances but only 2.3M (under 2%). SM_SnowMound_01 alone has 6,358 instances and 21.8M triangles.
- HexPillar's Nanite fallback draws 4,336 triangles (1,596 before an uncommitted 10-23 change), and every pillar has a 2,081-vertex convex hull. M_Pillar2's WPO puts every pillar in the velocity pass (about 9 ms). The ice and snow materials write Pixel Depth Offset, which turns off early depth rejection.
- The path is generated up to 1.8 km ahead (ExtendDistance 60000 x HexRadius 3). Debug costs were on: a per-hex PrintString in OnConstruction (44,812 log lines), bridge bDrawDebug=True, and BuildHexGrid running twice on the server.
- UE 5.8 options compared: ISM LODs are picked per instance on the GPU and pop without dithering. Per-component Desired Max Draw Distance/SetCullDistance is ignored on Nanite components. Per-instance InstanceStart/EndCullDistance works but pops unless the material uses PerInstanceFadeAmount. HISM is a bad fit because it rebuilds its tree on every RemoveInstance. HLOD is impossible because the platforms are runtime PCG and StartupMap isn't World Partition. Never use SetVisibility or SetHiddenInGame to switch components off.
- Recommended order: (1) clean the measurement (bDrawDebug off, remove the PrintString, a 1920x1080 window, stat unit). (2) No-code changes: HexPillar fallback back to about 1,600 triangles, untick World Position Offset Writes Velocity on HexPillarsISM, Cast Shadow off on flat props, torch light draw distance, SnowMound collision off, ExtendDistance 20000. (3) 12-vertex hex-prism pillar collision. (4) Draw distances set in AMyHexPlatform::OnTilePropsGenerated and AMyHexBridge::BeginPlay. (5) LODs. (6) Props generated only near players. (7) A Nanite A/B test.
- Measurement advice: use stat gpu pass times, r.MeshDrawCommands.Stats 1 or stat culling (stat rhi can't see GPU-culled triangles), r.ForceLOD, r.InstancedStaticMeshes.GpuLod 0, ShowFlag.InstancedStaticMeshes 0 and r.HZBOcclusion 1. Bracket every test with baselines, because GPU time drifted from 68 to 60 ms over two minutes.
- Won't fix: the 27 ms of non-instanced GPU work (Lumen about 7 ms, TSR and post 6-8 ms), VRAM (peak 5.4 of 8 GB), buffer re-uploads on every AddInstance or RemoveInstance, and collapse hitches. The prop LODs and lighter GCs it recommends shipped in 3d05747 (2026-11-15).

## Sections

- Short answer
- What the platforms do today
- Measured on your PC
- How UE 5.8 treats each option
- Recommended order
- How to measure
- What this won't fix, and what is unchecked

## Made by

- Thread: [[Redo3 Threads#Add far-break ISMs for distant tiles|Add far-break ISMs for distant tiles]]
- Explained on: [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Tile Props]], [[Redo3 Hex Bridges]], [[Redo3 Engine Tools and Assets]]
- Group: [[Redo3 Artifacts - Hex Performance and Breaking]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Far-break ISMs]]
- [[Redo3 Artifact - PCG layout - nodes added for fog and snow]]
- [[Redo3 Artifact - Hex platform optimization - what changed]]
- [[Redo3 Artifact - Hex Platform FX Layout - PCG Nodes Added]]
- [[Redo3 Artifact - Atmospherics comparison]]
- [[Redo3 Artifact - Top pillar GC test]]
