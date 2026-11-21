↑ [[Redo3 Artifacts - Hex Performance and Breaking]] · [[Redo3 Artifacts Index]]

# Files: Profiling Reports

Shared project folder · 3 files · 17.2 KB · 2026-10-19 to 2026-10-23

Files saved to the project's shared folder by [[Redo3 Threads#CPU profile during play|CPU profile during play]]. They are working files: renders, comparison sheets, reports and source files that the threads made while building the feature, not files in the game repo.

Explained on: [[Redo3 Engine Tools and Assets]], [[Redo3 Hex Pillars and Breaking]].

## Files

| File | Size | Date | Thread | What it is |
|---|---|---|---|---|
| `PIE_CPU_Profile_2026-10-20.md` | 6.3 KB | 2026-10-19 | [[Redo3 Threads#CPU profile during play\|CPU profile during play]] | First PIE CPU profile: game, render, RHI and worker thread rankings, hitch list and suggested fixes with file:line references. |
| `PIE_CPU_Profile_2026-10-23.md` | 5.1 KB | 2026-10-22 | [[Redo3 Threads#CPU profile during play\|CPU profile during play]] | Second PIE profile (184 ms per frame), per-10 s breakdown, the Vulkan allocation stalls and a comparison with 10-20. |
| `PIE_Second_Run_Lag_2026-10-23.md` | 5.8 KB | 2026-10-23 | [[Redo3 Threads#CPU profile during play\|CPU profile during play]] | Why the second PIE run lags: VRAM measurements, the Vulkan eviction mechanism with engine source lines, evidence table and bridge GC cost. |

## Related

- [[Redo3 Artifacts - Hex Performance and Breaking]]
- [[Redo3 Engine Tools and Assets]]
- [[Redo3 Hex Pillars and Breaking]]
