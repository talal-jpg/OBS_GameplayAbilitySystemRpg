↑ [[Redo3 Artifacts - Hex Path, Bridges and Paving]] · [[Redo3 Artifacts Index]]

# Hex Platform Flow

claude.ai Artifact · diagram/map · [open the artifact](https://claude.ai/artifact/Eu9pvSfWyobfjNLSFgcETj)

An interactive mind map (subtitle 'AuraRedo3 · hex path, platforms and bridges') of every function from BeginPlay on the path spawner to a bridge standing between two platforms, and on to collapse, culling and player ranking. It holds 178 function nodes (155 marked verified) in ten stages; clicking a node shows its file, line, callers, calls, which machine runs it and a full description, and there is a search box, expand-all and stages-only views. A Key variables section lists each stage's settings with defaults, which ones scale with HexRadius / 100 and which replicate. It states that one knob, HexRadius on BP_MyHexPathSpawner, scales the whole system. Note cards flag that the 'lead' is the host's pawn, that bridge debug drawing is on in DefaultGame.ini, that BuildHexGrid runs twice, and which distances are not scaled.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Ten stages: Path grows, PCG lays out platforms, Platform builds itself, Bridge batch, Planner, Bridge actor, Lead lands and platform collapses, Culling and teardown, Player ranking, Not used at runtime; 178 nodes, 155 verified.
- Path: AMyHexPathSpawner (server only, not replicated) extends the spline with Perlin-steered points (PerlinGain 2, MaxYawDeviation 60 deg, NoiseSeed 17.31) and spawns BP_MyHexPathSegment actors; SegmentLength 10000 (BP 30000), ExtendDistance 15000 (BP 60000), CheckInterval 0.25 s.
- PCG_SplineIHexPlatformsFromSpline runs once per segment on the server in layout space (radius-100 units): centre platforms every 50 m from 25 m with grid 11-12, side streams 42-48 m left and right with grid 6-9, an upper chain grid 10-11 lifted 1500 cm, segment end boxes of 600 m.
- AMyHexPlatform is replicated and always relevant; GridSize, HexRadius, noise, curve and SplineDistance replicate initial-only and ActivatedTiles always; every machine builds its own grid.
- UMyHexBridgeSubsystem batches registered platforms (BuildDelay 0.1-0.5 s, burst quiet 1 s, max 8 s) and FPlanner::Run plans with geometry first and traces last through nine passes: Spine, Tree (Kruskal), Loop, Leaf, Route, Bypass, Branch, Fallback, Rescue.
- Planner limits: MaxBridgeLength 4500, MaxSlopeDegrees 30, MaxEndAngleDegrees 40, MaxBridgesSmall/Medium/Large 3/4/6, BridgeSeparation 300, PlatformClearance 230, SpineMaxGap 7500.
- Collapse: UpdateLeadState every 0.1 s (LandingTolerance 150 cm), bWaitForBridgesBeforeCollapsing and bCollapseOnlyWhenLeadLands true, chains of 15 tiles every 5 s and 0.1 s apart; culling at CullBehindDistance 20000 with at most 4 platform culls per frame.
- Ranking: AMyCharPlayer::BaseChange records the landed platform; AMyGameState ranks players by that platform's SplineDistance.
- Unused code: AMyHexBridgeBuilder, AMyHexPillar, SpawnAnimatedPillar (declared, never defined), GetOriginalEdgeHexes and IsValidHex (no callers).

## Commits it documents

- `e7a2588` Rewrite hex bridge pairing, replicate platforms reliably, rank players by path distance, add Niagara damage numbers and muzzle smoke · [[Redo3 Hex Path and Ranking]], [[Redo3 Hex Bridges]], [[Redo3 Beam and Gun FX]], [[Redo3 Menus and Settings]], [[Redo3 Hex Platform PCG]]
- `a7bd059` Scale hex path, PCG layout space and bridge lengths with one HexRadius · [[Redo3 Hex Path and Ranking]], [[Redo3 Hex Platform PCG]], [[Redo3 Hex Bridges]]
- `5bec016` Qualify bridge trace constants so the unity build finds one definition · [[Redo3 Hex Bridges]]
- `55f9c66` Wire the hex PCG graph to layout space and scale bridge trace offsets · [[Redo3 Hex Platform PCG]], [[Redo3 Hex Bridges]]

## Sections

- Hex Platform Flow
- Key variables

## Made by

- Thread: [[Redo3 Threads#platform and bridge system uptill hex radius scale|platform and bridge system uptill hex radius scale]]
- Explained on: [[Redo3 Hex Path and Ranking]], [[Redo3 Hex Platform PCG]]
- Group: [[Redo3 Artifacts - Hex Path, Bridges and Paving]]
