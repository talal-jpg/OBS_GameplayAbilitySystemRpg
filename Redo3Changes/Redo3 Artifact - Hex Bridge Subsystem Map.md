↑ [[Redo3 Artifacts - Hex Path, Bridges and Paving]] · [[Redo3 Artifacts Index]]

# Hex Bridge Subsystem Map

claude.ai Artifact · diagram/map · updated 2026-10-06 · [open the artifact](https://claude.ai/artifact/6aKynVeFcpfGiyfPhNbGxN)

This page explains UMyHexBridgeSubsystem, the server-only world subsystem that collects hex platforms as PCG spawns them one path segment at a time. It plans which platforms to join and how, spawns one AMyHexBridge per bridge, and tells each platform when its bridges are ready so it may start collapsing. The page has a Mermaid mind map of the inputs, batching, per-batch steps, planner rules, the nine planner passes, the bridge actor, other jobs and settings. It also has a flowchart of one batch with six numbered steps, and a code index with line numbers in MyHexBridgeSubsystem.cpp. Those line numbers come from the uncommitted redo3 working copy on 6 Oct 2026, when the new planner compiled but had not run in the engine. It was committed in e7a2588 (2026-10-07).

## What it shows

- Batching: each AMyHexPlatform calls RegisterPlatform in BeginPlay. The timer is debounced by BuildDelay 0.1 s and capped at MaxBuildDelay 0.5 s, so one segment becomes one batch. Batches less than 1 s apart form a burst, whose bridges stay tentative and get re-planned.
- Per batch: prune dead bridges and platforms; register each new platform with a serial, a chain (tags HexLower/HexUpper), a kind (centre, side, ring, islet), its live tiles and its open edges; run FPlanner::Run; keep, destroy or spawn actors; call NotifyBridgesReady; and write one log line.
- Planner rules: up to 10 edge-pair options per platform pair, costed by length, exit angle, curve and slope. The deck is straight or one Hermite curve. Checks: CrossConflict, ClearOfPlatforms, TraceOk and LensBlocked, with a trace cache per burst. A bridge with a player on or near it is never touched.
- Passes, in order: Spine (along the centre platforms), Tree (connect everything), Loop (alternative routes), Leaf (second bridge for dead ends), Route repair (single-point cuts), Bypass (around a big platform), Branch (secure side branches), then Fallback and Rescue as a last resort.
- AMyHexBridge: FMyHexBridgeEnds holds the start and end plus StartTangent and EndTangent and is replicated once (InitialOnly). ApplyEnds builds the spline, PCG_SplineISMs places planks every 38 cm, and the bridge is destroyed when an end tile collapses.
- Other jobs: the lead pawn for collapse and culling, the lead's distance along the path, and a cull budget of 4 per frame. Settings are in Project Settings > Game > Hex Bridges (MyHexBridgeSettings.h, saved to Config/DefaultGame.ini): caps of 3 small, 4 medium and 6 large, MaxBridgeLength 45 m, slope 30 deg, bCurvedBridges, bAllowCrossChain and bDrawDebug.
- Code index (MyHexBridgeSubsystem.cpp): RegisterPlatform :4755, UnregisterPlatform :4798, ProcessPendingPlatforms :4941, FPlanner::Run :4362, Finish :4435, MakeCandidate :1202, BuildDeck :1422, TraceOk :2055, Spine :3147, Rescue :4283.
- The planner rewrite was committed in e7a2588 ('Rewrite hex bridge pairing, replicate platforms reliably, rank players by path distance...', 2026-10-07); later bridge length scaling came in a7bd059.

## Sections

- Structure
- Flow of one batch
- Where it lives in the code

## Made by

- Thread: [[Redo3 Threads#Natural bridge placement|Natural bridge placement]]
- Explained on: [[Redo3 Hex Bridges]]
- Group: [[Redo3 Artifacts - Hex Path, Bridges and Paving]]
