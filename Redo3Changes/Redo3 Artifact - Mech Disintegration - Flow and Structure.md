↑ [[Redo3 Artifacts - Beam, Gun and Mech FX]] · [[Redo3 Artifacts Index]]

# Mech Disintegration: Flow and Structure

claude.ai Artifact · walkthrough · [open the artifact](https://claude.ai/artifact/RfDZHVmEaWj9bGu8RQ4Q8D)

A Claude Docs document dated 2026-10-23 describing the reworked (v3, cube kick) mech death effect. When a player's health reaches 0 the mech is eaten away by noisy holes over 1.6 s while about 2,600 glowing cubes are born on the surface, kicked sideways, fall and pile up; the pile darkens from 2.0 to 3.25 s and the cubes rise and shrink away from 2.9 to 5.3 s, with everything gone at 5.6 s on every machine. It traces the call chain from UMyAttributeSet damage on the server through the death multicast to UMyAnimInstance, StartDisintegrate and SwapToDisintegrateMaterial, and explains the MF_MechDisintegrate eat maths and the three body materials. It then tables the seven Niagara emitters and user parameters, lists where every asset, C++ file and build script lives, and covers knobs, gotchas, known limits and a PIE test recipe. Two diagrams are embedded: a 9-part death-effect timeline and a 6-step death call chain.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Timeline: holes open at about 0.16 s and the body is gone by 1.55 s (EatDuration 1.6 s); pile darkens 2.0-3.25 s; cubes rise 2.9-5.3 s; total 5.6 s; the CR_Mech control-rig ragdoll falls during the eat.
- Call chain: UMyAttributeSet::PostGameplayEffectExecute -> AMyCharPlayer::Die (server, skips AMyCharBase's physics ragdoll) -> MulticastHandlePlayerDeath sets bIsDead and broadcasts OnDeathDelegate -> UMyAnimInstance::OnDeathCallback stores DeathVelocity -> StartDisintegrate -> SwapToDisintegrateMaterial.
- DisintegrateDelay and DisintegrateSwapDelay now default to 0 s; StartDisintegrate spawns NS_Claude_Disintegrate not auto-activated, sets User.DeathVelocity, then activates it, and writes world time into Custom Primitive Data slot 0.
- Materials: slots based on M_MasterMech get a dynamic instance of the new masked M_MasterMechDissolve with copied parameters; other slots get M_MechDisintegrateBody; M_MasterMechDissolve uses a MainLightDirection parameter (StartupMap sun 0.9842, -0.0344, 0.1736) because masked materials cannot compile the Main Directional Light node.
- MF_MechDisintegrate maths: p = saturate(age / EatDuration); n = saturate((noise - 0.5) * 2.4 + 0.5); mask = n + 0.45 - 1.15 p clipped at 0.333; RimGain 25 for the cyan hole rims.
- Niagara: seven emitters in one 5.6 s shot (Body_Cubes, Black_Spray, Body_Sparks, Arcs, Pile_Flicker, Pile_Glow, Pile_Light with radius up to 450); the four pile emitters run in world space so the pile stays where it fell.
- Cube kick: vertical velocity clamped to -200..100 cm/s plus a horizontal kick of User.KickSpeed 180 cm/s x (1 + KickSpeedGain 0.6 x speed / KickRefSpeed 600, capped) x random 0.55-1.45, direction switching to movement above KickDirSpeed 120 cm/s, spread about +-29 degrees.
- Backups NS_Claude_Disintegrate_BeforeKick (v2) and NS_Claude_Disintegrate_V1; build with cat lib.py dis_v1.py dis_v2.py dis_v3.py; mf_dis2.py builds the function; _dis_touch.py re-sets expressions after a build.
- Gotcha: wired material pins can stay overridden by saved constants; the console command Aura.ClearConnectedRootConstants (AuraMaterialFixCommand.cpp) fixes it. The dying-while-running slide was fixed on 23 Oct in CR_Mech (Death_SpaceMotion) plus a State.Dead tag that blocks move and rotate input.
- Testing: DebugDieAfterBeginPlaySeconds = 3 on BP_MyCharMech, console 'ke <pawn> MulticastHandlePlayerDeath', or Saved/BeamClaudeWork/rec_death2.sh <speed> <out.mp4> with ARM, ANG, PRE_S, KEEP options; the DeathVelocity C++ change was built but not committed when written.

## Commits it documents

- `2c1d78b` Mech death: disintegrate FX, ragdoll, no move or look while dead · [[Redo3 Mech Character]]
- `9dd0d5d` MyCharBase: skip null startup abilities, set bIsDead on all machines · [[Redo3 Mech Character]]
- `ce1125d` Add editor console commands for scripted material edits · [[Redo3 Engine Tools and Assets]]

## Sections

- Overview
- Flow: from the killing hit to the effect
- Materials: how the body is eaten
- Niagara system: NS_Claude_Disintegrate
- Where everything lives
- Tuning knobs, gotchas and limits
- How to test it in PIE

## Made by

- Thread: [[Redo3 Threads#Beam and gunshot Niagara effects|Beam and gunshot Niagara effects]]
- Explained on: [[Redo3 Mech Character]]
- Group: [[Redo3 Artifacts - Beam, Gun and Mech FX]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Mech FX Reels]]
- [[Redo3 Artifact - Mech Disintegration Effect]]
