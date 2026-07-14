# RogueMassExample - Architectural Overview

This project is a high-performance simulation of a train network with passengers using Unreal Engine's **MassEntity** (ECS) framework. It demonstrates complex agent behaviors, entity lifecycle management, and spatial coordination using a data-oriented approach.

### 🏗 Architectural Dependency Map

The following map shows the relationships between core ECS components (Fragments, Tags, and Shared Fragments) and the Systems (Processors) that operate on them.

```text
+------------------------+      +---------------------------+      +--------------------------+
|       FRAGMENTS        |      |        PROCESSORS         |      |          TAGS            |
+------------------------+      +---------------------------+      +--------------------------+
| FRoguePassengerFragment| <--- | RoguePassengerMovement    | <--- | FRogueTrainPassengerTag  |
| FRogueStationQueueFrag | <--- | RoguePassengerSpawn       | <--- | FRogueTrainStationTag    |
| FRogueTrainStateFrag   | <--- | RogueTrainEngineMovement  | <--- | FRogueTrainEngineTag     |
| FRogueTrainTrackFollow | <--- | RogueTrainCarriageFollow  | <--- | FRogueTrainCarriageTag   |
| FRogueCarriageFragment | <--- | RogueTrainStationOps      |      +--------------------------+
| FRogueTrainLinkFragment| <--- | RogueTrainStationDetect   |
+------------------------+      +---------------------------+
           ^                                  |
           |          +-----------------------+
           |          |
           v          v
+-----------------------------------------------------------+
|                  URogueTrainWorldSubsystem                |
+-----------------------------------------------------------+
| - Manages Entity Lifecycle (Spawning/Pooling)             |
| - Holds FRogueTrackSharedFragment (Spline & Station Data) |
| - Coordinates Global Simulation State                     |
+-----------------------------------------------------------+
```

### 🔄 Entity Lifecycle & Data Flow

This diagram illustrates how entities are spawned, which processors handle their logic, and how they interact with data fragments.

```text
[ INITIALIZATION ]
       |
       v
+---------------------------+      Spawns      +---------------------------+
| RogueTrainWorldSubsystem  | ---------------> |      Station Entities     |
+---------------------------+                  +---------------------------+
       |                                       | - FRogueStationQueueFrag  |
       |  Once Stations Ready                  | - FRogueStationFragment   |
       v                                       +---------------------------+
+---------------------------+
|       Train Entities      | <--------------- [ URogueTrainWorldSubsystem ]
+---------------------------+
| - FRogueTrainStateFrag    |
| - FRogueTrainTrackFollow  |
+---------------------------+

[ SIMULATION LOOP ]

    PASSENGER SPAWN FLOW:
    +---------------------------+      Request Spawn      +---------------------------+
    | RoguePassengerSpawnProc   | ----------------------> | RogueTrainWorldSubsystem  |
    +---------------------------+                         +---------------------------+
                 |                                                 |
                 | (Reads Station Queue for Spawn Points)          | (Spawns/Retrieves from Pool)
                 v                                                 v
    +---------------------------+                         +---------------------------+
    |  FRoguePassengerFragment  | <---------------------- |     Passenger Entity      |
    +---------------------------+                         +---------------------------+

    PASSENGER BEHAVIOR FLOW:
    1. RoguePassengerMovementProcessor
       - Reads: FRoguePassengerFragment, FTransformFragment
       - Writes: FMassMoveTargetFragment, FRoguePassengerFragment
       - Logic: Moves passenger to station waiting points -> handles boarding/unloading -> exits world.

    TRAIN MOVEMENT FLOW:
    1. RogueTrainStationDetectProcessor
       - Reads/Writes: FRogueTrainStateFragment, FRogueTrainTrackFollowFragment
       - Logic: Detects station approach, manages dwell time, and triggers station phases.
    2. RogueTrainEngineMovementProcessor
       - Reads: FRogueTrainStateFragment
       - Writes: FRogueTrainTrackFollowFragment, FTransformFragment
       - Logic: Updates engine position along the spline based on current speed/state.
    3. RogueTrainCarriageFollowProcessor
       - Reads: FRogueTrainLinkFragment, FRogueTrainTrackFollowFragment (of Lead)
       - Writes: FRogueTrainTrackFollowFragment (Self), FTransformFragment
       - Logic: Ensures carriages maintain correct spacing behind the engine.

    STATION OPERATIONS FLOW:
    1. RogueTrainStationOpsProcessor
       - Reads/Writes: FRogueTrainStateFragment, FRogueCarriageFragment, FRogueStationQueueFragment
       - Logic:
         - [Unloading Phase]: Moves passengers from Carriage -> Station (Target: Waiting Point).
         - [Loading Phase]: Moves passengers from Station Queue -> Carriage.
```

### 🛠 Core Components Detail

| Component                          | Responsibility                                                          |
| :--------------------------------- | :---------------------------------------------------------------------- |
| **FRoguePassengerFragment**        | Stores state (Phase), target destination, and current vehicle handle.   |
| **FRogueStationQueueFragment**     | Manages waiting grids, passenger queues, and spatial spawn/wait points. |
| **FRogueTrainStateFragment**       | Tracks train velocity, current station index, and dwell timers.         |
| **FRogueTrainTrackFollowFragment** | Stores the `Alpha` [0..1] position along the track spline.              |
| **FRogueTrackSharedFragment**      | Shared data containing the USplineComponent and all station metadata.   |
| **URogueTrainWorldSubsystem**      | The central "hub" for ECS/Actor bridging and mass entity spawning.      |
