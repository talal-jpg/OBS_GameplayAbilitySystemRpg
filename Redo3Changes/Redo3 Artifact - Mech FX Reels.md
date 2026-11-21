↑ [[Redo3 Artifacts - Beam, Gun and Mech FX]] · [[Redo3 Artifacts Index]]

# Mech FX Reels

claude.ai Artifact · gallery · updated 2026-10-23 · [open the artifact](https://claude.ai/artifact/4ZyT9YGkn9bKYJ5cTs1nLe)

A video gallery of the mech's Niagara effects, recorded in PIE or rendered frame by frame in UE 5.8, newest first, each with a short spec grid. The reels are: NS_Claude_Disintegrate with a new sideways cube kick (23 Oct); NS_Claude_RespawnFX rebuilt to follow the Codi VFX cube respawn reference frame by frame (22 Oct); the GC_Beam cue's NS_ElectricBeamMy at BeamWidth 4, set against RIOT reference frames (22 Oct); mech disintegration v2 on the control rig ragdoll (22 Oct); and NS_Claude_Beam v8, where texture erosion dissolves the beam, with its charge and burst gameplay cues (21 Oct). The page was first published as 'NS_Claude_Beam v8 video'.

## What it shows

- Disintegration kick: cubes no longer take the body's velocity. They get a 180 cm/s kick along the mech's +Y when standing, turned toward the run and x1.6 at 6 m/s and above. It is tuned with the User.Kick* parameters, and the run velocity comes from User.DeathVelocity, set in C++.
- NS_Claude_RespawnFX v10: a 4.3 s one-shot with 13 emitters. A glowing cube flies in and bursts, a slab and wire box build up into a lattice cube, then a white flash with a chromatic hex rim leaves a glass volume that dissolves. The projectile start comes from User.StartOffset, and the colour is tuned for StartupMap at 2755 K.
- GC_Beam at BeamWidth 4 with BlueElectricBeam off: the beads, noise streaks, bolts and wisps scale with the beam instead of being squashed, and the flash and light grow by sqrt(4).
- Disintegration v2 starts the moment bIsDead is set. The mesh is eaten away over 1.6 s with a burnt edge and a cyan rim, about 2600 cubes are born with the mesh velocity and bounce and settle, and everything is gone after 5.6 s.
- NS_Claude_Beam v8: the solid rod breaks into beaded strands, then sparse beads, and is gone about 2.5 s after the blast. The video is 10.7 s at 1280x720 and 60 fps, with jump-to chips.
- In the gameplay cues, the charge plays in NS_beamChargeFx (GC_BeamChargingLooping) and holds at full charge until release. The burst plays in NS_ElectricBeamMy (GC_Beam) with Beam Start and Beam End. The cues were rendered with a 2 s charge and a target 20 m ahead.
- Related commits: 3f71d59 (NS_Claude_Beam and NS_ElectricBeamMy update, 2026-10-18), 450006c (Beam FX v8 with charge and release cues), 2c1d78b (mech death: disintegrate FX, ragdoll, DeathVelocity) and f84c282 (Respawn FX v10), all three on 2026-11-15.

## Sections

- Disintegration: cube kick
- Cube respawn, rebuilt
- GC_Beam at 4x width
- Mech disintegration v2
- NS_Claude_Beam v8
- In the gameplay cues

## Made by

- Thread: [[Redo3 Threads#Beam and gunshot Niagara effects|Beam and gunshot Niagara effects]]
- Explained on: [[Redo3 Beam and Gun FX]], [[Redo3 Mech Character]]
- Group: [[Redo3 Artifacts - Beam, Gun and Mech FX]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Mech Disintegration Effect]]
- [[Redo3 Artifact - Mech Disintegration - Flow and Structure]]
