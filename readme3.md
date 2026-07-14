# RogueMassExample - MassEntity Architectural Dependency Map (readme3)

This document is generated from the **C++ source** in `Source/RogueMassExample` and focuses on Unreal Engine’s **MassEntity (ECS)** usage:

* **Who spawns which entities** (and where the spawn/configure code lives)
* **Which processors run, in what order** (based on explicit `ProcessingPhase` / `ExecutionOrder` constraints)
* **Which fragments each processor reads/writes** (including key engine-provided fragments)
* **Function-by-function ASCII call-flow** showing the class that calls each step

> Note: the `Content/Mass/Configurations/DA_EC_*.uasset` configs can add additional fragments (representation, movement, navigation, etc.). This map lists what is **required/used by code** and what is **explicitly added/initialized** by traits and subsystem code.

---

## 1) Key source files (navigation index)

### Core “hub”
* `Source/RogueMassExample/Public/Subsystems/RogueTrainWorldSubsystem.h`
* `Source/RogueMassExample/Private/Subsystems/RogueTrainWorldSubsystem.cpp`

### Fragments/Tags/Shared
* `Source/RogueMassExample/Public/Mass/Fragments/RogueFragments.h`
* `Source/RogueMassExample/Private/Mass/Fragments/RogueFragments.cpp`

### Entity composition (traits)
* `Source/RogueMassExample/Public/Mass/Traits/RogueEntityTrait*.h`
* `Source/RogueMassExample/Private/Mass/Traits/RogueEntityTrait*.cpp`

### Processors
* Passengers
  * `.../Processors/Passengers/RoguePassengerSpawnProcessor.*`
  * `.../Processors/Passengers/RoguePassengerMovementProcessor.*`
  * `.../Processors/Passengers/RoguePassengerHeightProcessor.*`
* Stations
  * `.../Processors/Stations/RogueTrainStationDetectProcessor.*`
  * `.../Processors/Stations/RogueTrainStationOpsProcessor.*`
* Trains
  * `.../Processors/Trains/RogueTrainHeadwayProcessor.*`
  * `.../Processors/Trains/RogueTrainEngineMovementProcessor.*`
  * `.../Processors/Trains/RogueTrainCarriageFollowProcessor.*`
* Debug
  * `.../Processors/Debug/RogueDebugDataProcessor.*`

### Utilities (called by processors / subsystem)
* `Source/RogueMassExample/Private/Utilities/RogueStationQueueUtility.cpp`
* `Source/RogueMassExample/Private/Utilities/RoguePassengerUtility.cpp`
* `Source/RogueMassExample/Private/Utilities/RogueTrainUtility.cpp`

---

## 2) MassEntity building blocks used by this project

### Tags (custom)
Defined in `RogueFragments.h`:

* `FRogueTrainEngineTag`
* `FRogueTrainCarriageTag`
* `FRogueTrainStationTag`
* `FRogueTrainPassengerTag`
* `FRoguePooledEntityTag` (used by pooling / reuse)

### Fragments (custom)
Defined in `RogueFragments.h`:

* `FRogueStationQueueFragment`
  * Waiting grids (`Grids`) + occupancy (`OccupiedBy`)
  * `WaitingPoints`, `SpawnPoints`
  * `QueuesByWaitingPoint` (logical queue entries)
* `FRogueStationFragment` (station index, docked train)
* `FRogueTrainTrackFollowFragment` (`Alpha`, `Speed`, cached world pos/fwd)
* `FRogueTrainStateFragment` (stop/dwell state, target station, `Carriages[]`, headway)
* `FRogueTrainLinkFragment` (lead handle, carriage index, spacing)
* `FRogueCarriageFragment` (capacity, occupants, unload timers/cursor)
* `FRoguePassengerFragment` (origin/destination stations, phase, waiting slot, target, speed)
* `FRogueDebugSlotFragment` (debug slot index)

