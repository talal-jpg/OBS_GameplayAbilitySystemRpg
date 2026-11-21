↑ [[Redo3 Artifacts - Hex Look, Fog and Ice]] · [[Redo3 Artifacts Index]]

# Ice look refs

claude.ai Artifact · comparison · [open the artifact](https://claude.ai/artifact/JxABNpmMXfxQFEEm2ry6rE)

Reference set and progress log for the hex-platform ice restyle in UE 5.8, dated 2026-10-25 (prep) and 2026-10-26 (iterations). It measures the YouTube style target (Borderlands 4 Ellie DLC clip, video IHTCnpCLCPs) with the character, HUD and floor masked, and Talal's HexPillar ice image with a 9-stop value ramp. It shows baseline captures A-D from StartupMap, the course change to outlines inside the existing materials, and a five-view grid comparing Baseline, V1 Blue hour, V2 pre-torch, V2 with V3 ink (a slot-swap error), V3 Bold ink night, V2.1 and StartupMap after the apply. The verdict picked V2 (four independent reviews), and V2.1, a 7-pass polish of V2 with V3-weight hand-drawn ink, was applied to StartupMap on 2026-10-26 at 06:29 UTC. The page lists env_match scores, mean luma per view, an ink acceptance table against V3 and the cost of the outline (about 1.5-2 fps, 3-4 %).

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Style target: 97% of environment pixels are blue (hue 180-240), flat sky #026ab5, fog about #0359a5, navy shadows #00255e, lines about 2 px at 720p.
- Baseline env_match was 8.8 / 13.4 / 8.9 / 25.9 for views A-D; reference frames score a median of 87 against each other.
- Course change: no separate ice material; 18 slots reverted to M_Pillar2 / M_Pillar2_GC, prop copies MM_Ice_Hex, MI_Ice_GlacialsKit_Hex, MM_SnowAssets_Hex, MI_SnowPiles_Hex made in HexPlatforms/Props.
- Outline comment box in M_Pillar2, M_Pillar2_GC, MM_Ice_Hex and MM_SnowAssets_Hex behind static switch UseOutline (off by default); on costs +140, +153, +67 and +67 PS instructions.
- Env mean scores: Baseline 14.1, V1 29.7, V2 pre-torch 49.1, V2 (slot-swap) 57.9, V3 58.3, V2.1 52.3, StartupMap after apply 54.2.
- Slot-swap error: V2's 02:54 UTC re-shoot ran with V3's slots; every shot run now reapplies its own slots and logs them to slots_log.json.
- Torch override: 37 bridge torch lights from B_BridgeTorchFire changed from yellow 25.7 m / 100000 to peach (255,185,135) 4.5 m / 30000, specular 0.2.
- V2.1 light: moon 2.24 lux, skylight 0.70; ink 5 px with navy #003E77 mix, creases 3.0; blue mist copies NS_*_Cerulean.
- Apply backup Saved/ClaudeBackups/IceLook_2026-10-25/Apply_20261026T062849Z (30 files); outline costs 49.1-50.5 fps vs 51.4-51.9 fps at view C.
- Far platform rims (200-280 m) are not fixed; a fix would need a screen-space outline pass.

## Commits it documents

- `bf8dbba` Add M_HexIce pillar materials, built but kept unassigned · [[Redo3 Hex Look and Atmosphere]]
- `cf4ec00` Ice look V2.1: in-material ink outlines, iteration MIs, blue mist · [[Redo3 Hex Look and Atmosphere]]
- `32a308e` Update StartupMap: ice look lighting, fog floor, fog fade sync · [[Redo3 Hex Look and Atmosphere]]

## Sections

- Video style target
- Environment-only style
- HexPillar ice reference
- Baseline A-D
- Verdict
- Course change · materials
- Iterations
- V2.1 · how it got here
- Lines up close
- Published files

## Made by

- Thread: [[Redo3 Threads#Fix and extend the hex ink lines|Fix and extend the hex ink lines]]
- Explained on: [[Redo3 Hex Look and Atmosphere]]
- Group: [[Redo3 Artifacts - Hex Look, Fog and Ice]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Ink lines in the hex pillar materials]]
- [[Redo3 Artifact - Ink lines under Nanite]]
- [[Redo3 Artifact - Props outline section layout shots]]
- [[Redo3 Artifact - Bottom pillar ink first pass shots]]
- [[Redo3 Artifact - Bottom pillar rim line shots]]
- [[Redo3 Artifact - Bottom bridge ink shots]]
- [[Redo3 Artifact - Bottom pillar snow-edge line shots]]
