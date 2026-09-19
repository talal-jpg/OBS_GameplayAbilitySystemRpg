                         ┌───────────────────────┐
                         │   AMyHexPlatform      │
                         │  ONE ACTOR           │
                         │  bReplicates = true  │
                         └───────────┬───────────┘
                                     │
             ┌───────────────────────┼───────────────────────┐
             │                       │                       │
             ▼                       ▼                       ▼
       CONSTRUCTION               SERVER                  CLIENT
             │                       │                       │
             ▼                       │                       │
      BuildHexGrid()                 │                       │
             │                       │                       │
      ┌──────┴───────┐               │                       │
      ▼              ▼               │                       │
 GenerateHexGrid  HexToWorld         │                       │
      │              │               │                       │
      └──────┬───────┘               │                       │
             ▼                       │                       │
      AddHexInstance()               │                       │
             │                       │                       │
       ┌─────┴─────┐                 │                       │
       ▼           ▼                 ▼                       │
     ISM       HexMap        GenerateAndActivate...          │
               + InstanceToHex        │                      │
                                      ▼                      │
                              GenerateChain()                │
                                      │                      │
                                      ▼                      │
                               ActivateChain()               │
                                      │                      │
                                      ▼                      │
                                ActivateHex()                │
                                      │                      │
                       ┌──────────────┴──────────────┐       │
                       ▼                             ▼       │
                ActivatedTiles                 ActivateHexLocal
                  [REPLICATED]                       │       │
                       │                             │       │
                       │                             ▼       │
                       │                    Remove ISM instance
                       │                             │       │
                       │                             ▼       │
                       │                    Spawn skeletal mesh
                       │                             │       │
                       │                             ▼       │
                       │                       Play RBD
                       │                             │       │
                       │                             ▼       │
                       │                    Remove after anim
                       │                                     │
                       │                                     │
                       └──────────── REPLICATION ────────────►│
                                                             │
                                                             ▼
                                                     OnRep_ActivatedTiles
                                                             │
                                                             ▼
                                                     ActivateHexLocal()
                                                             │
                                                             ▼
                                                     Same visual transition


![[HexPlatformWithISMs]]
