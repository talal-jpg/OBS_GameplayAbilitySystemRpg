---

kanban-plugin: board

---

## TBD

- [ ] hosting char starts with default move mode on client because enters and sets move mode before client game starts
- [ ] Direct join (joining via a known IP or Steam friend) when appropriate.
- [ ] [[AbilityInfo]]
- [ ] [[Ability aoe melee attack with two handed axe anim]]
- [ ] And ground melee attack??
- [ ] Rockets ability with Projectile Component??


## backLog

- [ ] MultiCastRollEvent checks for velocity and only roll if greater thn ,even when called might not execute
- [ ] WhileJumpPressed Charge jump and Jump based on that, mesh space additive anim for charging jump?
- [ ] [[Hud]]
- [ ] [[AttributesAndAbilities]]
- [ ] ablities Gameplay effects
- [ ] Main Menu and Multiplayer menu / lobby thinking todo
- [ ] While Equiping when the status of the Spell has not yet been updated/ replicated and I equip again , the crash happens??
- [ ] //TODO hit trace and target location for arms and loc while flyboosting should be 2 different vars
- [ ] Damage Numbers via GameExecCalc , adding via Niagara DC
- [ ] 
- [ ] GA_Boost , Cd only comit when in air


## InProgress

- [ ] svo pathfinding tut c+++
- [ ] UdemyFirstPerson Project explore
- [ ] Find Anim For Boost when in air and beam when in air
- [ ] Die implementation , TODO Thinking ,with Tag?
- [ ] [[while BoostFlying Control rig]] ^2jxoq4
- [ ] bothControllerAndCharDoingTraces
- [ ] customHitTraceChannel
- [ ] HexPlatform Spawn Only on Server , events only on server with multicast
- [ ] gunShootBulletNs
- [ ] [[BeamAbility]]
- [ ] Respawn func
- [ ] level up implementation
- [ ] need to understand cr physics solver better


## Completed

- [ ] boostToRollSetControlRotAndActorRot
- [ ] [[AbilityActivationFromServerToClient]]
- [ ] [[Set SimProxyTargetLoc In CharClass]]
- [ ] Boost flying Traversal
- [ ] [[boost niagara System, make own]]
- [ ] only move forward when boosting and flying
- [ ] server char not showing move anims on client
- [ ] input only reps from client to server so if any thing needs to be done on all clients on input release then ,can play montage with AT will handle replication auto
- [ ] While dropping in abp calling multicast_roll might cause bug if pressed shift while falling? does cause
- [ ] ControlRigHitReacts
- [ ] [[Damage delay only when done to a client]]
- [ ] GC_Beam Only not appearing in packaged build only
- [ ] [[Improve NS_boost]]
- [ ] [[Broadcast AbilityInfo bug fixes]]
- [ ] ServerSpendSpellPoints both ability can also be the same , if euiped in both slots , need fix
- [ ] [[VM making]]
- [ ] [[MatchType set to AuraRedo3 , will filter on find with this and show that session]]
- [ ] While flying if boosting orient to rotation in pitch as well??? , not working as well
- [ ] GunRotateWhileShooting


## ScrappedIdeas

- [ ] [[twoSteamGameInstances]]




%% kanban:settings
```
{"kanban-plugin":"board","list-collapse":[false,false,false,false,false]}
```
%%