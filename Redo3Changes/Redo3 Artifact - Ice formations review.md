↑ [[Redo3 Artifacts - Hex PCG and Tile Props]] · [[Redo3 Artifacts Index]]

# Hex Ice Cliffs Review

claude.ai Artifact · report · [open the artifact](https://claude.ai/artifact/7bUGvhGZWKZZ6wjDRiUFdw)

A review page (eyebrow 'MechMania · hex platform props, layer 2') showing ice cliff and frozen rock formations rebuilt from pieces of the FrozenNorse L_Showcase level that Talal circled, each merged into one mesh with a matching Geometry Collection. It reports the layer is running in game: HexTileProps2 replaces the old rock layer with ice cliffs on the rim facing out, big and medium formations inside and small ice debris on the fringes. It also documents three gate-cliff replicas (SM_IceMountain_01..03) and SM_Tilekit walkways from each bridge to the centre tile, all of which break into Geometry Collections with their tiles in PIE. Each formation is shown with its size and GC piece count, next to Talal's reference shots. It ends with how PCG_HexTileProps2 places the pieces and two open questions about snow shading and pushing 83 FrozenNorse files (about 556 MB) to LFS.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Formations: SM_IceCliff_01..04 (3.6-4.1 m wide, 4.6-6.5 m tall at scale 1, GC 12-20 pieces) and SM_IceRocks_01..04 (about 2 m, GC 6-11 pieces).
- Gate replicas: SM_IceMountain_01 gate massif 8.9 x 8.7 x 9.6 m from about 1160 level pieces (GC about 20 chunks), SM_IceMountain_02 icefall spire 6.4 x 6.4 x 7.4 m, SM_IceMountain_03 both together 8 x 12 x 9.8 m.
- Walkways SM_PathFloor_01..03 use SM_Tilekit_02/03/01 slabs from each bridge to the centre tile; nothing spawns on them.
- In PIE collapsing tiles removed ice instances and spawned their GCs, with 25 to 60 debris actors alive at a time.
- PCG_HexTileProps2 only uses spots the first graph left empty; ice cliffs go on about 1 in 8 free rim tiles facing outward, 6-10 m tall in game (HexRadius/100 = 2, scale 0.55-0.75).
- Small formations cluster where a second noise is high, at 1.6-3 m; the two prop components keep separate entries in the shared tile map.
- Front (icicle side) of each formation is +X, which the scatter node turns outward on rim tiles.
- Open questions: snow shading depends on world height and the level's virtual texture; whether to push 83 FrozenNorseEnvironment files (about 556 MB) to LFS or copy only the needed materials into Props.

## Commits it documents

- `6e7c02a` Hex tile props 2: ice cliffs and frozen rock formations replace the rock layer · [[Redo3 Hex Tile Props]]
- `6444429` Hex tile props 2: L_Showcase gate cliff replicas and bridge-to-centre walkways · [[Redo3 Hex Tile Props]]

## Sections

- Ice cliffs for the hex platforms
- Gate cliff replicas and walkways
- On the platforms (Simulate)
- What you circled
- Big formations
- Small formations
- How they get on the platforms
- Open questions

## Made by

- Thread: [[Redo3 Threads#Overlapping Geometry Collections|Overlapping Geometry Collections]]
- Explained on: [[Redo3 Hex Tile Props]]
- Group: [[Redo3 Artifacts - Hex PCG and Tile Props]]