### Shared fragments (custom)
* `FRogueTrackSharedFragment` (`Spline`, `StationEntities`, `Platforms`, `TrackLength`)
  * Owned/cached by `URogueTrainWorldSubsystem` and read by multiple processors.

### Engine-provided fragments referenced by code
These are not defined here, but are **required by queries** or accessed by utilities:

* `FTransformFragment` (position/orientation)
* `FMassMoveTargetFragment` (movement target + desired speed)
* `FMassMovementParameters` (shared movement settings)
* `FAgentRadiusFragment` (passenger collision radius)
* `FMassRepresentationLODFragment` (used by `HidePassenger` / `ShowPassenger`)
* `FMassVelocityFragment` (zeroed when hiding passenger)

---

## 3) Entity types → composition (traits + important runtime init)

> “Composition” below is the **minimum** needed by code. The template assets (`DA_EC_*`) may add more.

### Station entity
* **Trait:** `URogueEntityTraitStation` (`.../Traits/RogueEntityTraitStation.*`)
* **Tag:** `FRogueTrainStationTag`
* **Custom fragments:** `FRogueStationFragment`, `FRogueStationQueueFragment`, `FRogueDebugSlotFragment`
* **Runtime initialization:** `URogueTrainWorldSubsystem::ConfigureStation(...)`
  * Fills `FRogueStationFragment::StationIndex`
  * Fills `FRogueStationQueueFragment::{SpawnPoints, WaitingPoints, WaitingGridConfig}`
  * Builds waiting grids via `RogueStationQueueUtility::BuildGridForWaitingPoint(...)`

### Train engine entity
* **Trait:** `URogueEntityTraitTrainEngine`
* **Tag:** `FRogueTrainEngineTag`
* **Custom fragments:** `FRogueTrainStateFragment`, `FRogueTrainTrackFollowFragment`, `FRogueDebugSlotFragment`
* **Engine fragments used:** `FTransformFragment`
* **Runtime initialization:** `URogueTrainWorldSubsystem::ConfigureTrain(...)`
  * Initializes state (starts “at station”, sets `TargetStationIdx`, clears carriage list)
  * Initializes follow alpha/speed

### Train carriage entity
* **Trait:** `URogueEntityTraitTrainCarriage`
* **Tag:** `FRogueTrainCarriageTag`
* **Custom fragments:** `FRogueTrainTrackFollowFragment`, `FRogueTrainLinkFragment`, `FRogueCarriageFragment`, `FRogueDebugSlotFragment`
* **Engine fragments used:** `FTransformFragment`
* **Runtime initialization:** `URogueTrainWorldSubsystem::ConfigureCarriage(...)`
  * Sets `FRogueTrainLinkFragment::{LeadHandle, CarriageIndex, Spacing}`
  * Sets `FRogueCarriageFragment::{Capacity, NextAllowedUnloadTime, UnloadCursor}`
  * Registers carriage under engine’s `FRogueTrainStateFragment::Carriages`

### Passenger entity
* **Trait:** `URogueEntityTraitPassenger`
* **Tag:** `FRogueTrainPassengerTag`
* **Custom fragments:** `FRoguePassengerFragment`, `FRogueDebugSlotFragment`
* **Engine fragments used:** `FTransformFragment`, `FMassMoveTargetFragment`, `FAgentRadiusFragment`, `FMassMovementParameters` (shared), `FMassRepresentationLODFragment`, `FMassVelocityFragment`
* **Runtime initialization:** `URogueTrainWorldSubsystem::ConfigurePassenger(...)`
  * Sets `FRoguePassengerFragment` to `Phase = ERoguePassengerPhase::EnteredWorld`
  * Sets `OriginStation`, `DestinationStation`, speed/target defaults
  * Sets `FAgentRadiusFragment::Radius`
  * Calls `RoguePassengerUtility::ShowPassenger(...)`

---

## 4) Architectural dependency map (Fragments ⇄ Processors)

