↑ [[Redo3 Artifacts - Menus]] · [[Redo3 Artifacts Index]]

# Aura Spell Menu Flow

claude.ai Artifact · walkthrough · [open the artifact](https://claude.ai/artifact/EmiL5reCd4EobeK1q5Dk4e)

A page (tagged 'AuraRedo3 · redo3') explaining how the spell menu widget, its controller, the ability system component and the player state work together, from opening the menu to equipping a spell into an input slot. The widget only talks to UMySpellMenuWidgetController, which sends requests to the server through UMyAbilitySystemComponent RPCs and hears back through delegates on the client copy of the ASC and AMyPlayerState. Mermaid diagrams show the structure and four flows: opening the menu, selecting a globe, spending a spell point and equipping into a slot, colour-coded for owning client, server and reliable RPCs, each with 'In code' file and line references. It notes that the server repeats every client check, and that equipping into an occupied slot does not swap. Line numbers match the redo3 branch with the hardened Server_SpendSpellPoint.

> [!note] Linked from a thread
> This artifact is not in the project's Artifacts list. It was found through a link the thread posted.

## What it shows

- Structure: spell menu widget Blueprint -> UMySpellMenuWidgetController (SelectedAbility, bWaitingForEquippedRowPress), cached on UMyOverlayWidgetController; UDA_MyAbilityInfo supplies icon and cooldown tag per ability.
- Opening: GetSpellMenuWidgetController (MyBPFuncLib.cpp:42) creates the controller once and caches it; BindCallbacksToDependencies binds weak lambdas once (bCallbacksBound) to OnAbilityEquippedDelegate, OnSpellPointsChangedDelegate and OnAbilityStatusChangedDelegate; BroadcastInitialValues resets the selection and broadcasts ability info and SpellPoints.
- Selecting: SpellGlobeClicked cancels any pending equip; ShouldEnableButtons enables Spend for Eligible, Unlocked or Equipped with SpellPoints above 0, and Equip for Unlocked or Equipped.
- Spending: Server_SpendSpellPoint re-checks spell points and the spec on the server, levels up Unlocked/Equipped abilities or unlocks Eligible ones, calls AddToSpellPoints(-1) and Client_UpdateAbilityStatus; the point count also returns via OnRep_SpellPoints.
- Equipping is two clicks (Equip arms, a slot row sends Server_EquipAbility); the server reads the previous slot from its own spec, clears other abilities in the slot to Input.None and Unlocked, sends Client_EquipAbility for them first, then equips with status Equipped.
- Equipping into an occupied slot does not swap: the previous ability goes back to Unlocked; reliable RPCs to the same actor arrive in order so the client clears the slot before refilling it.
- Code references: MySpellMenuWidgetController.cpp:121 SpellGlobeClicked, :151 SpendSpellPointButtonPressed, :162 EquippedRowPressed; MyAbilitySystemComponent.cpp:214 Server_SpendSpellPoint, :253 Server_EquipAbility, :298 Client_EquipAbility.

## Commits it documents

- `aaaf14b` Make the server the source of truth for spell slot equipping · [[Redo3 Spell Menu Fixes]]
- `76625c2` Fix spell menu selection state around equipping · [[Redo3 Spell Menu Fixes]]
- `a63041a` Fix crashes from deleted Spell and Attribute menu controllers · [[Redo3 Spell Menu Fixes]]
- `45f0aa8` Validate spell point spending on the server · [[Redo3 Spell Menu Fixes]]

## Sections

- Structure
- Opening the menu
- Selecting a globe
- Spending a spell point
- Equipping into a slot

## Made by

- Thread: [[Redo3 Threads#Bring pinned artifacts into project|Bring pinned artifacts into project]]
- Explained on: [[Redo3 Spell Menu Fixes]]
- Group: [[Redo3 Artifacts - Menus]]

## Other artifacts from the same thread

- [[Redo3 Artifact - Hex Path and Bridges]]
- [[Redo3 Artifact - Hex Path Setup Steps]]
