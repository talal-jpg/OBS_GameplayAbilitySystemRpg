https://www.youtube.com/watch?v=HTPbHeqstT0
![[Pasted image 20260605164623.png]]
replicated control rig physics
https://forums.unrealengine.com/t/simulating-physical-hit-reactions-using-control-rig/2055170/2



//AIMCONTROLS TESTING

//Test1 **Test1**
~~not working~~
~~right click to lock target , right click to add control rotation~~


//Test2  **Test2**
// this was already similar to anthem
space jump , space flight mode
hold shift boost , if forward while holding shiftand side then roll.

anthem style
disable aim when in flight /controll flight with mouse
as soon as shoot , become hover
~~hover doesnt use mana? only thruster does~~
~~proper  3rd person shooter with aim centered in middle~~

~~OnGround; Space to jump , Space again to Hover~~
~~aim in middle screen, side strafe~~  

~~hover;Shift to thrust , ability to land , based on distance , calculate anim and damage / knockback  (not in hover mode? only in thrust?)~~
~~hover aim in middle, side strafe like flight~~ 

~~thruster; shoot to get back to hover , direct land with ability , calculate time while thrust , and add to land ability if done while thrusting~~
~~thruster , get dir orient cam to dir  , thrust side and orient cam to move will keep going to side and orienting cam again and again? or can do thruster with side key and if keep pressing side while thrusting add tilt to side also , and if pressed side and then after camera tilt switched to forward~~ 

land ability only able to execute at a certain height?

//AIMCONTROLS TESTING

control rig driven , skeletal mesh based UI??
ctrl b for highlight
**# STEAMPUNK-PARTS-MACHINES-GEAR-INDUSTRY**
steam punk panels , industry parts

~~//control rig for prcedural walk , strafe ,turning , delayed turning from top to bottom~~ 
~~// aiming / arms controlled by anim? , the closer to ground the more procedural in rig?~~

~~Damage mechanics~~
~~hitscan delayed , upgrade with attack speed~~

~~GameInstance subsystem , each bullet shot is fed in the subsys with a dir , a location and a time and gameplayeffect spec?, at that engine time get the dir and loc do line trace and apply ge spec~~
~~visual representation for bullet by niagara~~


**General gameplay rules**
death only respawn , no loss other than giv exp to opp
no refill hp mana, so that can die even if higher in lvl
hexagons pillars , that break randomly and remove collision
on bullet hit, hit react ability
on big hit , control rig physics

~~static mesh hexagon pillars , replaced by static mesh with VAT mat and propagate falling down by distance in WS~~

figure flying Mechanics 
setting gravity scale 0 while acitve , setting to 1 when released

**FootLocking**
~~either foot anim notify trace thr set either footLocked pos to that if no notify trace actor pos - offset z~~ 

https://www.youtube.com/watch?v=eNVKiWPropQ

~~pillarActor~~
	~~staticMesh~~
		~~visibility=1~~
	~~skeletalMesh~~
		~~visibility=0~~
		~~AnimPlay=0~~
	~~activateAnimFunc~~
		~~staticM vis=0 , SkelM Vis=1 AnimPlay=1, delay SkelM Vis=0?~~
		
		
~~MyHexPlatform~~
	~~RemovingHex from TMap imediately after activating chain , remove in the Lambda itself after getting the item from TMap~~
		

~~GA_shoot~~
	~~Damage, KnockBack~~
	

GA_Beam
	Charge Damage, KnockBack


Flying
Multiplayer
MultiplayerChat

IDEA! add third bar heatup and when heatup starts losing health

https://www.youtube.com/watch?v=G9fe_ty1q_o

~~**AnimsToRetarget**~~
~~falling idle~~ 
~~falling to roll~~
~~Jump~~
~~FlyingThruster~~

~~**Test3**~~
~~UENUM ECharState(){~~
~~Standing ,~~
~~Hover,~~
~~Falling,~~
~~Thruster.~~
~~}~~

~~MyChar~~
	~~void SetCharStateFunc~~
		~~On Tick~~
		~~SphereTrace-Z~~ 
		~~Vel in Z  Falling vs Hover~~
		

~~track hover state in GA_launch , event driven timeline turn gavityscale~~

~~thurster while on ground , thruster when jump are two different things~~
~~and bInAir~~

~~I want to implement , a flying mode with checking each tick if it is in air , if not in air jump else use launch to add velocity in z + movement dir  then if shift is pressed activate boost and speed up in a direction ,and as soon as close to a surface change mode to walking and if more than a certain velocity play a root motion animation in a direction if direction input else in front~~ 


![[CharMovementComp]]

~~CharPlayer~~
	~~LerpFeetRotToRot~~
		~~replace ControlRot with GetActorRot(ActorRot is the default replicated rot for simulated proxy: verify!)~~
	~~MeshRotToBeWorldOriented(not follow Actor or capsuleRot(Capsule still uses Controller desired rotation but char mesh doesnt))~~
		~~Click on your Mesh component in the Components panel.In the Details panel, look for Transform settings.Click the small drop-down arrow next to Rotation and change it from Relative to World~~


PlayerController
	TargetLocation
		For Simulated proxy set TargetLocation in Char class by Getting actor rot and line tracing to infinity