Legend:
* `R` = read
* `W` = write
* `R/W` = both

### 4.1 Processor → fragment I/O (source-of-truth from `ConfigureQueries` + code)

| Processor                                                                                 | Phase / Group / Order                                         | Query filter              | Reads                                                                                                                         | Writes                                                                                                                               | Key calls                                                                                                                                                                                                            |
| ----------------------------------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `URoguePassengerSpawnProcessor` (`.../Passengers/RoguePassengerSpawnProcessor.cpp`)       | `FrameEnd` / `Tasks`                                          | `FRogueTrainStationTag`   | `FRogueStationQueueFragment` (declared `R/W`, used mostly `R`), `FRogueTrackSharedFragment` (via subsystem)                   | (none directly; enqueues spawn request)                                                                                              | `URogueTrainWorldSubsystem::EnqueueSpawns`                                                                                                                                                                           |
| `URoguePassengerMovementProcessor` (`.../Passengers/RoguePassengerMovementProcessor.cpp`) | `PrePhysics` / `Movement`                                     | `FRogueTrainPassengerTag` | `FTransformFragment`, `FMassMovementParameters` (shared), station `FRogueStationQueueFragment`, carriage `FTransformFragment` | `FRoguePassengerFragment`, `FMassMoveTargetFragment`, (pool enqueue)                                                                 | `AssignWaitingPoint` → `RogueStationQueueUtility::ClaimWaitingSlot`; `ToStationWaitingPoint` → `RoguePassengerQueueUtility::EnqueueAtWaitingPoint`; `ToExitSpawn` → `URogueTrainWorldSubsystem::EnqueueEntityToPool` |
| `URoguePassengerHeightProcessor` (`.../Passengers/RoguePassengerHeightProcessor.cpp`)     | `PrePhysics` / after `URoguePassengerMovementProcessor`       | `FRogueTrainPassengerTag` | (world trace)                                                                                                                 | `FTransformFragment` (Z snap)                                                                                                        | `RoguePassengerUtility::SnapToPlatform`                                                                                                                                                                              |
| `URogueTrainStationDetectProcessor` (`.../Stations/RogueTrainStationDetectProcessor.cpp`) | `PrePhysics` / before `Avoidance`                             | `FRogueTrainEngineTag`    | `FRogueTrackSharedFragment` (via subsystem), settings                                                                         | `FRogueTrainStateFragment`, `FRogueTrainTrackFollowFragment` (declared `R/W`), previous station `FRogueStationFragment::DockedTrain` | `RogueTrainUtility::FindNextStation`, `ArcDistanceWrapped`                                                                                                                                                           |
| `URogueTrainStationOpsProcessor` (`.../Stations/RogueTrainStationOpsProcessor.cpp`)       | `PrePhysics` / after `URogueTrainStationDetectProcessor`      | `FRogueTrainEngineTag`    | Station `FRogueStationQueueFragment`, carriage `FTransformFragment`, passenger fragments                                      | `FRogueTrainStateFragment`, `FRogueCarriageFragment`, `FRoguePassengerFragment`, station grid occupancy                              | `RoguePassengerUtility::{Disembark,TryBoard}`; `RogueStationQueueUtility::{PeekFromGrid,ReleaseSlot}`                                                                                                                |
| `URogueTrainHeadwayProcessor` (`.../Trains/RogueTrainHeadwayProcessor.cpp`)               | `Movement` group (phase not explicitly set)                   | `FRogueTrainEngineTag`    | `FRogueTrainTrackFollowFragment`, shared track                                                                                | `FRogueTrainStateFragment` (`HeadwaySpeedScale`, `TrainLength`)                                                                      | `RogueTrainUtility::{WrapTrackAlpha,ArcDistanceWrapped,FindNextStation}`                                                                                                                                             |
| `URogueTrainEngineMovementProcessor` (`.../Trains/RogueTrainEngineMovementProcessor.cpp`) | `Movement` group (phase not explicitly set)                   | `FRogueTrainEngineTag`    | shared track, `FRogueTrainStateFragment`                                                                                      | `FRogueTrainTrackFollowFragment`, `FTransformFragment`                                                                               | `RogueTrainUtility::GetSplineSample`                                                                                                                                                                                 |
| `URogueTrainCarriageFollowProcessor` (`.../Trains/RogueTrainCarriageFollowProcessor.cpp`) | `Movement` group / after `URogueTrainEngineMovementProcessor` | `FRogueTrainCarriageTag`  | lead’s `FRogueTrainTrackFollowFragment`, `FRogueTrainLinkFragment`                                                            | carriage `FRogueTrainTrackFollowFragment`, `FTransformFragment`                                                                      | `RogueTrainUtility::GetSplineSample`                                                                                                                                                                                 |
| `URogueDebugDataProcessor` (`.../Debug/RogueDebugDataProcessor.cpp`)                      | `FrameEnd` / `Tasks`                                          | per-entity tags           | entity transforms, follow/state/queue fragments                                                                               | (writes snapshots to subsystem arrays on game thread)                                                                                | `URogueTrainWorldSubsystem::Set*DebugSnapshot`                                                                                                                                                                       |

