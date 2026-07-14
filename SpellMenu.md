![[Excalidraw/SpellMenu]]

Server Spend Spell points 
ServerUpgradeAttrs
ServerEquipAbility
on ASC as server RPCs called on clients executed on server

High-Level Flow
Player opens Spell Menu
        |
        v
BroadcastInitialValues()
        |
        v
BindCallbacksToDependencies()
        |
        v
User clicks spell icon
        |
        v
SpellGlobeSelected()
        |
        v
UI shows:
    - description
    - spend point button
    - equip button

Spend point ->
    SpendPointButtonPressed()

Equip ability ->
    EquipButtonPressed()
        |
        v
Wait for row click
        |
        v
EquipAbility()

Gameplay updates ->
    OnAbilityEquipped()
    OnAbilityStatusChanged()
    OnSpellPointsChanged()


------------------

                         +----------------------------------+
                         | UAuraSpellMenuWidgetController   |
                         +----------------------------------+

      Initialization
      ------------------------------------------------------

             +---------------------------+
             | BroadcastInitialValues()  |
             +---------------------------+
                        |
                        | broadcasts
                        v
        +---------------------------------------------+
        | SpellPointsChangedDelegate                  |
        | AbilityInfoDelegate                         |
        | AbilityStatusChangedDelegate                |
        +---------------------------------------------+


      Delegate Binding
      ------------------------------------------------------

             +-----------------------------------+
             | BindCallbacksToDependencies()     |
             +-----------------------------------+
                        |
                        |
                        v
     +------------------------------------------------------+
     | AbilitySystemComponent Delegates                     |
     |                                                      |
     |  OnAbilityStatusChanged                              |
     |  OnAbilityEquipped                                   |
     |  OnSpellPointsChanged                                |
     +------------------------------------------------------+
                        |
                        v
          Updates UI / broadcasts to widgets



      Ability Selection
      ------------------------------------------------------

           +-----------------------------+
           | SpellGlobeSelected(Tag)    |
           +-----------------------------+
                        |
                        v
         +---------------------------------------+
         | Store SelectedAbility                 |
         | Store CurrentAbilityStatus            |
         | Store AbilityType                     |
         +---------------------------------------+
                        |
                        v
           Broadcast SpellGlobeSelectedDelegate



           +-------------------------------+
           | SpellGlobeDeselected()        |
           +-------------------------------+
                        |
                        v
               Clear selected ability
                        |
                        v
              Broadcast deselection



      Spending Spell Points
      ------------------------------------------------------

          +--------------------------------+
          | SpendPointButtonPressed()      |
          +--------------------------------+
                        |
                        v
       +------------------------------------------+
       | AbilitySystemComponent->ServerSpendPoint |
       +------------------------------------------+
                        |
                        v
          Ability status gets updated
                        |
                        v
           OnAbilityStatusChanged callback



      Equipping Abilities
      ------------------------------------------------------

          +--------------------------------+
          | EquipButtonPressed()           |
          +--------------------------------+
                        |
                        v
      +---------------------------------------------+
      | Wait for player to click equip slot         |
      +---------------------------------------------+
                        |
                        v
          +------------------------------+
          | EquipAbility(RowTag)         |
          +------------------------------+
                        |
                        v
     +------------------------------------------------+
     | AbilitySystemComponent->ServerEquipAbility     |
     +------------------------------------------------+
                        |
                        v
                Server updates ability
                        |
                        v
               OnAbilityEquipped callback



      Gameplay Updates
      ------------------------------------------------------

          +----------------------------------+
          | OnAbilityStatusChanged(...)      |
          +----------------------------------+
                        |
                        v
           Broadcast updated ability status
                        |
                        v
                UI updates spell tree



          +----------------------------------+
          | OnAbilityEquipped(...)           |
          +----------------------------------+
                        |
                        v
         Update equipped row (bottom bar)
                        |
                        v
             Broadcast EquippedAbility



          +----------------------------------+
          | OnSpellPointsChanged(int32)      |
          +----------------------------------+
                        |
                        v
              Update spell points UI 


// TODO Get Ability In the CurrentInputTag Ability in the slot and Remove InputTag from it then SpendSpellPoint