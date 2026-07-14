# RogueMassExample - Architectural & Data Flow Deep Dive

This document provides a detailed map of the project's architecture, entity lifecycle, and the execution flow of its MassEntity ECS systems.

## 🏗 Architectural Dependency Map

The following map illustrates the relationships between **Fragments**, **Processors**, and **Tags**. It shows how data is partitioned and which systems are responsible for operating on specific data sets.

```text
+-----------------------+       +----------------------------+       +-----------------------+
|       FRAGMENTS       |       |        PROCESSORS          |       |         TAGS          |
+-----------------------+       +----------------------------+       +-----------------------+
| FRoguePassengerFrag   | <---> | RoguePassengerMovement     | <---  | FRogueTrainPassenger  |
| FRogueStationQueueFrag| <---> | RoguePassengerSpawn        | <---  | FRogueTrainStation    |
| FRogueTrainStateFrag  | <---> | RogueTrainEngineMovement   | <---  | FRogueTrainEngine     |
| FRogueTrainTrackFollow| <---> | RogueTrainCarriageFollow   | <---  | FRogueTrainCarriage   |
| FRogueCarriageFrag    | <---> | RogueTrainStationOps       |       +-----------------------+
| FRogueTrainLinkFrag   | ----> | RogueTrainStationDetect    |
| FRogueStationFrag     | <---> | RogueTrainHeadway          |
| FTransformFragment    | <---> | RoguePassengerHeight       |
+-----------------------+       +----------------------------+
            ^                                |
            |         (Subsystem Access)     |
            +--------------------------------+
                             |
                +-------------------------+
                | URogueTrainWorldSubsys  |
                +-------------------------+
```

---

## 🔄 Entity Lifecycle & Execution Flow

This flow diagram details which classes are responsible for spawning entities and the order in which processors handle them during the simulation loop.

```text
[ INITIALIZATION ]
       |
       v
+--------------------------+          Spawns          +--------------------------+
| URogueTrainWorldSubsys   | -----------------------> |     Station Entities     |
+--------------------------+                          +--------------------------+
       |                                              | FRogueStationQueueFrag   |
       |  Once Stations Ready                         | FRogueStationFrag        |
       v                                              +--------------------------+
+--------------------------+
|  Train Engine Entities   | <----------------------- [ URogueTrainWorldSubsys ]
+--------------------------+
| FRogueTrainStateFrag     |          Spawns          +--------------------------+
| FRogueTrainTrackFollow   | -----------------------> |    Carriage Entities     |
+--------------------------+                          +--------------------------+
                                                      | FRogueCarriageFrag       |
                                                      | FRogueTrainLinkFrag      |
                                                      +--------------------------+

[ SIMULATION LOOP ]

  PHASE: PrePhysics (Agent Logic & Station Ops)
  1. RogueTrainStationDetectProcessor
     - Updates train state (IsStopping, AtStation, TargetStation).
     - R/W: FRogueTrainStateFrag, FRogueTrainTrackFollow
     - Write: FRogueStationFrag (DockedTrain handle)
  2. RogueTrainStationOpsProcessor (Executes after Detect)
     - Handles passenger boarding/unloading logic.
     - R/W: FRogueTrainStateFrag, FRogueCarriageFrag, FRogueStationQueueFrag
     - R/W: FRoguePassengerFrag (Phase & VehicleHandle)
  3. RoguePassengerMovementProcessor
     - Handles passenger state transitions (Waiting -> Boarding -> Exiting).
     - R/W: FRoguePassengerFrag, FTransformFrag, FMassMoveTargetFrag
     - Read: FRogueStationQueueFrag (Waiting Grid slots)
  4. RoguePassengerHeightProcessor (Executes after Movement)
     - Snaps passenger transforms to platform height.
     - R/W: FTransformFrag

  PHASE: Update / Movement Group (Physics-like updates)
  5. RogueTrainHeadwayProcessor
     - Adjusts speed based on distance to the train ahead.
     - R:   FRogueTrainTrackFollow
     - R/W: FRogueTrainStateFrag (HeadwaySpeedScale)
  6. RogueTrainEngineMovementProcessor
     - Moves the engine along the spline based on speed & state.
     - R/W: FRogueTrainTrackFollow, FRogueTrainStateFrag, FTransformFrag
  7. RogueTrainCarriageFollowProcessor (Executes after Engine)
     - Ensures carriages follow the lead engine at correct spacing.
     - R/W: FRogueTrainTrackFollow, FTransformFrag
     - Read: FRogueTrainLinkFrag (Lead handle & index)

  PHASE: FrameEnd (Maintenance & Debug)
  8. RoguePassengerSpawnProcessor
     - Periodically requests new passenger spawns at stations.
     - R/W: FRogueStationQueueFrag
     - Calls: URogueTrainWorldSubsys::EnqueueSpawns
  9. RogueDebugDataProcessor
     - Collects snapshot data for the UI and AI Debugger.
     - Read: All Fragments
```

---

## 🛠 Component & Fragment Responsibilities

### Fragments (Data)
*   **FRoguePassengerFragment**: Tracks passenger phase, current target, and assigned vehicle/station handles.
*   **FRogueStationQueueFragment**: Manages the spatial waiting grid and the queue of passengers for each waiting point.
*   **FRogueTrainStateFragment**: Core state for trains (speed, dwell timers, carriages list, current station).
*   **FRogueTrainTrackFollowFragment**: Stores the normalized `Alpha` position [0..1] along the world track spline.
*   **FRogueCarriageFragment**: Manages carriage capacity and the list of current occupants.
*   **FRogueTrainLinkFragment**: Defines the connection between a carriage and its lead engine.
*   **FRogueStationFragment**: Stores station-specific runtime data like the currently docked train.
*   **FRogueTrackSharedFragment**: Shared data containing the `USplineComponent` and platform metadata.

### Processors (Logic)
*   **Movement Group**: Handles the actual spatial updates for trains (Engines and Carriages) and ensures safe spacing.
*   **Station Systems**: Handle the high-level logic of trains arriving, dwelling, and interacting with station queues.
*   **Passenger Systems**: Orchestrate individual passenger behavior from spawning to boarding and eventually exiting the simulation.
*   **Subsystem (URogueTrainWorldSubsystem)**: Acts as the central hub for entity lifecycle management (Spawning/Pooling) and bridging between Actor-based track data and the ECS simulation.
