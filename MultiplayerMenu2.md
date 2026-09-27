
# MainMenu2 / MultiplayerSessionsSubsystem — function flow reference

To give a fresh chat this context, upload this file alongside `MultiplayerSessionTypes.h`, `MultiplayerSessionsSubsystem.h`/`.cpp`, `MainMenu2.h`/`.cpp`, and `SessionEntry2.h`/`.cpp`.

Notation: `[Subsystem]` = `UMultiplayerSessionsSubsystem`, `[MainMenu2]` = `UMainMenu2`, `[Entry]` = `USessionEntry2`. **Async boundary** marks a point where control returns to the engine and the next step only runs once an Online Subsystem delegate fires later.

## CreateSessionWithSettings(HostSettings)

Trigger: `[MainMenu2] StartLobbyButtonClicked()`

1. MainMenu2 reads Difficulty/Rules off the combo boxes into `PendingHostSettings` (Privacy was already set by whichever `PrivacyXClicked` handler last ran), disables `StartLobbyButton`, sets status, calls `[Subsystem] CreateSessionWithSettings(PendingHostSettings)`.
2. Bails with `MultiplayerOnSessionError` if `SessionInterface` isn't valid.
3. Checks `GetNamedSession(NAME_GameSession)` for an existing session (re-hosting case):
    - If one exists: sets `bCreateSessionOnDestroy = true`, `bRecreateWithHostSettings = true`, caches the subsystem's own `PendingHostSettings` copy, calls `DestroySession()`, **returns** (the bugfix — it used to fall through and create on top of the still-live session).
    - `DestroySession()` is async — **async boundary**. When it completes, `OnDestroySessionComplete` sees `bRecreateWithHostSettings == true` and calls `CreateSessionWithSettings(PendingHostSettings)` again, this time hitting the next branch since there's no existing session anymore.
4. Fresh-create branch: `CurrentJoinCode` is generated (or reuses `HostSettings.JoinCode` if supplied), builds `FOnlineSessionSettings` with `bAllowJoinViaPresenceFriendsOnly` set only for `FriendsOnly` privacy, writes `GameName`/`MatchType`/`Difficulty`/`Rules`/`JoinCode`/`Listed` via `AuraSessionKeys`, calls `SessionInterface->CreateSession(...)`.
5. **Async boundary.** OSS calls back `OnCreateSessionComplete` → broadcasts `MultiplayerOnCreateSessionComplete(bWasSuccessful)`.
6. `[MainMenu2] OnCreateSession(bWasSuccessful)`: on success, reads `GetCurrentJoinCode()` into `JoinCodeDisplayText`, then `World->ServerTravel(PathToLobby)` — the actual level change. On failure, sets status text and re-enables `StartLobbyButton`.

## FindSessionsFiltered(Filter)

Trigger: `RefreshSessionsButtonClicked` (also reached via `ApplyFiltersButtonClicked` and `BrowseSessionsButtonClicked`, which both just call it), and internally from `JoinSessionByCode`.

1. MainMenu2 disables Refresh/ApplyFilters, sets status, calls `[Subsystem] FindSessionsFiltered(BuildFilterFromWidgets())`.
2. Bails with `MultiplayerOnSessionError` if `bSearchInProgress` is already true — this is what stops a concurrent search from silently eating an accepted Steam invite.
3. Caches `CurrentSessionFilter`, sets `bPendingCodeJoin = !Filter.JoinCode.IsEmpty()` (false for a normal browse), builds `FOnlineSessionSearch` with server-side `Difficulty`/`Rules`/ `JoinCode` filters only where the filter actually specifies them, sets `bSearchInProgress = true`, calls `SessionInterface->FindSessions(...)`.
4. **Async boundary.** `OnFindSessionsComplete(bWasSuccessful)` fires, clears `bSearchInProgress`.
    - Zero raw results → broadcasts empty on both `MultiplayerOnFindSessionsComplete` (old path, untouched) and `MultiplayerOnSessionsFound` (new path); if this was a code lookup, also broadcasts the "no session found with that code" error.
    - Otherwise: runs the untouched old GameName filter for `UMainMenu`, then separately builds `FAuraSessionInfo` rows — checking `Listed`, code match, `bOnlyJoinable`, the ping-bucket cap for `Distance`, `Difficulty`, `Rules` — and broadcasts `MultiplayerOnSessionsFound(SessionInfos, bWasSuccessful)`.
    - If `bPendingCodeJoin` was true: calls `JoinCachedSession()` on the surviving match, or broadcasts the "not found" error if nothing survived.
5. `[MainMenu2] OnSessionsFound`: re-enables the two buttons. If `bAwaitingCodeJoin` is true it stops right there — the code-join flow owns the UI. Otherwise it rebuilds `SessionListPanel` and sets the "Found N"/"No lobbies" status.

## JoinSessionByCode(JoinCode)

Trigger: `ConfirmJoinCodeButtonClicked`.

1. MainMenu2 cleans the code, sets `bAwaitingCodeJoin = true`, disables `ConfirmJoinCodeButton`, calls `[Subsystem] JoinSessionByCode(Code)`.
2. Subsystem re-cleans the code, builds a filter with only `JoinCode` set, calls `FindSessionsFiltered(Filter)` — **re-enters the flow above**, ending with `bPendingCodeJoin == true`.
3. Everything from `FindSessionsFiltered` step 4 onward runs. The outcome lands as either a call into `JoinCachedSession` (→ eventually `OnJoinSession`) or a `MultiplayerOnSessionError` broadcast (→ `OnSessionError`). Those two handlers — not `OnSessionsFound` — are what clear `bAwaitingCodeJoin` and re-enable `ConfirmJoinCodeButton`.

