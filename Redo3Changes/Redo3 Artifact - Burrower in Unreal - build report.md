↑ [[Redo3 Artifacts - Enemies]] · [[Redo3 Artifacts Index]]

# Burrower in Unreal: build report

claude.ai Artifact · report · updated 2026-11-21 · [open the artifact](https://claude.ai/artifact/MHNAj2dw7RMVVzB9vC9HfJ)

A Claude Docs report on importing, rigging and play-testing the Burrower enemy in UE 5.8. The work was pushed to branch claude/project-thread-78otz4 in commits 5d5f2bc, a52a516 and 9727f1d, on top of 7fa2585 (the UE skeletal mesh FBX) and a414ca9 (BurrowerCharacter, the per-player spawner, rotor tile cutting and BurrowerAnimInstance). It was built in a separate copy (_BurrowerWork) so the user's AuraRedo3 working copy was not touched. The report lists every asset in /Game/Assets/Enemies/Burrower/ and gives the results of a listen-server plus one-client PIE test on StartupMap. It also records where the work departed from the recipe and what is still open. The branch is unmerged: it starts from redo3 before PR #7, so it needs a merge through a pull request.

> [!note] Since this report
> The branch was merged into redo3 in PR #8 (`2c7d311`, 2026-11-22 06:48), after redo3 with PR #7 was merged into it (`dacbbda`). See [[Redo3 Burrower]] and [[Redo3 Branches and PRs#The burrower branch]].

## What it shows

- Branch claude/project-thread-78otz4, commits 5d5f2bc (SK_Burrower, PA_Burrower, settings, editor scripts), a52a516 (CR_Burrower, ABP_Burrower, BP_Burrower) and 9727f1d (PIE tested, Control Rig ragdoll on). Not merged into redo3.
- SK_Burrower / SKEL_Burrower from Burrower_UE.fbx: 71 bones, root with children J0_Head and Rotor, all within 0.1 cm of Blender. The mesh is 2023 cm long with all 45 parts in one skinned mesh (217 MB in LFS) and 1,232,914 triangles.
- PA_Burrower: 5 capsules on J0_Head to J4_Tail and 4 constraints limited to 20 degrees. The Rotor has no body, so it cannot fall off in the mesh ragdoll.
- CR_Burrower Control Rig: body wave on J1 to J4, rotor spin on J0_Head, rigid-body upper hoses, wire cables and side hoses, and a ragdoll from PA_Burrower when bIsDead. 124 of 124 numeric checks pass.
- ABP_Burrower runs on BurrowerAnimInstance with all 9 rig variables wired. BP_Burrower runs on BurrowerCharacter and uses the Control Rig ragdoll; the C++ mesh ragdoll fallback is off.
- DefaultGame.ini sets Burrower Class to BP_Burrower and adds the folder to DirectoriesToAlwaysCook. The build scripts are in Docs/Burrower/scripts/ (for example bw_02_nanite.py and bw_05b_use_pin_ui.sh).
- PIE results: one Burrower per player spawns 30 m behind and closes to about 11 m. The body wave reaches 289 cm sideways at the tail and the rotor spins at 720 deg/s. 31 tiles were cut, with the same counts on server and client. Gunfire took health from 220 to 217. Aura.Burrower.KillAll ragdolls it on both machines, and a new one arrives after 8 s.
- Deviations: the legacy FBX importer was used instead of Interchange. UE 5.8's double Sin/Cos Control Rig nodes were switched back to float. 'Use Pin' was ticked through UI automation, and recipe step 5.2 (speed gate) was skipped.
- Still open: the corpse falls through the tiles after death; the FBX has no UVs; Nanite is off; four DarkerNodes plugin materials are deliberately left uncommitted.

## Sections

- Result
- Play test
- Deviations from the recipe
- Still open

## Made by

- Thread: [[Redo3 Threads#Model, rig and export the burrower creature|Model, rig and export the burrower creature]]
- Explained on: [[Redo3 Burrower]]
- Group: [[Redo3 Artifacts - Enemies]]
