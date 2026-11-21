↑ [[Redo3 Artifacts - Hex PCG and Tile Props]] · [[Redo3 Artifacts Index]]

# Hex Platform PCG Walkthrough

claude.ai Artifact · walkthrough · updated 2026-10-22 · [open the artifact](https://claude.ai/artifact/5ALhLEyyA4WiJQUPJhmHwe)

A walkthrough, one node group at a time, of PCG_SplineIHexPlatformsFromSpline, the graph on BP_MyHexPathSegment that places every BP_MyHexPlatform. It has 18 numbered steps with SVG diagrams, and all distances are in layout cm (HexRadius 100 units; multiply by S = HexRadius/100 for world size). The steps cover: reading the segment, path samples, end boxes, older platforms as obstacles, centre and side streams with fit-to-space shrinking, medium and small rings, restoring heights and seeding the outline, and spawning the lower chain (SpawnActor_1, tag HexLower) and the upper chain (UpSpawnActor, tag HexUpper, lifted 15 m with short pillars). Because the PC was offline, it was built from earlier threads' notes (Oct 5-12, baseline 620b1fe), not from the live asset. It ends with a table of every value a spawned platform receives.

## What it shows

- The graph runs on the BP_MyHexPathSegment that AMyHexPathSpawner::ExtendPath spawned, after InitSegment sets Seed = SegmentIndex and calls GenerateLocal. SegmentSpline is already shrunk by S around the path start. Seven Get Actor Property nodes read PathStartDistance, LayoutDownScale/Offset, LayoutUpScale/Offset, LayoutScale and HexRadius.
- Path_Samples drops a point every 500 cm, and ring platforms are kept only with PathDist <= 9000 (90 m). Ends_Bounds puts 600 m boxes beyond each segment end, and the distance to them is CapD.
- Obstacles: Get Actor Data reads tag HexLower plus every BP_MyHexBridge_C with Always Requery. The points are moved to layout space and flattened into tall columns (bounds Z +/-100000), so overlap checks are 2D (WorldD). World_Dummy keeps the set from being empty on the first segment.
- Centre stream: one platform every 5000 cm from a 2500 cm start, Gf = floor(U(11, 12.999)), and a Perlin height offset of about +/-4 m. SplineDistance = Distance + PathStartDistance, which AMyGameState uses to rank players.
- Fit formula: A = min(CapD-150, PeerD/2-150, WorldD-300), GAl = floor((A-100)/175), Gf = min(Gf, GAl), Radius = 175G+100, Rm = 175G+250. Centre points below grid 3 are dropped. Side points have an extra OtherD limit (at least 3 m from the centre platforms) and are dropped below grid 4.
- Side streams start at 20 m and 30 m and are offset 42-48 m sideways, with about 15% dropped, a +/-17 m self-prune and Gf 6-9. Medium rings: 6 directions, about 55% kept, G 3-5, gap 3.5-8 m. Small rings: 5 directions, about 60% kept, G 1-2, gap 2.6-4.2 m. Both are self-pruned LargeToSmall.
- Upper chain: it reads only HexUpper obstacles and has one stream every 50 m from 0 m with G 10-11. Its rings mostly step up (U(-0.3, 1.0) x Gap), UpFinal_Lift raises it by +1500 cm, and AttributeRemap_0 sets VS = 0.08 + 0.02 x Gf (0.10-0.30) for VerticalScaling. Lower platforms keep the Blueprint default of 0.5.
- Spawned property overrides: GridSize, NoiseOffset, SplineDistance and HexRadius, plus VerticalScaling on the upper chain. They are replicated initial-only, so clients rebuild the same grid in BeginPlay. Bridges are planned afterward by UMyHexBridgeSubsystem.
- Commits: 620b1fe (graph reorganised 2026-10-12, the baseline described), 86539d1 (World_Bridges selects every MyHexBridge, 2026-10-17), and later ff2437c (floating small top platforms, bottom chain class).

## Sections

- 01 Read the segment
- 02 Sample the path every 5 m
- 03 Fence off the segment ends
- 04 Turn older platforms into obstacles
- 05 Place the centre stream
- 06 Shrink centre platforms to fit
- 07 Place the left and right streams
- 08 Shrink side platforms to fit
- 09 Merge the big platforms
- 10 Grow medium rings
- 11 Grow small rings
- 12 Restore heights and seed the outline
- 13 Scale back and spawn the lower chain
- 14 Upper chain: its own obstacles
- 15 Upper stream and fit
- 16 Upper medium and small rings
- 17 Lift the upper chain and shorten its pillars
- 18 Scale back and spawn the upper chain
- What each platform ends up with

## Made by

- Thread: [[Redo3 Threads#Hex PCG graph walkthroughs|Hex PCG graph walkthroughs]]
- Explained on: [[Redo3 Hex Platform PCG]], [[Redo3 Hex Path and Ranking]]
- Group: [[Redo3 Artifacts - Hex PCG and Tile Props]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Tile Props PCG Walkthrough]]