### 4.2 Fragment-centric view (what touches what)

```text
FRogueStationQueueFragment
  R : URoguePassengerSpawnProcessor (spawn points)
  R : URoguePassengerMovementProcessor (waiting/spawn points)
  W : URoguePassengerMovementProcessor (enqueue passenger into logical queue)
  R/W : URogueTrainStationOpsProcessor (peek grid; release slots; boarding)
  R : URogueDebugDataProcessor

FRoguePassengerFragment
  W : URogueTrainWorldSubsystem::ConfigurePassenger
  R/W : URoguePassengerMovementProcessor
  R/W : URogueTrainStationOpsProcessor (via boarding/unloading helpers)
  R : URogueDebugDataProcessor

FRogueTrainStateFragment
  W : URogueTrainWorldSubsystem::ConfigureTrain
  R/W : URogueTrainStationDetectProcessor
  R/W : URogueTrainStationOpsProcessor
  R/W : URogueTrainHeadwayProcessor
  R : URogueTrainEngineMovementProcessor
  R : URogueDebugDataProcessor

FRogueTrainTrackFollowFragment
  W : URogueTrainWorldSubsystem::ConfigureTrain / ConfigureCarriage
  R/W : URogueTrainStationDetectProcessor
  R : URogueTrainHeadwayProcessor
  R/W : URogueTrainEngineMovementProcessor
  R/W : URogueTrainCarriageFollowProcessor
  R : URogueDebugDataProcessor

FRogueCarriageFragment
  W : URogueTrainWorldSubsystem::ConfigureCarriage
  R/W : URogueTrainStationOpsProcessor
  R : URogueDebugDataProcessor

FRogueTrainLinkFragment
  W : URogueTrainWorldSubsystem::ConfigureCarriage
  R : URogueTrainCarriageFollowProcessor
  R : URogueDebugDataProcessor
```

---

## 5) Processor execution ordering (explicit constraints)

This is the **best-effort ordering** based on explicit code constraints. Some relative ordering is not explicitly constrained (and may vary by Mass configuration).

### 5.1 FrameEnd / Tasks

```text
FrameEnd / Tasks:
  URoguePassengerSpawnProcessor
  URogueDebugDataProcessor
```

Both are explicitly `ProcessingPhase = FrameEnd` and `ExecuteInGroup = Tasks`.

### 5.2 PrePhysics + Movement group (DAG)

