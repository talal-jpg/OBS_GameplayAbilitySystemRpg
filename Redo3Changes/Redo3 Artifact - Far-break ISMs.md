↑ [[Redo3 Artifacts - Hex Performance and Breaking]] · [[Redo3 Artifacts Index]]

# Far-break ISMs

claude.ai Artifact · report · updated 2026-10-25 · [open the artifact](https://claude.ai/artifact/7T68Bs1gquCPMv2ivfgEEh)

Test report from PIE in StartupMap on 25 Oct 2026. Tiles and tile props that collapse between 50 m and 200 m from the local player can now play a baked break animation instead of just vanishing. Each far break adds one instance to a world-wide Instanced Static Mesh for its mesh and material, with the break time in custom data [0] and a playback speed in [1]. The page explains the C++ in AMyHexPlatform and UMyHexPillarBreakSubsystem and the material function MF_HexFarBreakFrame that reads the custom data. It also gives the cost measured with the placeholder material M_HexFarBreakPlaceholder, as a screenshot filmstrip and a table with bars. The far slots on BP_MyHexPlatform were still empty, so in the game far tiles still vanished. The page also covers a Houdini USD export of 49 Geometry Collection falls and two pillar GC fixes. Nothing was committed then; the code later landed in 613707b and b5ff0ef (2026-11-15).

## What it shows

- ActivateHexLocal measures the tile's horizontal distance to the local pawn once. Beyond BreakEffectsMaxDistance (50 m) and within FarBreakMaxDistance (200 m), the pillar plays FarTopPillar or FarBottomPillar and its props play FarPropMeshes[prop mesh]. No far breaks run on a dedicated server, for late joiners, past PlatformMaxDrawDistance/PropMaxDrawDistance, or above FarBreakMaxActive (512 at once, world-wide).
- Custom data [0] = GetWorld()->GetTimeSeconds() at the break. Custom data [1] = speed: for pillars GetPillarAnimRate(Z scale), 1x up to 2.5x; for props FarPropSpeedRange x 1/sqrt(instance Z scale), clamped to 0.25-4 (ice shards at scale 0.13 play at 2.7x, path floor at scale 2 at 0.71x).
- There is one ISM per mesh and material, owned by a transient, unreplicated actor. Collision, Lumen, distance fields, ray tracing, shadows (bCastShadow) and WPO velocity (bWPOWritesVelocity) are off. Finished instances are hidden at zero scale and their slots reused, an ISM is emptied when its last break ends, and all ISMs are created empty at BeginPlay so their PSOs are ready.
- Test: breaking a 63-tile and an 11-tile platform 100-135 m away peaked at 223 instances (74 pillars, 149 props on 8 meshes). Cost: +0.44 ms GPU in PrePass + BasePass, +0.00 ms in the velocity pass, +0.16 ms on the game thread; nothing was left over afterwards.
- MF_HexFarBreakFrame (/Game/Assets/MyPlatform/FarBreak): inputs FPS (30), FrameCount (180) and VertexFrame; outputs Age, Frame, FrameAlpha, Visible, Speed and StartTime. Each slot needs Length set to 6 s, and the material needs 'Used with Instanced Static Meshes' saved on it.
- Houdini export: 49 .usdc files in Saved/Exports/TilePropSim (all 45 PropBreakables GCs, 3 pillar GCs and the full-height GC_HexPillar_H100_from_H070). Each is a 6 s free fall at 30 fps, Y up, in metres; 164 MB in total and 2.04 M triangles. Made with usdsim/dump_gc.py and usdsim/write_usd.py in Saved/ClaudeTools/opt1024/usdsim.
- Fixes along the way: the 3 pillar GCs now use M_Pillar2_GC (M_Pillar2 without the WPO node) so their cut edges no longer tear apart. The pillar GCs also moved out of the AMyHexPlatform constructor into TopPillarHeightVariants on BP_MyHexPlatform, which stops their Dataflow graphs from being emptied on load.
- Console commands: Aura.BreakTopPlatform [Chains] [X Y Z] [Bottom] and Aura.PillarBreakStats. PIE test script: pt_far_test.py.
- Not done at the time: the Blueprint slots were empty, the cooked-build PSO path was unchecked, and the cost still had to be measured again with the real baked meshes. The code was committed later in 613707b (UMyHexPillarBreakSubsystem, M_Pillar2_GC) and b5ff0ef (FarBreak assets and far breaks).

## Sections

- Results at a glance
- A top platform 103 m away breaking
- What the C++ does
- Making your baked meshes work with it
- Cost, far meshes off vs on
- Baked tile-prop break for Houdini
- Also fixed on the way
- Changes and test commands

## Made by

- Thread: [[Redo3 Threads#Add far-break ISMs for distant tiles|Add far-break ISMs for distant tiles]]
- Explained on: [[Redo3 Hex Pillars and Breaking]], [[Redo3 Hex Tile Props]]
- Group: [[Redo3 Artifacts - Hex Performance and Breaking]]

## Other artifacts from the same thread

- [[Redo3 Artifact - PCG layout - nodes added for fog and snow]]
- [[Redo3 Artifact - Hex platform optimization - what changed]]
- [[Redo3 Artifact - Hex platforms - LODs vs switching instances off]]
- [[Redo3 Artifact - Hex Platform FX Layout - PCG Nodes Added]]
- [[Redo3 Artifact - Atmospherics comparison]]
- [[Redo3 Artifact - Top pillar GC test]]
