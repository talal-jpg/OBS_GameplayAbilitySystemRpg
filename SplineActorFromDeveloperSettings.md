 `URogueDeveloperSettings::TrackSplineActor` is a `UPROPERTY(EditDefaultsOnly, Config)` on a `UDeveloperSettingsBackedByCVars` class (`Config=Game, DefaultConfig`). That means it’s meant to be edited in the editor UI and serialized into a `.ini`.

In this project it is currently persisted here:

- `Config/DefaultGame.ini`
- Section: `[/Script/RogueMassExample.RogueDeveloperSettings]`
- Key: `TrackSplineActor`

Example from your repo:

```ini
[/Script/RogueMassExample.RogueDeveloperSettings]
TrackSplineActor=/Game/Maps/L_Example1.L_Example1:PersistentLevel.B_TrainTrack_C_1
```

That value is a *soft reference path* to a **placed actor instance in the map** (`L_Example1` → `PersistentLevel` → `B_TrainTrack_C_1`).

In the editor, this property shows up under:

- **Project Settings** → **Rogue Mass Example** → **Simulation Settings** → `TrackSplineActor`

When you change it there and save, Unreal writes the config entry above.

---

### Where `TrackSplineActor` is **gotten** (read/used in code)

All reads go through the class default object (CDO) loaded from config:

```cpp
const URogueDeveloperSettings* Settings = GetDefault<URogueDeveloperSettings>();
```

Then the code checks and resolves the soft pointer:

- `Settings->TrackSplineActor.IsValid()`
- `AActor* TrackActor = Settings->TrackSplineActor.Get();`

#### Primary runtime consumer: `URogueTrainWorldSubsystem`

File: `Source/RogueMassExample/Private/Subsystems/RogueTrainWorldSubsystem.cpp`

1) Called at world begin play:

- `URogueTrainWorldSubsystem::OnWorldBeginPlay(UWorld& InWorld)` calls `DiscoverSplineFromSettings()`.

2) Resolves actor → spline:

- `URogueTrainWorldSubsystem::DiscoverSplineFromSettings()`
  - `Settings = GetDefault<URogueDeveloperSettings>()`
  - `TrackActor = Settings->TrackSplineActor.Get()`
  - `TrackActor->FindComponentByClass<USplineComponent>()`
  - stores it in `TrackSpline`
  - calls `ResampleSplineUniform(*TrackSpline.Get(), Settings->TrackSplineResampleStep)`

So the “get” flow here is:

- settings soft ptr → `AActor` → `USplineComponent` → cached as `TrackSpline` in the subsystem.

#### Editor-time consumer: `ARogueTrainStation`

File: `Source/RogueMassExample/Private/Actors/RogueTrainStation.cpp`

- In editor builds, `ARogueTrainStation::OnConstruction()` calls `ComputeStationAlpha()`.
- `ComputeStationAlpha()`:
  - `Settings = GetDefault<URogueDeveloperSettings>()`
  - resolves `TrackSplineActor` to `TrackActor`
  - finds `USplineComponent`
  - computes distance along spline at the station’s world location
  - normalizes by spline length → `StationAlpha`

---

### ASCII diagram: end-to-end set/get flow

```text
              (SET)                                             (GET / USE)

   Designer in Editor
   ------------------
   Project Settings
   "Rogue Mass Example"
   Simulation Settings
   TrackSplineActor
          |
          |  Save config
          v
   Config/DefaultGame.ini
   [/Script/RogueMassExample.RogueDeveloperSettings]
   TrackSplineActor=/Game/Maps/L_Example1...:PersistentLevel.B_TrainTrack_C_1
          |
          |  Engine loads config into CDO at startup
          v
   URogueDeveloperSettings (Class Default Object)
   GetDefault<URogueDeveloperSettings>()
          |
          +-------------------------------+
          |                               |
          |                               |
          v                               v 
  URogueTrainWorldSubsystem          ARogueTrainStation (editor)
  OnWorldBeginPlay()                 OnConstruction()
        |                                  |
        v                                  v
  DiscoverSplineFromSettings()        ComputeStationAlpha()
        |                                  |
        |  TrackSplineActor.IsValid()      |  TrackSplineActor.IsValid()
        |  TrackSplineActor.Get()          |  TrackSplineActor.Get()
        v                                  v
  TrackActor (AActor*)                TrackActor (AActor*)
        |                                  |
        |  FindComponentByClass<USplineComponent>()
        v                                  v
  TrackSpline (USplineComponent*)     Spline distance -> normalize -> StationAlpha
        |
        |  ResampleSplineUniform(..., TrackSplineResampleStep)
        v
  Cached spline used by simulation systems
```

---

### Practical notes / common pitfalls with `TSoftObjectPtr<AActor>` here

- `TSoftObjectPtr` can be **non-null** but still not **resolved** (`IsValid()` false) until the referenced object exists in memory.
- In this project, `URogueTrainWorldSubsystem` resolves it in `OnWorldBeginPlay()`, which is late enough that the persistent level actors should exist, so `IsValid()` usually works.
- If you ever need it earlier (or across streamed levels), you’d typically switch to checking `!TrackSplineActor.IsNull()` and/or call `TrackSplineActor.LoadSynchronous()` (but note: loading a *placed actor in a map* via soft path is not the same as loading a standalone asset; you generally want to rely on the level being loaded).

If you tell me whether you want the diagram to include *all downstream users of `TrackSpline`* (train movement, station detection, etc.), I can extend the right-hand side to show how the cached spline propagates further.