```text
PrePhysics:
  URogueTrainStationDetectProcessor  (ExecuteBefore: Avoidance)
            |
            v
  URogueTrainStationOpsProcessor     (ExecuteAfter: StationDetect)

Movement group (relative order only where explicitly constrained):
  URogueTrainHeadwayProcessor
  URogueTrainEngineMovementProcessor
            |
            v
  URogueTrainCarriageFollowProcessor (ExecuteAfter: EngineMovement)

  URoguePassengerMovementProcessor
            |
            v
  URoguePassengerHeightProcessor     (ExecuteAfter: PassengerMovement)
```

> Important: there is **no explicit ordering constraint** between passenger movement and train movement processors besides the constraints shown above.

---

## 6) Who spawns what (Subsystem + processors)

### 6.1 World initialization (function-by-function)

Source: `URogueTrainWorldSubsystem` in `.../Subsystems/RogueTrainWorldSubsystem.cpp`.

```text
UWorld
  -> URogueTrainWorldSubsystem::Initialize()
       -> InitEntityManagement()
       -> InitTemplateConfigs()           // loads `StationConfig`, `TrainEngineConfig`, ... from settings
       -> (editor) InitDebugData()

  -> URogueTrainWorldSubsystem::OnWorldBeginPlay(UWorld&)
       -> DiscoverSplineFromSettings()    // finds `TrackSpline`
       -> InitConfigTemplates(World)      // creates entity templates from `UMassEntityConfigAsset`
       -> StartSpawnManager()             // starts timer-driven spawn processing
       -> CreateStations()                // enqueues station spawn requests
```

### 6.2 Spawn manager pipeline (pooling + spawn + per-type configuration)

```text
FTimerManager tick
  -> URogueTrainWorldSubsystem::SpawnManager()
       -> URogueTrainWorldSubsystem::ProcessPendingSpawns()
            -> (if pooled entity exists) reuse entity + clear `FRoguePooledEntityTag`
               else -> UMassSpawnerSubsystem::SpawnEntities(Template, Count, Transforms)
            -> for each spawned entity:
                 URogueTrainWorldSubsystem::ConfigureSpawnedEntity(Request, Entity)
                    -> ConfigureStation()  / ConfigureTrain() / ConfigureCarriage() / ConfigurePassenger()
            -> Request.OnSpawned(SpawnedEntities)  // optional callback (used by trains to spawn carriages)
```

### 6.3 Station spawning

```text
URogueTrainWorldSubsystem::CreateStations()
  -> BuildStationPlatformData()
  -> for each station platform:
       EnqueueSpawns({Type=Station, PlatformData, StationIdx, Transform})

ConfigureStation(Request, StationEntity)
  -> init `FRogueStationFragment`
  -> init `FRogueStationQueueFragment` (waiting/spawn points)
  -> build grids: RogueStationQueueUtility::BuildGridForWaitingPoint(...)
  -> if all stations spawned:
       TrackActors[i]->BuildTrackMeshes()
       CreateTrains()
```

### 6.4 Train spawning (engine spawns carriages)

```text
URogueTrainWorldSubsystem::CreateTrains()
  -> for each desired train:
       EnqueueSpawns({Type=TrainEngine, StartAlpha, Transform, StationIdx, OnSpawned=...})

OnSpawned(LeadEngineEntity)
  -> for c=1..N:
       EnqueueSpawns({Type=TrainCarriage, LeadHandle=LeadEngineEntity, CarriageIndex=c, Spacing, StartAlpha, Transform})

ConfigureTrain(Request, EngineEntity)
  -> init `FRogueTrainStateFragment` and `FRogueTrainTrackFollowFragment`

ConfigureCarriage(Request, CarriageEntity)
  -> init `FRogueTrainLinkFragment` and `FRogueCarriageFragment`
  -> TrainState(CarriageEntity.LeadHandle).Carriages.Add(CarriageEntity)
```

### 6.5 Passenger spawning (processor-driven)

