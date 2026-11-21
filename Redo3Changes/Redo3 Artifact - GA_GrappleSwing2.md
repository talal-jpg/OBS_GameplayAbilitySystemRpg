↑ [[Redo3 Artifacts - Grapple Swing]] · [[Redo3 Artifacts Index]]

# GA_GrappleSwing2

claude.ai Artifact · guide · updated 2026-10-10 · [open the artifact](https://claude.ai/artifact/Lduv4qKY5tt1DkeBDgPTo2)

A reference page, with a sticky contents rail, for GA_GrappleSwing2: a Local Predicted Spider-Man style grapple swing. UGA_GrappleSwing2 handles input, the anchor search, network messages and the rope cue. FRootMotionSource_GrappleSwing2 simulates the pendulum inside the CharacterMovementComponent on every move, so the swing is predicted, replayed and server-checked like normal movement. The page covers a Mermaid mind map, the pieces, the ability state machine, the activation and search flow, anchor scoring, the client-server sequence, one move of the root motion source, how a swing ends, networking, the full tuning reference, Blueprint setup, debugging and known limits. It matches Docs/GA_GrappleSwing2.md, added in 7d3c5b6 and corrected in 6235450 and e038cc3.

## What it shows

- Files: Public/AbilitySystem/Abilities/GA_GrappleSwing2.h and Private/AbilitySystem/Abilities/GA_GrappleSwing2.cpp. The Blueprint child GA_BPGrappleSwing2 uses InputTag Input.Shift and is granted through BP_MyCharMech StartupAbilities. Native tags: Ability.GrappleSwing2 and GameplayCue.GrappleRope.
- EGS2AbilityState values: Idle, Searching, Hooked, Releasing, Spent, Rejected; the source's phases are Travel, Swing and Done. CanActivateAbility accepts only while Falling on the owning client and is blocked by bBlockUntilRelease.
- FindAnchor casts crosshair, aim-assist cone (2 rings, AimAssistConeDeg 14) and fan rays (SwingFanYawDeg 25, elevations 35/50/65) on Visibility, up to MaxGrappleDistance 4000. EvaluateHit rejects hits by RopeObjectTypes, distance, height, rope line of sight, missing surface or floor clearance (608 cm with an 88 cm half height). Survivors are scored, and the crosshair hit wins within CrosshairStickiness 0.15.
- The client sends FGameplayAbilityTargetData_GrappleSwing2 once (SwingId, Anchor, StartLocation, ClientStartTime, TravelTime, Floor, ChainCount, AgainstMotionDir), and ServerValidate checks it. The release is sent as a source time in a GameCustom1 payload, so client and server end on the same move.
- Physics per substep: reel in and pay out, gravity scaled 2 descending and 1.3 ascending, drag with SoftMaxSpeed 3000 and HardMaxSpeed 5000. The rope only pulls, and a slack rope that catches keeps CatchEnergyRetention 0.55 of the impact. Chained swings get +5% per stack, up to 3 stacks.
- End reasons, in order: Released/Forced, JumpReleased, Timeout (MaxSwingDuration 20), TooClose, Snapped (LOSBreakTime 0.12), Landed, AutoReleased (AutoReleaseAngleDeg 50). Finish outputs the end velocity, and the CMC removes the source with MaintainLastRootMotionVelocity.
- Debugging: bDrawDebug shows search rays (green valid, red rejected, grey no hit) and the rope. Reject reasons go to the screen and the LogGrappleSwing2 log category. GetGrappleSwing2State is BlueprintPure for animation or UI, and p.RootMotion.Debug 1 shows the source.
- Known limits: moving geometry in RopeObjectTypes (the hex platforms are WorldDynamic) can cause corrections, the rope stays attached if the anchor tile collapses, AI never finds an anchor, and the swing was not yet play-tested in a real client/server session.
- Commits: 09decfd (GA_GrappleSwing2 added), 7d3c5b6 (Docs/GA_GrappleSwing2.md), 079e1cc and dfeab8d (look-against-the-motion attach), 6235450 and e038cc3 (doc corrections), 73aa727 (GA_BPGrappleSwing2 and blueprint updates).

## Sections

- 1. Mind map
- 2. The pieces
- 3. Ability state machine
- 4. Main flow
- 4.1 Activation and search (owning client)
- 4.2 Anchor search (FindAnchor + EvaluateHit)
- 4.3 Network sequence (remote client)
- 4.4 One move of the root motion source (PrepareRootMotion)
- 4.5 How a swing ends
- 5. Networking summary
- 6. Tuning reference
- 7. Blueprint setup
- 8. Debugging
- 9. Known limits

## Made by

- Thread: [[Redo3 Threads#Spider-Man grapple swing ability|Spider-Man grapple swing ability]]
- Explained on: [[Redo3 Grapple Swing]]
- Group: [[Redo3 Artifacts - Grapple Swing]]

## Other artifacts from the same thread

- [[Redo3 Artifact - GA_GrappleSwing2 Map]]
