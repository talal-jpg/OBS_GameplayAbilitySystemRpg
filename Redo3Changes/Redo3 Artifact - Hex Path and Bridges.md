↑ [[Redo3 Artifacts - Hex Path, Bridges and Paving]] · [[Redo3 Artifacts Index]]

# Hex Path and Bridges

claude.ai Artifact · walkthrough · [open the artifact](https://claude.ai/artifact/NfkiqDnruAtiAWCZmhkPJm)

A page (tagged 'AuraRedo3 · redo3 · uncommitted') explaining how the growing path spline spawns hex platforms through PCG, how the bridge subsystem connects them before they start collapsing, and how each bridge hears about its end tiles collapsing. It introduces four new classes and one settings object: AMyHexPathSpawner, AMyHexPathSegment, UMyHexBridgeSubsystem, AMyHexBridge and UMyHexBridgeSettings, with a table of why each has its type. Mermaid diagrams cover the structure, a sequence diagram of one extension end to end, and four flows: growing the path, platform start-up, building bridges and tile collapse to bridge, each with 'In code' file and line references. A settings table lists defaults for the spawner, Hex Bridges project settings, the bridge actor and the platform. It describes the first version of the system, later committed in 17605e3.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- The spawner and its segments exist only on the server; AMyHexPlatform and AMyHexBridge replicate, and each machine runs the bridge's PCG graph itself because PCG output does not replicate.
- Path growth: a timer every CheckInterval checks whether any pawn is within ExtendDistance (2D) of the last path point; ExtendPath adds PointsPerSegment points with yaw = start yaw + noise x MaxYawDeviation and z = start z + noise x HeightNoiseAmplitude, then spawns a segment whose InitSegment seeds PCG with the segment index.
- Platforms no longer start collapsing in BeginPlay: they RegisterPlatform with the subsystem and, with bWaitForBridgesBeforeCollapsing, wait for StartCollapsing (first chain after 1 s, then every 5 s).
- Bridge building compares each new platform only with platforms in nearby spatial grid cells, each pair once, up to 4 x 4 = 16 facing edge pairs shortest first, checking MaxBridgeLength and slope (abs(dZ) / length <= sin(MaxSlopeDegrees)) before line traces; one bridge per pair via deferred SpawnBridge.
- Edges are stored by tile coordinate, not instance index, because removing an instance moves the last one into its slot.
- Tile collapse: ActivateHex adds to replicated ActivatedTiles, ActivateHexLocal broadcasts OnHexTileActivated on every machine, AMyHexBridge::OnPlatformTileActivated calls HandleEndTileCollapsed and the server destroys the bridge (or SetLifeSpan(DestroyDelay)).
- Defaults: SegmentLength 10000 cm, PointsPerSegment 4, ExtendDistance 15000 cm, MaxYawDeviation 60 deg, YawNoiseFrequency 0.0001, HeightNoiseAmplitude 0, MaxSlopeDegrees 30, CandidateEdgesPerPlatform 4, NeighborRadiusMultiplier 3, bDrawDebug off.
- Code references such as MyHexBridgeSubsystem.cpp:115 ProcessPendingPlatforms, :248 TryBridgePlatforms, :441 TraceReaches, MyHexBridge.cpp:147 OnPlatformTileActivated, matching the uncommitted redo3 working tree.

## Commits it documents

- `17605e3` Add growing hex path, PCG platform segments and bridge subsystem · [[Redo3 Hex Path and Ranking]], [[Redo3 Hex Bridges]], [[Redo3 Hex Platform PCG]]

## Sections

- Structure
- One extension, end to end
- Growing the path
- Platform start-up
- Building bridges
- Tile collapse to bridge
- Settings

## Made by

- Thread: [[Redo3 Threads#Bring pinned artifacts into project|Bring pinned artifacts into project]]
- Explained on: [[Redo3 Hex Path and Ranking]], [[Redo3 Hex Bridges]]
- Group: [[Redo3 Artifacts - Hex Path, Bridges and Paving]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Hex Path Setup Steps]]
- [[Redo3 Artifact - Aura Spell Menu Flow]]
