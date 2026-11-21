↑ [[Redo3 Artifacts - Hex Path, Bridges and Paving]] · [[Redo3 Artifacts Index]]

# Paving Round 3 Patch

claude.ai Artifact · patch notes · updated 2026-10-30 · [open the artifact](https://claude.ai/artifact/SQJaLaijAmnRADk22xdUXa)

A four-file C++ patch (MyHexPlatform.h/.cpp and PCGHexPlatformTiles.h/.cpp; 425 lines added, 42 removed) with sha256 hashes before and after, applied with patch -p1. The page also gives the HLSL for a Custom node in the paving material's World Position Offset. The patch bends paving stones on curves: FHexPavingSlab gains a Bend value, which is passed to the material as per-instance custom data 1 and 2 through new SlabBendX/SlabBendY PCG attributes. It also makes the plaza grow with the platform, gives a path that joined another near the plaza a side of its own, allows pentagons for 5 paths, and adds the Aura.PathPavingDebug console variable. All of this code is in 5ccdfef.

## What it shows

- New UPROPERTY PavingPlazaSizeRatio = 0.15 (0 to 0.6): plaza sides stand that fraction of the way to the nearest bridge tile when larger than PavingPlazaSideCells. An OverTiles check samples 24 points so the plaza stays over tiles.
- FPlazaPlan (NumSides, SideLength, Alpha, Sides, Points, WorstAngle, bValid, IsBetterThan) and a PlanPlaza lambda. Routes that ran into a hub route within JoinReach get their own side (JoinedRoutes). Joiners are merged back while the plan is invalid or WorstAngle > MaxSideAngle (75 degrees); routes cut back to their bridge count 5x (ShortRouteAngleScale).
- Stone bending: curvature = 2 sin(dHeading/2) / chord, clamped to 1.6 / (stone height). The pivot is offset by the sagitta (Curvature * Chord^2 / 8) so the bent stone's ends land on the lane. Plaza stones pass curvature 0.
- WPO Custom node: Float 3 output, inputs LP, AX, AY, BendX, BendY (PerInstanceCustomData 1 and 2). Set the material's Max World Position Offset Displacement to 40.
- PCGHexPlatformTiles Paving pin: new float attributes SlabBendX / SlabBendY (1/cm in world space, divided by the actor's X scale), packed after TileIndex as custom data 1 and 2.
- Aura.PathPavingDebug (ECVF_Cheat): 1 draws the paving debug for every platform (as bDrawPavingDebug); 2 also writes each platform's paving input (TILES, CENTRE, STARTS, ROUTES, PLAZA, PARAM, PATTERN) to Saved/PathPaving/<map>_<Client|Server>_<actor>.txt, outside shipping builds.
- Lead turn: the route start is moved back until the tail's tightest radius reaches 1.5x the half width, without causing a fold further back.

## Sections

- Header and file hash table
- Paving material: Custom node for World Position Offset
- Patch

## Made by

- Thread: [[Redo3 Threads#Pave bridge paths with stone patterns|Pave bridge paths with stone patterns]]
- Explained on: [[Redo3 Path Paving]]
- Group: [[Redo3 Artifacts - Hex Path, Bridges and Paving]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Paving Round 4 Patch]]
- [[Redo3 Artifact - Plaza Paving Patch]]
- [[Redo3 Artifact - Path Paving Patch]]
- [[Redo3 Artifact - Bridge Path Paving in-game shots]]
