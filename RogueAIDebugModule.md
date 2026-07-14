# Rogue AI Debugger

The **Rogue AI Debugger** is a specialized debugging tool built on top of Unreal Engine's **Gameplay Debugger** framework. It provides real-time visualization and state inspection for Mass-based entities in the Rogue Mass Example, including Passengers, Trains, Carriages, and Stations.

## Table of Contents
1. [Core Functionality](#core-functionality)
2. [Workings & Data Flow](#workings--data-flow)
3. [Key Classes & Responsibilities](#key-classes--responsibilities)
4. [Structural Dependencies](#structural-dependencies)
5. [How to Use](#how-to-use)

---

## Core Functionality
The debugger allows developers to:
- **Visualize Entity State**: See real-time data like current phase, target station, speed, and movement goals directly above entities in the game world.
- **Inspect Mass Fragments**: Bridging the gap between Mass's data-oriented fragments and the human-readable Gameplay Debugger.
- **Performance-Friendly**: Uses a snapshot system to avoid heavy per-frame queries during visualization.
- **Categorized Data**: Organized into logical groups such as "Movement Info", "Station Info", and "Wait Info".

---

## Workings & Data Flow

The debugger follows a **Producer-Consumer** pattern mediated by a World Subsystem. This decouples the high-frequency Mass simulation from the visualization layer.

### 1. Data Collection (The Producer)
`URogueDebugDataProcessor` is a Mass Processor that executes within the Mass pipeline. It queries entities for relevant fragments (Transform, Passenger, Train, etc.) and packs their data into standardized debug structs. 

### 2. Synchronization
Since Mass processors can run on worker threads, the processor uses an `AsyncTask` to send the gathered data snapshots to the `URogueTrainWorldSubsystem` on the **Game Thread**.

### 3. Data Storage (The Hub)
`URogueTrainWorldSubsystem` acts as the central repository. It stores fixed-size snapshots of the debug data, indexed by a "Debug Slot" assigned to entities when they are spawned.

### 4. Visualization (The Consumer)
When the Gameplay Debugger is active, `FRogueAIDebugCategory` collects the latest snapshots from the subsystem. It then uses the `FGameplayDebuggerEntityOverheadTiles` utility to format and render this data on the screen.

### ASCII Flow Diagram
```text
[ Mass Simulation ]
       |
       v
[ URogueDebugDataProcessor ] ----( Extract Data from Fragments )
       |
       | (AsyncTask / Game Thread)
       v
[ URogueTrainWorldSubsystem ] ---( Store Snapshots )
       |
       | (Request Data)
       v
[ FRogueAIDebugCategory ] <------( Gameplay Debugger Active )
       |
       v
[ FGameplayDebuggerEntityOverheadTiles ] --( Format )
       |
       v
[ Screen Rendering (Canvas) ]
```

---

## Key Classes & Responsibilities

| Class                                  | Module             | Role                | Description                                                    |
| :------------------------------------- | :----------------- | :------------------ | :------------------------------------------------------------- |
| `URogueDebugDataProcessor`             | `RogueMassExample` | **Writer**          | Mass processor that collects fragment data into debug structs. |
| `URogueTrainWorldSubsystem`            | `RogueMassExample` | **Data Hub**        | Stores debug snapshots and manages entity registration.        |
| `FRogueAIDebugCategory`                | `RogueAIDebugger`  | **Reader/Renderer** | Implements the Gameplay Debugger category logic.               |
| `FRogueEntityDebugData`                | `RogueMassExample` | **Data Contract**   | Defines `FRogueDebugPassenger`, `FRogueDebugTrain`, etc.       |
| `FGameplayDebuggerEntityOverheadTiles` | `RogueAIDebugger`  | **Utility**         | Handles the layout and drawing of text tiles in the world.     |

---

## Structural Dependencies

The debugger is split into two modules to maintain a clean separation between simulation logic and editor/debug tools.

```text
[ RogueAIDebugger Module ]
          |
          |-- Depends On --> [ GameplayDebugger (Engine) ]
          |
          |-- Depends On --> [ RogueMassExample Module ]
                                    |
                                    |-- Defines --> [ URogueTrainWorldSubsystem ]
                                    |-- Defines --> [ URogueDebugDataProcessor ]
                                    |-- Defines --> [ FRogueEntityDebugData ]
```

---

## How to Use
1. **Enable Gameplay Debugger**: Press the apostrophe key (`'`) in-game.
2. **Select Rogue Category**: Use the Numpad keys (default is `9` for RogueMass) to enable the Rogue AI Debugger category.
3. **Toggle Overheads**:
   - `Z`: Toggle Passenger Overheads
   - `X`: Toggle Train Overheads
   - `C`: Toggle Carriage Overheads
   - `V`: Toggle Station Overheads
   - `B`: Toggle Track Overheads
4. **Settings**: Configurations can be found in **Project Settings -> Rogue AI Debugger**.
