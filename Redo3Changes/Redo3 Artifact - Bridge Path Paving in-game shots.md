↑ [[Redo3 Artifacts - Hex Path, Bridges and Paving]] · [[Redo3 Artifacts Index]]

# Bridge Path Paving

claude.ai Artifact · gallery · [open the artifact](https://claude.ai/artifact/43thHZGmYWHZmcFtJfSyY9)

An in-game shot page for the stone paving on the hex bridge paths, taken in PIE on StartupMap and organised newest round first. Round 4 (2026-10-30) gives plazas with five or more sides one spare side so paths turn in less, and reports 4 plazas with a spare side, 3 hexagons plus 1 pentagon, 21 square plazas and 47 paved platforms. Round 3 bends the stones with the path through curvature in custom data, sizes the plaza to the platform and makes pentagons for five paths. Round 2 (2026-10-29) shows the first fixed 4.8 m square plazas, and an earlier section shows paths just crossing before any plaza. Each round lists the remaining flaws, such as S-bends, paths arcing round the plaza and glossy stones, alongside debug-draw and tile-collapse shots.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Round 4 numbers: 4 plazas with a spare side, 3 hexagons + 1 pentagon, 21 square plazas, 47 platforms paved.
- Spare-side plazas: BP_MyHexPlatform_C_2 (287 tiles) and C_7 (296 tiles), BP_MyHexPlatformBottom_C_3 (149 tiles) with 5 paths into hexagons, and BP_MyHexPlatformBottom_C_24 (66 tiles) with 4 paths into a pentagon.
- No two paths share a side, no path runs into a plaza corner and no stones overlap in the round 4 shots.
- Round 3: stones bend in the vertex shader from curvature custom data; tightest bend radius about 50 cm gives keystones, but some big stones come out as parallelograms.
- M_PathSlab Roughness is 0.1 (set by hand on 10-25), so the stones still look glossy.
- Stones are 1x1, 2x1 and 2x2 on a 40 cm grid with 3 cm joints; bent stones break into ordinary block-shaped debris when their tile collapses.
- Debug draw colours: red rails, cyan cuts, orange stone boxes, yellow plaza outline.

## Commits it documents

- `5ccdfef` Pave hex bridge paths with stone slabs and a centre plaza · [[Redo3 Path Paving]]

## Sections

- Round 4: spare plaza sides
- Round 3: bending stones and plazas sized to the platform
- Plazas
- Bends
- Debug view
- Collapse
- Round 2 (2026-10-29): first plaza
- Before the plaza (2026-10-29 morning)

## Made by

- Thread: [[Redo3 Threads#Pave bridge paths with stone patterns|Pave bridge paths with stone patterns]]
- Explained on: [[Redo3 Path Paving]]
- Group: [[Redo3 Artifacts - Hex Path, Bridges and Paving]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Paving Round 4 Patch]]
- [[Redo3 Artifact - Paving Round 3 Patch]]
- [[Redo3 Artifact - Plaza Paving Patch]]
- [[Redo3 Artifact - Path Paving Patch]]
