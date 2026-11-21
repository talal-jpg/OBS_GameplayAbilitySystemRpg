↑ [[Redo3 Artifacts - Hex Path, Bridges and Paving]] · [[Redo3 Artifacts Index]]

# Lead-Landing Collapse Map

claude.ai Artifact · diagram/map · updated 2026-10-04 · [open the artifact](https://claude.ai/artifact/CSc2msP2LYK5pFVdS3oWvQ)

An interactive page mapping the changes that stop hex platforms from collapsing the moment they spawn. A platform now waits until its bridges exist and the lead player has landed on it. Once the lead is far enough ahead, platforms and path segments left behind are removed, unless a player is still on them. A clickable SVG mind map groups the changes by six classes (AMyHexPlatform, UMyHexBridgeSubsystem, AMyHexBridge, AMyHexPathSpawner, AMyHexPathSegment, UMyHexBridgeSettings), and each leaf is tagged new, changed or fix and has a detail panel. The page also has a seven-step flow of one platform's life, the collapse and cull gates, a table of new tunables and a list of open questions. The changes were replayed on redo3 at 9c04634 (10 files, +727/-15), uncommitted and not play-tested; they later appear in e7a2588.

## What it shows

- Root cause: step 3 of UMyHexBridgeSubsystem::ProcessPendingPlatforms() called StartCollapsing() about 0.1 s after a platform spawned. It now calls NotifyBridgesReady() instead.
- Collapse gate: TryStartCollapsing() requires bBridgesReady && (bLeadLanded || !bCollapseOnlyWhenLeadLands). NotifyLeadLanded() fires when the lead's movement base is the platform, or when an airborne lead's downward trace (half-height + LandingTolerance 150 cm) hits it. Both latches are server-only and fire once.
- AMyHexPlatform::UpdateLeadState runs every LeadCheckInterval (0.1 s) with a random first delay. IsLeftBehind() needs the lead more than 200 m (CullBehindDistance 20000) from the footprint edge and the platform's front more than 200 m behind along the path; MaxPathOffset is 10000.
- Keep-alive: IsAnyPlayerOnPlatform() counts a player standing on the platform, on a bridge ending on it, inside its footprint, or within BridgeKeepDistance (300 cm) of one of its bridges (the mid-bridge jump fix). TryConsumeCullBudget() allows 4 culls per frame (MaxPlatformCullsPerFrame).
- AMyHexPathSpawner: CheckPlayers runs every 0.25 s and extends the path when a player is within 150 m of its end. DestroyPassedSegments removes segments more than 300 m behind the lead (DestroySegmentsBehindDistance 30000). NextSegmentIndex++ gives unique PCG seeds, and RegisterPathSpline hands the path to the subsystem. AMyHexPathSegment gains PathEndDistance.
- AMyHexBridge binds to both end platforms' OnEndPlay (HandleEndPlatformEndPlay), registers itself in their bridge lists (RegisterBridge/UnregisterBridge), and destroys itself in BeginPlay if an end platform was already culled.
- UMyHexBridgeSubsystem adds SelectLeadPawn/GetLeadPawn (the first player controller's pawn, cached per frame), GetLeadPathDistance, a path spline registry and CheckedPairs pruning on unregister. UMyHexBridgeSettings only got a new BuildDelay tooltip.
- Open items: in multiplayer the lead is always the host, the path spline is never trimmed, and the debug PrintString calls in BeginPlay remain. Base was redo3 9c04634; NotifyBridgesReady and DestroyPassedSegments were committed in e7a2588 (2026-10-07).

## Sections

- Structure
- Flow of one platform
- The two gates
- New tunables
- Still open

## Made by

- Thread: [[Redo3 Threads#Mind map of hex collapse changes|Mind map of hex collapse changes]]
- Explained on: [[Redo3 Hex Path and Ranking]], [[Redo3 Hex Bridges]]
- Group: [[Redo3 Artifacts - Hex Path, Bridges and Paving]]
