↑ [[Redo3 Artifacts - Grapple Swing]] · [[Redo3 Artifacts Index]]

# GA_GrappleSwing2 Map

claude.ai Artifact · diagram/map · updated 2026-10-10 · [open the artifact](https://claude.ai/artifact/C8R7XHji58uQoc2CJsGfsj)

A second page on GA_GrappleSwing2, published at the same time as the 'GA_GrappleSwing2' page and with the same reference text. It is laid out as one long technical sheet with a sticky section bar, and its Mermaid diagrams (mind map, state machine, activation flowchart, network sequence and PrepareRootMotion flowchart) sit on paper cards that scroll sideways. It adds an intro: this is the second version, GA_GrappleSwing (v1) is left as it was, and the full source and Blueprint paths are listed. It also has a one-sentence description of the controls.

## What it shows

- It has the same nine sections and the same five Mermaid diagrams (mindmap, stateDiagram-v2, activation flowchart, sequenceDiagram, PrepareRootMotion flowchart) as the GA_GrappleSwing2 page.
- Full paths: Source/Aura/Public/AbilitySystem/Abilities/GA_GrappleSwing2.h, Source/Aura/Private/AbilitySystem/Abilities/GA_GrappleSwing2.cpp, and Content/Level/GAS/GameplayAbilities/GA_GrappleSwing/GA_BPGrappleSwing2.
- v1 GA_GrappleSwing (157aa06) is left untouched. Its native tag C++ symbols differ from v2's so both files can share a unity build, and the rope cue tag string is the same.
- Controls: press in the air to search for an anchor and fire a short hook flight, hold to swing on a pendulum that keeps momentum, and let go for a vault that depends on where in the arc you release. Keep holding to auto-release at the end of each arc and fire the next web at the apex.
- Shift is shared with GA_Boost: Boost refuses while Falling and the swing only accepts while Falling, so one press starts only one of them (see ab6e9c4, 'Only allow GA_Boost on the ground').
- Tuning defaults are the same as on the other page, for example MaxGrappleDistance 4000, HookSpeed 15000, MinRopeLength 400, IdealRopeLength 1300 and RootMotionPriority 500.
- Commits: 09decfd (added), 7d3c5b6 (documentation with mind map and flow diagrams), 6235450 and e038cc3 (doc corrections).

## Sections

- Mind map
- The pieces
- Ability state machine
- Main flow
- Networking summary
- Tuning reference
- Blueprint setup
- Debugging
- Known limits

## Made by

- Thread: [[Redo3 Threads#Spider-Man grapple swing ability|Spider-Man grapple swing ability]]
- Explained on: [[Redo3 Grapple Swing]]
- Group: [[Redo3 Artifacts - Grapple Swing]]

## Other artifacts from the same thread

- [[Redo3 Artifact - GA_GrappleSwing2]]
