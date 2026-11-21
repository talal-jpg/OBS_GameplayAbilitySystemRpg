↑ [[Redo3 Artifacts - Menus]] · [[Redo3 Artifacts Index]]

# Main Menu 3 setup guide

claude.ai Artifact · guide · updated 2026-10-30 · [open the artifact](https://claude.ai/artifact/P1fvxL7BRYm7cpPegmvyud)

A Claude Docs setup guide for the MainMenu3 front end, built to match Far Far West's menu options where the project has a backing system. The C++ classes are MainMenu3, SettingsMenu3, PauseMenu3, SessionEntry3, KeyBindingRow3 and SessionPlayerRow3. The guide starts with a parity table of what was built and what was left out. It then gives the exact widget names to bind in WBP_MainMenu3, WBP_SettingsMenu3 and WBP_PauseMenu3, with a mock-up figure and behaviour notes for each panel and options tab. It closes with the editor steps that connect the C++ to assets, how each session flow runs through MultiplayerSessionsSubsystem, and known limits. The C++ came in c4b51bd, 14ed4e1, f2ddd92 and 477dba4 on 2026-10-30; the hook-up (5d2fd5b) and widgets (d11e55a) followed on 2026-11-15.

## What it shows

- Not built: mission select, hub town, save backups, Nanite toggle, DLSS/FSR/frame generation, ADS options, polling rate, voice chat, and the Backups/Twitch tabs. Added beyond FFW: Credits, Quit confirm, colour blind mode, UI scale, subtitles.
- WBP_MainMenu3 uses one Widget Switcher, MenuSwitcher, with TitlePanel, HomePanel, HostPanel, BrowsePanel, CodePanel, BusyPanel, ConfirmPanel, OptionsPanel and CreditsPanel. Every binding is optional. A Graffiti_ Image with M_UI_Button gives MainMenu2's hover effect.
- Join codes are 6 characters from A-Z without I and O plus 2-9 (JoinCodeLength in MultiplayerSessionsSubsystem.h). SessionEntry3 colours ping green up to GoodPingMs 60, yellow up to OkPingMs 120, red above. Credits scroll at CreditsScrollSpeed (40 px/s) and loop.
- Offline: Browse Sessions and Start Lobby grey out and OnlineStatusText turns red. Play Solo opens the lobby map as an offline listen server; Pause > Current Session > Open Lobby turns it into a session later.
- Options apply immediately. Window mode and resolution revert after DisplayConfirmSeconds (15) unless kept. Closing saves GameUserSettings.ini and key bindings, and LB/RB switch tabs. Look sensitivity 0.10-5.00; FOV 70-120 (default 90) applied as an offset from 90, read live by AMyPlayerController2; UI scale 75-150%, not applied in the editor.
- Pause menu: the co-op game keeps running underneath. SessionRefreshSeconds is 1. Only the host can change privacy (in place) and kick or ban; a ban is refused in PreLogin until a new lobby. A host leaving tells clients 'The host closed the lobby.'
- Editor setup: in MM_MainMenu create WBP_MainMenu3 and call MenuSetup; set PauseMenuClass = WBP_PauseMenu3 and IA_Pause on the AMyPlayerController2 blueprint; map IA_Pause to Escape and Gamepad Special Right in IMC_PlayerInputMappingContext.
- More editor setup: give each rebindable mapping Player Mappable Key Settings and add the IMC under Project Settings > Game > Aura Menu. Create a Sound Mix and Master/Music/Effects/Voice/Interface sound classes. MySettingsSubsystem applies settings at startup.
- Known limits: Friends Only lobbies can't be found by code. 'Allow join in progress' has no checkbox because AMyGameMode::PostLogin starts the session at once. The guide was written without an Unreal build in the loop.

## Sections

- What was built
- Main menu widget (WBP_MainMenu3): Title screen, Home panel, Create Session panel, Browse Sessions panel, Join with Code panel, Busy and Confirm overlays, Credits panel
- Options widget (WBP_SettingsMenu3): Video, Audio, Controls, Gameplay and Accessibility tabs
- Pause menu widget (WBP_PauseMenu3): Pause menu, Current Session tab, Pause menu Confirm panel
- Editor setup
- How the flows behave

## Made by

- Thread: [[Redo3 Threads#Build full main menu system|Build full main menu system]]
- Explained on: [[Redo3 Menus and Settings]]
- Group: [[Redo3 Artifacts - Menus]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Main Menu 3 Look]]
