↑ [[Redo3 Artifacts - Beam, Gun and Mech FX]] · [[Redo3 Artifacts Index]]

# How respawn works

claude.ai Artifact · walkthrough · updated 2026-11-15 · [open the artifact](https://claude.ai/artifact/QHZgKzEX3C3aWjN1jForLz)

A Claude Docs explainer for the server-side respawn system from PR #4, merged into redo3 on Nov 15 (main commit b3b02ac; f84c282 rebuilt NS_Claude_RespawnFX). AMyGameMode watches each pawn from SetPlayerDefaults. Five seconds after a death or a fall out of the world, it spawns a fresh pawn on a tile of the nearest AMyHexPlatform past the lead player's SplineDistance. NS_Claude_RespawnFX plays at that spot for everyone 3 s earlier through AMyGameState::MulticastPlayRespawnFX. The doc covers how the platform and tile are picked, the fit test and fallbacks, how AMyCharPlayer keeps the PlayerState's ability system, the BP_MyGameMode settings, edge cases and a file-by-file code map, and includes a timeline diagram of one respawn.

## What it shows

- Lives in PR #4 (base redo3), merged Nov 15. The main commit is b3b02ac 'Respawn dead players ahead of the lead after a fixed delay' (MyGameMode.cpp +509 lines, plus MyCharPlayer, MyGameState, MyHUD, MyPlayerController2 and MyGameplayTags).
- Death detection: SetPlayerDefaults binds OnDeathDelegate (IMyCombatInterface, broadcast from MulticastHandlePlayerDeath) and OnDestroyed (KillZ). Running abilities are cancelled. A second event while a respawn is pending is ignored, Logout clears it, and ServerRestartPlayer is refused while one is pending.
- Platform choice: the smallest SplineDistance above max(lead distance, GetActivatedSplineDistance()), so the first choice is never a collapsing platform. Ties between the upper and lower chain go to the platform nearer the lead's.
- Tile choice: up to 12 live tiles, tried in this order: bridge-path tiles (PathDistance 0), then EdgeDepth > 0, then nearest the centre. FitPawnOnTile puts the capsule 10 cm above the tile with UWorld::FindTeleportSpot, keeps it on the tile, and keeps it two capsule radii from other pending spots.
- Fallbacks: further platforms, then the lead's own platform, then a PlayerStart.
- Timeline diagram: 0 s the player dies; 2 s the spot is picked and the FX starts; 5 s the dead pawn is destroyed, the tile is re-tested, and the new pawn is spawned with full health and mana. If the tile fell or is blocked, a new spot is picked and the FX plays again.
- Respawned pawns keep the ASC on AMyPlayerState. PossessedBy and OnRep_PlayerState check bAbilitiesGiven and call InitAbilitySystemAfterRespawn. Primary and secondary attribute effects are skipped and the vital one is re-applied. State.Dead is cleared, and the boosting handler is rebound with AddUObject.
- BP_MyGameMode Respawn settings: bRespawnEnabled true, RespawnDelay 5 s, RespawnFX NS_Claude_RespawnFX, RespawnFXLeadTime 3 s, RespawnHeightAboveTile 10 cm. RespawnFX is loaded with ConstructorHelpers so the maps-only cook includes it.
- Known gaps: ability keys can still fire while dead, because only Move and Rotate check State.Dead. On redo3 without the local death work, AMyCharPlayer::Die() is empty. The local DebugDieAfterBeginPlaySeconds (3 s) causes a die-and-respawn loop.

## Sections

- Overview
- Timeline of one respawn
- How a death is detected
- Where the player respawns
- The respawn FX
- Fresh pawn, same PlayerState
- Settings
- Edge cases and known behaviour
- Where it lives in the code

## Made by

- Thread: [[Redo3 Threads#Respawn players ahead of leader|Respawn players ahead of leader]]
- Explained on: [[Redo3 Mech Character]], [[Redo3 Hex Path and Ranking]]
- Group: [[Redo3 Artifacts - Beam, Gun and Mech FX]]
