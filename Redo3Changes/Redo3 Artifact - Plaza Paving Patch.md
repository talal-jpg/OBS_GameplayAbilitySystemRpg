↑ [[Redo3 Artifacts - Hex Path, Bridges and Paving]] · [[Redo3 Artifacts Index]]

# Plaza Paving Patch

claude.ai Artifact · patch notes · updated 2026-10-29 · [open the artifact](https://claude.ai/artifact/7obwugM4KjrfSr529ThMii)

An incremental patch for the AuraRedo3 working copy (branch redo3 at HEAD 8d98fd7 plus the uncommitted paving work): four files, 1177 lines added and 207 removed, with sha256 hashes before and after, applied with patch -p1. It adds the paved centre plaza where the bridge paths meet: a regular polygon around the centre tile (FPlaza), with each path turning square onto its own side over a straight lead. The plaza is paved in rings with a plinth in the middle for a lamp, which a new Lamp pin on the HexPlatformTiles PCG node exposes. Bridge exits now follow the real bridge side, through a replicated PathStartSides array. Lane ends are clipped where they meet an earlier route's strip or the plaza. The code is in 5ccdfef.

## What it shows

- New UPROPERTYs: bPavePlaza (true), PavingPlazaSideCells 6, PavingPlazaMinSides 4 (4 to 8), PavingPlazaLead 1.5 cells (straight lead before the side), PavingPlazaTurn 3 cells (room to turn onto the lead).
- AssignPlazaSides tries every order-keeping assignment of routes to sides and picks the polygon turn (circular mean) that bends them least. The plaza shrinks, but not below the path width plus a cell, so no bridge tile ends up under it.
- Plaza stones: one-cell-deep courses in rings from the outside in (a pinwheel on a square, mitred courses with diamond keystones at the corners otherwise), then a 2x2 plinth for the lamp.
- FHexPathPlaza (bValid, Centre, Yaw, NumSides, SideLength, Tile, TileIndex) and GetPathPlaza(). The PCG Lamp pin outputs one point at the plaza centre with attributes TileIndex, PlazaSides and PlazaSideLength, plus LampScale, LampZOffset and LampExtent settings.
- PathStartSides (TArray<uint8>, replicated with OnRep_PathStartTiles) and GetPathStartSide(): each start tile records the hex side its bridge leaves through; 255 = unknown.
- FRail gains ChunkBounds (16-segment chunks), At(S, Offset), TightestRadius and InCorridor (flat end caps). FCellRect is replaced by FStone (S0/S1 in cm along the rail, lanes V..V+H-1).
- Lanes end where their edges enter the plaza or an earlier route's strip (searched in CellSize/8 steps, then bisected); pieces shorter than 0.35 cells are dropped. Stones on a bend span their own lane's stretch, split into several where a turn is sharp.
- TileAt looks a tile up 0.01 cm along X so points on hex edges resolve the same on every machine. The debug view draws the plaza outline in yellow.

## Sections

- Header and file hash table
- Patch

## Made by

- Thread: [[Redo3 Threads#Pave bridge paths with stone patterns|Pave bridge paths with stone patterns]]
- Explained on: [[Redo3 Path Paving]]
- Group: [[Redo3 Artifacts - Hex Path, Bridges and Paving]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Paving Round 4 Patch]]
- [[Redo3 Artifact - Paving Round 3 Patch]]
- [[Redo3 Artifact - Path Paving Patch]]
- [[Redo3 Artifact - Bridge Path Paving in-game shots]]
