↑ [[Redo3 Artifacts - Menus]] · [[Redo3 Artifacts Index]]

# Main Menu 3 Look

claude.ai Artifact · other · updated 2026-10-30 · [open the artifact](https://claude.ai/artifact/8P51YGjCsdb8Y3y6SRSPYy)

An interactive HTML mock-up of the MainMenu3 visual style. It follows Persona 5's red-and-black design rules with yellow in place of red, using Aura's own motifs: hex platforms, the mech silhouette, halftone fog dots and hazard stripes. A 1920x1080 stage, the size of the game's UMG canvas, switches between seven screens (Title, Home, Create Session, Browse Sessions, Join With Code, Options and Pause), with an offline-mode toggle and keyboard focus, Enter and Esc navigation; lobbies, names and pings are example data. Below the stage, a kit section gives the 12-colour palette, the five free fonts and six design rules. The drawing functions stand in for planned UMG assets (M_UI_Plate, M_UI_Slab, M_UI_Burst, WBP_RansomWord, WBP_OptionRow). The menu code landed in c4b51bd, 14ed4e1, f2ddd92 (which adds Docs/MainMenu3_Style.md) and 477dba4, with widgets and OFL fonts in d11e55a.

## What it shows

- Palette: Yellow #FFD400 (key), Yellow Hot #FFE433 (hover), Yellow Press #E6B800, Yellow Dim #8A7300 (unselected), Ink #0B0B0B, Coal #161616, Coal 2 #222222, Paper #FFFFFF (focus accent), Grey #6E6E6E (disabled), Grey Light #B4B4B4, Error #FF4D3A and Good #38E07B (on Ink only).
- Fonts: Anton (display, skewed 12 degrees), Bebas Neue (tags), Barlow Condensed (body), Rubik Mono One (join codes). Mixed 'ransom' letters come from Anton, Archivo Black, Alfa Slab One, Bebas Neue and one Abril Fatface.
- Rules: text on yellow is always black; white never touches yellow (at least 4 px of black between); game-written text stays upright in Barlow; no gradients or plain rectangles (plates lean 12 degrees, fades are halftone dots on a hex grid); focused items move, selected ones sit still; nothing is copied from Persona (the burst has 16 points).
- Home: CREATE SESSION, BROWSE SESSIONS, TUTORIAL, OPTIONS, CREDITS, QUIT, with callouts HOST / SCAN / DRILL / TUNE / CREW / POWER OFF and a hex lighting under the mech. Pause: RESUME, CURRENT SESSION, OPTIONS, LEAVE SESSION, QUIT GAME over the live game.
- Create Session: privacy tags PUBLIC / FRIENDS / CODE ONLY, lobby name, max players 1-4, difficulty Easy to Nightmare, START LOBBY and PLAY SOLO. Browse: Distance / Difficulty / Rules filters, SHOW FULL, QUICK JOIN, REFRESH, JOIN WITH CODE, FRIENDS. Join With Code: six slots, CONFIRM unlocks at six characters.
- Options tabs VIDEO / AUDIO / CONTROLS / GAMEPLAY / ACCESSIBILITY with LB/RB, an ABOUT THIS hint card, and BACK / RESET TAB / AUTO DETECT / RESET KEY BINDINGS chips. Key rows include Boost / Grapple L SHIFT, Shoot LMB, Beam RMB, Attribute menu Q, Spell menu E and Jump / Hover SPACE.
- Some mock-up ranges differ from the setup guide: FOV 70-110 (guide 70-120), look sensitivity 0.1-3 (guide 0.10-5.00), UI scale 0.8-1.5x (guide 75-150%).

## Sections

- Main Menu 3 (title and lede)
- Screen picker and 1920x1080 stage
- Palette
- Type
- Rules

## Made by

- Thread: [[Redo3 Threads#Build full main menu system|Build full main menu system]]
- Explained on: [[Redo3 Menus and Settings]]
- Group: [[Redo3 Artifacts - Menus]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Main Menu 3 setup guide]]
