↑ [[Redo3 Artifacts - Hex Path, Bridges and Paving]] · [[Redo3 Artifacts Index]]

# Paving Round 4 Patch

claude.ai Artifact · patch notes · updated 2026-10-30 · [open the artifact](https://claude.ai/artifact/KwJkivcjjyyoCRvGBW7gGP)

A one-file C++ patch to Source/Aura/Private/Actors/MyHexPlatform.cpp (21 lines added, 3 removed), applied with patch -p1 on top of round 3. It gives the bridge-path plaza a spare side when paths reach it bunched up on one side, so the outer path no longer swings round onto its side. PlanPlaza gains a SpareSides argument. When every side is taken, there are fewer than 8 sides, and the worst route meets its side more than 40 degrees off square (but within MaxSideAngle), a plan with one extra side is tried. It is kept if it turns the worst route in by at least 10 degrees less. The page gives sha256 hashes for the file before and after and for the diff; the code is in 5ccdfef ('Pave hex bridge paths with stone slabs and a centre plaza').

## What it shows

- File: Source/Aura/Private/Actors/MyHexPlatform.cpp. sha256 before a37cee28... (the round 3 result), after 7133a9af...; diff sha256 70499e69....
- New constants: SpareSideAngle = 40 degrees and SpareSideGain = 10 degrees.
- PlanPlaza(Hubs, Plan, SpareSides = 0): NumSides = max(clamp(PavingPlazaMinSides, 4, 8), Hubs.Num()) + SpareSides.
- Spare side is tried when Plan.bValid, Plan.NumSides == HubRoutes.Num(), NumSides < 8, and SpareSideAngle < WorstAngle <= MaxSideAngle. It is taken if Spare.WorstAngle < Plan.WorstAngle - SpareSideGain.
- The patch was applied to the uncommitted working copy on the PC; the result is in 5ccdfef (2026-11-15).

## Sections

- Header and file hash table
- Patch

## Made by

- Thread: [[Redo3 Threads#Pave bridge paths with stone patterns|Pave bridge paths with stone patterns]]
- Explained on: [[Redo3 Path Paving]]
- Group: [[Redo3 Artifacts - Hex Path, Bridges and Paving]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Paving Round 3 Patch]]
- [[Redo3 Artifact - Plaza Paving Patch]]
- [[Redo3 Artifact - Path Paving Patch]]
- [[Redo3 Artifact - Bridge Path Paving in-game shots]]