## JoinCachedSession(ResultIndex)

Trigger: `[Entry] JoinEntryButtonClicked()` → `[MainMenu2] JoinLobbyByIndex(ResultIndex)`, or internally from the code-join branch above.

1. Validates `ResultIndex` against `CachedResults`; broadcasts an error if stale.
2. Calls the pre-existing `JoinSession(CachedResults[ResultIndex])` — the same method the untouched `UMainMenu` and Steam invites both use.
3. **Async boundary.** `OnJoinSessionComplete(Result)` fires → broadcasts `MultiplayerOnJoinSessionComplete(Result)`.
4. `[MainMenu2] OnJoinSession(Result)`: clears `bAwaitingCodeJoin`, re-enables `ConfirmJoinCodeButton`. Switches on `Result` — anything but `Success` sets a specific status message and stops. On `Success`: `GetResolvedConnectString` → `PlayerController->ClientTravel(...)` — the actual join.

## ShowSteamInviteUI()

Trigger: `SteamInviteButtonClicked`.

1. Gets `IOnlineExternalUIPtr`, calls `ShowInviteUI(0, NAME_GameSession)` — pops the Steam overlay. The bool it returns only reflects whether the overlay opened, not whether anyone accepted.
2. Dead end on the host's own delegate chain. What happens when a friend accepts is a separate flow, on the **invited player's own client**: `OnSessionUserInviteAccepted` (bound once, in `Initialize()`) fires, calls `JoinSession(InviteResult)` — rejoining `JoinCachedSession`'s flow at its step 2 — and broadcasts `MultiplayerOnInviteAccepted` → `OnInviteAccepted` sets status text.

## GetCurrentJoinCode() / IsSearchInProgress()

Plain getters, no async chain:

- `GetCurrentJoinCode()` returns `CurrentJoinCode`, only ever written inside `CreateSessionWithSettings`. Read by `OnCreateSession` (populate the display) and `CopyJoinCodeButtonClicked` (clipboard).
- `IsSearchInProgress()` returns `bSearchInProgress`, set true by both `FindSessions` (old) and `FindSessionsFiltered` (new), always reset false at the top of `OnFindSessionsComplete`. Read as a guard by `RefreshSessionsButtonClicked`.

## State flags cheat sheet

| Flag                        | Owner     | Set true by                                                                | Cleared by                         | Meaning                                                                                                    |
| --------------------------- | --------- | -------------------------------------------------------------------------- | ---------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `bCreateSessionOnDestroy`   | Subsystem | `CreateSession`/`CreateSessionWithSettings`, when a session already exists | `OnDestroySessionComplete`         | recreate after this destroy finishes                                                                       |
| `bRecreateWithHostSettings` | Subsystem | `CreateSessionWithSettings`, when a session already exists                 | `OnDestroySessionComplete`         | the recreate should use `CreateSessionWithSettings(PendingHostSettings)`, not the old `CreateSession(...)` |
| `bSearchInProgress`         | Subsystem | `FindSessions` / `FindSessionsFiltered`                                    | `OnFindSessionsComplete` (always)  | guards against overlapping searches / invite-eating                                                        |
| `bPendingCodeJoin`          | Subsystem | `FindSessionsFiltered`, when `Filter.JoinCode` is non-empty                | `OnFindSessionsComplete` (always)  | auto-join whatever this search finds instead of just listing it                                            |
| `bAwaitingCodeJoin`         | MainMenu2 | `ConfirmJoinCodeButtonClicked`                                             | `OnJoinSession` / `OnSessionError` | tells `OnSessionsFound` to step aside — the Code panel owns the UI right now                               |

## Delegate → listener map

| Delegate                              | Broadcast from                      | MainMenu2 handler               | Binding      |
| ------------------------------------- | ----------------------------------- | ------------------------------- | ------------ |
| `MultiplayerOnCreateSessionComplete`  | `OnCreateSessionComplete`           | `OnCreateSession`               | `AddDynamic` |
| `MultiplayerOnFindSessionsComplete`   | `OnFindSessionsComplete` (old path) | — (`UMainMenu` only)            | —            |
| `MultiplayerOnSessionsFound`          | `OnFindSessionsComplete` (new path) | `OnSessionsFound`               | `AddUObject` |
| `MultiplayerOnJoinSessionComplete`    | `OnJoinSessionComplete`             | `OnJoinSession`                 | `AddUObject` |
| `MultiplayerOnDestroySessionComplete` | `OnDestroySessionComplete`          | `OnDestroySession` (no-op stub) | `AddDynamic` |
| `MultiplayerOnStartSessionComplete`   | `OnStartSessionComplete`            | `OnStartSession` (no-op stub)   | `AddDynamic` |
| `MultiplayerOnInviteAccepted`         | `OnSessionUserInviteAccepted`       | `OnInviteAccepted`              | `AddDynamic` |
| `MultiplayerOnSessionError`           | any guard/failure branch above      | `OnSessionError`                | `AddDynamic` |
