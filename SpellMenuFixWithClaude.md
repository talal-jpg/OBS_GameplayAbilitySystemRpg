┌─────────────────────────────┬─────────────────────────────────────────────────────────────┐
│           Commit            │                      What it contains                       │
├─────────────────────────────┼─────────────────────────────────────────────────────────────┤
│ aaaf14b Make the server the │ The ability system component changes, plus the spell menu's │
│  source of truth for spell  │  call updated to the new Server_EquipAbility(AbilityTag,    │
│ slot equipping              │ NewInputTag)                                                │
├─────────────────────────────┼─────────────────────────────────────────────────────────────┤
│ 76625c2 Fix spell menu      │ Spell menu selection and button fixes, and the Input.None   │
│ selection state around      │ check in the overlay controller                             │
│ equipping                   │                                                             │
├─────────────────────────────┼─────────────────────────────────────────────────────────────┤
│ a63041a Fix crashes from    │ Caching both menu controllers, weak lambdas, the bind-once  │
│ deleted Spell and Attribute │ flag, and resetting the selection when the menu opens       │
│  menu controllers           │                                                             │
└─────────────────────────────┴─────────────────────────────────────────────────────────────┘



[[Aura Spell Menu Flow]]