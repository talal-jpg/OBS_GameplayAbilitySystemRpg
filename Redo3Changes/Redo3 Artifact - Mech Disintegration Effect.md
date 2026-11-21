↑ [[Redo3 Artifacts - Beam, Gun and Mech FX]] · [[Redo3 Artifacts Index]]

# Mech Disintegration Effect

claude.ai Artifact · guide · [open the artifact](https://claude.ai/artifact/ECmYYrJQAPoWGJym7J8tvv)

A Claude Docs reference document dated 2026-10-21 for the mech death effect: when the mech dies it holds still for 2 s, darkens, breaks into small glowing cubes that collapse into a pile, and the pile rises away as a thin column, about 4.5 s after the effect starts (verified in PIE on 2026-10-21). The look is copied from a Gnomon livestream (YouTube br4lhl6UAPk at 25:39.9). It documents the three cooperating parts: C++ in UMyAnimInstance (death delegate, timers, Niagara spawn, Custom Primitive Data clock, material swap), the Niagara system NS_Claude_Disintegrate with six emitters, and the material function MF_MechDisintegrate used by M_MasterMech and M_MechDisintegrateBody. It also lists tuning knobs, a PIE and editor-world test recipe, existing captures, a known-issues table, assets, source files, build scripts and a dated history. A timeline diagram shows the disintegration stages in seconds after death.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Death path: AMyCharPlayer::Die() -> MulticastHandlePlayerDeath (NetMulticast, Reliable) -> OnDeathDelegate -> UMyAnimInstance::OnDeathCallback -> DisintegrateDelay timer (2 s) -> StartDisintegrate; nothing of the disintegrate state is replicated.
- StartDisintegrate spawns NS_Claude_Disintegrate with SpawnSystemAttached on the skeletal mesh, writes the world time into Custom Primitive Data index 0 (also on attached meshes like the jetpack), and after 0.25 s SwapToDisintegrateMaterial puts M_MechDisintegrateBody on all 20 slots of BP_MyCharMech.
- NS_Claude_Disintegrate plays once for 4.7 s with six emitters: Black_Spray, Body_Sparks, Body_Cubes (burst of 2600 cubes 1.4-3.8 cm at 0.25 s), Pile_Flicker, Pile_Glow (CPU, 220 cm bloom plus point light up to 380 cm) and Arcs (5 ribbons re-randomised 15 times a second).
- The four mesh-sampling emitters use SkeletalMeshLocation with Surface (Triangles) sampling (Bones gave only about 10 clusters) and run on the GPU with fixed bounds -400..400 cm XY and -50..600 cm Z, because SKM_Mech lacks Allow CPU Access.
- MF_MechDisintegrate reads DisintegrateTime from Custom Primitive Data 0 (0 = alive), samples Simplex noise on PreSkinnedPosition, ramps Darken 0-1 over 0-0.25 s, adds cyan sparkles (SparkleGain 60, SparkleColor 0.3, 0.85, 1.0) and drives OpacityMask.
- M_MasterMech stays Opaque; the swap to a Masked material exists because a Masked M_MasterMech ignored its mask, and the cause is unknown (the blamed bCanMaskedBeAssumedOpaque flag was not found). With clip value 0.3333 the body is gone by about 0.41 s instead of the designed 0.5 s.
- Testing: the debug hook DebugDieAfterBeginPlaySeconds (3 s) kills the mech in plain PIE; the screen is recorded with ffmpeg x11grab because CaptureViewport returns a stale frame during PIE; recompiling the system per capture ran the editor out of memory on 2026-10-21.
- Known issues: an uncommitted Tick check in MyCharPlayer.cpp:182-186 kills every mech after 100 s with no authority check; User.HitDirection is never set; no death pose during the 2 s delay; CR_Mech never reads bIsDead; GetSystemCompileState hangs for 120 s.
- Build scripts in Saved/BeamClaudeWork run through mcp.py: dis_v1.py, arcs_em.py, fixsamp.py, togpu.py, mat_dis.py, mf_dis.py, body_mat.py, stages.py.
- History: system and particle materials built 2026-10-14, committed 2026-10-16; MF_MechDisintegrate, the C++ hook, the material swap and the 2 s delay added 2026-10-21 (not committed when written).

## Commits it documents

- `808d60c` swing phase anim , Hex tile edge mask , beam by claude · [[Redo3 Mech Swing Animation]], [[Redo3 Hex Look and Atmosphere]], [[Redo3 Beam and Gun FX]]
- `2c1d78b` Mech death: disintegrate FX, ragdoll, no move or look while dead · [[Redo3 Mech Character]]

## Sections

- Overview
- Timeline
- How it is triggered (C++)
- Niagara system: NS_Claude_Disintegrate
- Materials
- Tuning knobs
- Testing
- Known issues and open items
- Reference

## Made by

- Thread: [[Redo3 Threads#Beam and gunshot Niagara effects|Beam and gunshot Niagara effects]]
- Explained on: [[Redo3 Mech Character]]
- Group: [[Redo3 Artifacts - Beam, Gun and Mech FX]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Mech FX Reels]]
- [[Redo3 Artifact - Mech Disintegration - Flow and Structure]]
