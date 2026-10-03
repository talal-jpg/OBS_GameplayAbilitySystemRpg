AuraRedo3 · redo3 · a63041a

# Aura Spell Menu Flow

How the spell menu widget, its controller, the ability system component and the player state work together, from opening the menu to equipping a spell into an input slot.

Runs on the owning client Runs on the server Reliable RPC crossing the network

1. [Structure](#structure)
2. [Opening the menu](#open)
3. [Selecting a globe](#select)
4. [Spending a point](#spend)
5. [Equipping into a slot](#equip)

OVERVIEW

## Structure

The widget only talks to its controller. The controller sends requests to the server through the ability system component, and hears back through delegates on the client copy of the ASC and the player state.

```mermaid
flowchart LR
  subgraph UI["Client UI"]
    WBP["Spell menu widget (Blueprint)globes, Equip and Spend buttons, slot rows"]:::client
    SMWC["UMySpellMenuWidgetControllerSelectedAbility, bWaitingForEquippedRowPress"]:::client
    OWC["UMyOverlayWidgetControllercaches the spell menu controller, HUD slots"]:::client
    DA["UDA_MyAbilityInfoicon and cooldown tag per ability"]:::client
  end
  subgraph CL["Client replicated state"]
    CASC["UMyAbilitySystemComponent (client copy)specs tagged Ability, Input, Status"]:::client
    CPS["AMyPlayerState (client)SpellPoints via OnRep"]:::client
  end
  subgraph SV["Server authority"]
    SASC["UMyAbilitySystemComponent (server)Server_EquipAbility, Server_SpendSpellPoint"]:::server
    SPS["AMyPlayerState (server)AddToSpellPoints"]:::server
  end
  WBP -- "SpellGlobeClicked, EquipButtonPressed,EquippedRowPressed, SpendSpellPointButtonPressed" --> SMWC
  SMWC -- "ability info, button state,globe deselect, spell points" --> WBP
  OWC -- "owns and caches" --> SMWC
  SMWC -. "reads" .-> DA
  SMWC -- "Server RPCs" --> SASC
  SASC -- "Client_EquipAbility,Client_UpdateAbilityStatus" --> CASC
  SASC -. "MarkAbilitySpecDirty,spec replication" .-> CASC
  CASC -- "OnAbilityEquippedDelegate,OnAbilityStatusChangedDelegate" --> SMWC
  CASC -- "OnAbilityEquippedDelegate" --> OWC
  SASC --> SPS
  SPS -. "SpellPoints replication" .-> CPS
  CPS -- "OnSpellPointsChangedDelegate" --> SMWC
  classDef client fill:#dbe7f6,stroke:#4d77b3,color:#12263f
  classDef server fill:#f3dcc2,stroke:#a8642a,color:#2e1a07
```

Solid arrows are calls, RPCs and delegate broadcasts. Dotted arrows are property replication or plain reads.

FLOW 1

## Opening the menu

The controller is created once and cached on the overlay controller. Every later open reuses it, binds nothing new, and starts with nothing selected.

```mermaid
flowchart TD
  A["Player opens the spell menu"]:::client --> B["Blueprint callsGetSpellMenuWidgetController"]:::client
  B --> C{"Overlay controller alreadyholds a spell menu controller?"}
  C -- "yes" --> D["Use the cached controller"]:::client
  C -- "no" --> E["NewObject, outer = overlay controllerSetWidgetControllerParamsstore it on the overlay controller"]:::client
  E --> D
  D --> F["Blueprint callsBindCallbacksToDependencies"]:::client
  F --> G{"bCallbacksBound?"}
  G -- "yes" --> J
  G -- "no" --> I["Bind weak lambdas toASC OnAbilityEquippedDelegatePlayerState OnSpellPointsChangedDelegateASC OnAbilityStatusChangedDelegate"]:::client
  I --> J["Blueprint callsBroadcastInitialValues"]:::client
  J --> K["Reset SelectedAbilitybWaitingForEquippedRowPress = false"]:::client
  K --> L{"bAbilitiesGiven?"}
  L -- "yes" --> M["BroadcastAbilityInfoone FAbilityInfo per spec, with its InputTag"]:::client
  L -- "no" --> N
  M --> N["Broadcast both buttons disabledBroadcast current SpellPoints"]:::client
  N --> O["Widget draws globes, slot rowsand the point count"]:::client
  classDef client fill:#dbe7f6,stroke:#4d77b3,color:#12263f
```

**In code**

- `Private/StaticLib/MyBPFuncLib.cpp:42` GetSpellMenuWidgetController
- `Private/UI/WidgetControllers/SpellMenu/MySpellMenuWidgetController.cpp:12` BindCallbacksToDependencies
- `Private/UI/WidgetControllers/SpellMenu/MySpellMenuWidgetController.cpp:105` BroadcastInitialValues
- `Private/UI/WidgetControllers/MyWidgetController.cpp:26` BroadcastAbilityInfo

**Assumed from the Blueprint:** the widget calls BindCallbacksToDependencies and BroadcastInitialValues each time it opens. Those calls live in the widget Blueprint, not in C++.

FLOW 2

## Selecting a globe

Clicking a globe is the only thing that sets the selection. The buttons are worked out from the ability's status and the spell point count.

```mermaid
flowchart TD
  A["Player clicks a spell globe"]:::client --> B["SpellGlobeClicked(AbilityTag)"]:::client
  B --> C["Cancel any pending equipbWaitingForEquippedRowPress = false"]:::client
  C --> D{"AbilityTag valid?"}
  D -- "no" --> X["Print Invalid Ability Tag, stop"]:::client
  D -- "yes" --> E["GetStatusFromAbiltyTagno spec on the client means Locked"]:::client
  E --> F["SelectedAbility = tag and status"]:::client
  F --> G["ShouldEnableButtons"]:::client
  G --> H["Spend: status Eligible, Unlocked or Equippedand SpellPoints above 0"]:::client
  G --> I["Equip: status Unlocked or Equipped"]:::client
  H --> J["OnSpellGlobeClickedBroadCastShouldEnableDelegatebEnableEquip, bEnableSpendPoint, tag text"]:::client
  I --> J
  classDef client fill:#dbe7f6,stroke:#4d77b3,color:#12263f
```

**In code**

- `Private/UI/WidgetControllers/SpellMenu/MySpellMenuWidgetController.cpp:121` SpellGlobeClicked
- `Private/UI/WidgetControllers/SpellMenu/MySpellMenuWidgetController.cpp:89` ShouldEnableButtons
- `Private/AbilitySystem/MyAbilitySystemComponent.cpp:31` GetStatusFromAbiltyTag

FLOW 3

## Spending a spell point

The server takes the point and either unlocks the ability or levels it up. Two separate paths bring the result back: the status RPC and the replicated point count.

```mermaid
flowchart TD
  A["Player presses Spend Point"]:::client --> B{"Selection is Ability.Noneor status Locked?"}
  B -- "yes" --> X["Do nothing"]:::client
  B -- "no" --> C{"SpellPoints above 0?"}
  C -- "no" --> X
  C -- "yes" --> D["Server_SpendSpellPoint(AbilityTag, StatusTag)"]:::rpc
  D --> E["AddToSpellPoints(-1)"]:::server
  E --> F{"Status the client sent"}
  F -- "Unlocked or Equipped" --> G["Spec Level + 1"]:::server
  F -- "Eligible" --> H["Swap status tag to UnlockedMarkAbilitySpecDirty"]:::server
  G --> I["Client_UpdateAbilityStatus(tag, status, level)"]:::rpc
  H --> I
  I --> J["OnAbilityStatusChangedDelegate"]:::client
  J --> K{"Is it the selected ability?"}
  K -- "no" --> Y["Ignore"]:::client
  K -- "yes" --> L["Update SelectedAbility.StatusTagrecompute and broadcast buttons"]:::client
  E -. "SpellPoints replicates" .-> M["OnRep_SpellPointsOnSpellPointsChangedDelegate"]:::client
  M --> N["Broadcast new point countrecompute buttons for the selection"]:::client
  classDef client fill:#dbe7f6,stroke:#4d77b3,color:#12263f
  classDef server fill:#f3dcc2,stroke:#a8642a,color:#2e1a07
  classDef rpc fill:#e4dcf5,stroke:#6b54a8,color:#1e1440
```

**In code**

- `Private/UI/WidgetControllers/SpellMenu/MySpellMenuWidgetController.cpp:151` SpendSpellPointButtonPressed
- `Private/AbilitySystem/MyAbilitySystemComponent.cpp:214` Server_SpendSpellPoint
- `Private/AbilitySystem/MyAbilitySystemComponent.cpp:244` Client_UpdateAbilityStatus

**Not yet hardened:** Server_SpendSpellPoint still trusts the StatusTag the client sends and does not null-check the spec. The equip path below was fixed for the same problems; this one was not part of that change.

FLOW 4

## Equipping into a slot

Equip is two clicks: the button arms it, then a slot row sends it. The server reads the current slot from its own spec, empties the target slot, and tells the client about every ability it changed.

```mermaid
flowchart TD
  A["Player presses Equip"]:::client --> B{"Selected ability isUnlocked or Equipped?"}
  B -- "no" --> X["Do nothing"]:::client
  B -- "yes" --> C["bWaitingForEquippedRowPress = true"]:::client
  C --> D["Player clicks a slot row"]:::client
  D --> E["EquippedRowPressed(InputTag)"]:::client
  E --> F{"Waiting for a row?"}
  F -- "no" --> X
  F -- "yes" --> G["Server_EquipAbility(AbilityTag, InputTag)"]:::rpc
  G --> G2["Deselect the globe, clear SelectedAbility,disable both buttons"]:::client
  G --> S1{"Slot is an Input tag other than Input.None,spec exists, status Unlocked or Equipped?"}
  S1 -- "no" --> SX["Reject, nothing changes"]:::server
  S1 -- "yes" --> S2["PrevInputTag = slot read from the server spec"]:::server
  S2 --> S3{"Already in this slot?"}
  S3 -- "yes" --> S7
  S3 -- "no" --> S4["For each other ability in the slot:cancel if active, set Input.None and Unlocked,MarkAbilitySpecDirty"]:::server
  S4 --> S5["Client_EquipAbility(other, Input.None, Unlocked, slot)sent first"]:::rpc
  S5 --> S6["Cancel this ability if activeSetInputTagOnSpec(slot)"]:::server
  S6 --> S7["SetStatusTagOnSpec(Equipped)MarkAbilitySpecDirty"]:::server
  S7 --> S8["Client_EquipAbility(ability, slot, Equipped, PrevInputTag)"]:::rpc
  S5 --> R
  S8 --> R["OnAbilityEquippedDelegate on the client"]:::client
  R --> R1{"PrevInputTag validand not Input.None?"}
  R1 -- "yes" --> R2["Broadcast empty info for the previous slot"]:::client
  R1 -- "no" --> R3
  R2 --> R3["Broadcast ability info with the new slot and status"]:::client
  R3 --> R4["Spell menu: globe status, slot rowsOverlay: HUD spell slots"]:::client
  classDef client fill:#dbe7f6,stroke:#4d77b3,color:#12263f
  classDef server fill:#f3dcc2,stroke:#a8642a,color:#2e1a07
  classDef rpc fill:#e4dcf5,stroke:#6b54a8,color:#1e1440
```

**In code**

- `Private/UI/WidgetControllers/SpellMenu/MySpellMenuWidgetController.cpp:144` EquipButtonPressed
- `Private/UI/WidgetControllers/SpellMenu/MySpellMenuWidgetController.cpp:162` EquippedRowPressed
- `Private/AbilitySystem/MyAbilitySystemComponent.cpp:250` Server_EquipAbility
- `Private/AbilitySystem/MyAbilitySystemComponent.cpp:112` SetInputTagOnSpec, `:121` SetStatusTagOnSpec
- `Private/AbilitySystem/MyAbilitySystemComponent.cpp:295` Client_EquipAbility
- `Private/UI/WidgetControllers/Overlay/MyOverlayWidgetController.cpp:93` overlay equip handler

Equipping into an occupied slot does not swap. The ability that was there loses its slot and goes back to Unlocked. Reliable RPCs to the same actor arrive in order, so the client clears the slot before it refills it.

All paths are under `Source/Aura/`. Line numbers match commit a63041a on redo3.