```text
URoguePassengerSpawnProcessor::Execute()
  -> pick random station from `FRogueTrackSharedFragment`
  -> read that station's `FRogueStationQueueFragment` for spawn/wait points
  -> TrainSubsystem->EnqueueSpawns({Type=Passenger, OriginStation, DestinationStation, Transform=SpawnLoc, ...})

ConfigurePassenger(Request, PassengerEntity)
  -> init `FRoguePassengerFragment` (Phase=EnteredWorld)
  -> init `FAgentRadiusFragment`
  -> RoguePassengerUtility::ShowPassenger(...)
```

---

## 7) Passenger lifecycle (processor + station ops interaction)

This shows the **cross-entity** behavior: passenger logic is split between passenger processors and station ops.

```text
Passenger spawned (ConfigurePassenger)
  Phase = EnteredWorld
    |
    v
URoguePassengerMovementProcessor::AssignWaitingPoint()
  -> RogueStationQueueUtility::ClaimWaitingSlot(StationQueueFragment, WaitingPointIdx, Passenger, OutSlotPos)
  -> Passenger.Target = SlotPos
  -> Phase = ToStationWaitingPoint
    |
    v
URoguePassengerMovementProcessor::ToStationWaitingPoint()
  (on arrival)
  -> RoguePassengerQueueUtility::EnqueueAtWaitingPoint(StationQueueFragment, WaitingPointIdx, Passenger, DestStation, Time)
  -> Passenger.bWaiting = true
    |
    v
URogueTrainStationOpsProcessor (when a train is loading at this station)
  -> RogueStationQueueUtility::PeekFromGrid(...)
  -> RoguePassengerUtility::TryBoard(...)
      -> Passenger.VehicleHandle = CarriageEntity
      -> Passenger.Phase = ToAssignedCarriage
  -> RogueStationQueueUtility::ReleaseSlot(...)
  -> Passenger.bWaiting = false
    |
    v
URoguePassengerMovementProcessor::ToAssignedCarriage()
  (on arrival to carriage)
  -> RoguePassengerUtility::HidePassenger(...)
  -> Phase = RideOnTrain
    |
    v
URogueTrainStationOpsProcessor (when a train is unloading at destination station)
  -> RoguePassengerUtility::Disembark(...)
      -> RoguePassengerUtility::ShowPassenger(...)
      -> Passenger.VehicleHandle = invalid
      -> Passenger.Phase = UnloadAtStation
    |
    v
URoguePassengerMovementProcessor::UnloadAtStation()
  -> choose nearest waiting point at destination
  -> Phase = ToPostUnloadWaitingPoint
    |
    v
URoguePassengerMovementProcessor::ToPostUnloadWaitingPoint()
  -> choose nearest destination spawn point
  -> Phase = ToExitSpawn
    |
    v
URoguePassengerMovementProcessor::ToExitSpawn()
  -> (optional) ReleaseSlot at origin
  -> Phase = Pool
  -> TrainSubsystem.EnqueueEntityToPool(Passenger)
```

---

## 8) Train + station interaction (station detect + ops)

```text
URogueTrainStationDetectProcessor::Execute()
  -> if no target station: FindNextStation()
  -> compute distance to dock alpha
  -> sets:
       State.bIsStopping (approach)
       State.bAtStation + StationTimeRemaining (dwell)
       State.TargetStationIdx advance on miss/depart
  -> on depart: clears PreviousStationFragment->DockedTrain

URogueTrainStationOpsProcessor::Execute()  (only when State.bAtStation)
  -> manages State.StationTrainPhase transitions
  -> if Unloading:
       for each carriage:
         disembark passengers whose DestinationStation == CurrentStation
  -> if Loading:
       for each carriage:
         peek passengers in station grids (nearest waiting point first)
         board up to budget per tick
```

---

## 9) Debug snapshot pipeline (FrameEnd)

`URogueDebugDataProcessor` runs at `FrameEnd/Tasks`, reads entity fragments and writes debug snapshots into `URogueTrainWorldSubsystem` using `AsyncTask(ENamedThreads::GameThread, ...)`.

This is intentionally **read-only** with respect to Mass entities (it only reads fragments).
