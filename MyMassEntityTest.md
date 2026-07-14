# Prompts


Take a look at the project and make a similar system
Make MyMechWorldSubsystem
Make the same folder structure and add Mech_FolderName for Myclasses
use static library instead of utilities and namespace 
use GameMode to hold global data instead of DeveloperSettings

I want a crowd of entities 
those entities should
1. avoid eachother
2. move to a target location , target location should be set by a shared fragment, first should move towards one of the player characters in level if too many already surrounding the character search for the other if too many there too then move to the other shared target location specified
3. when close enough to hit target character switch to attack state 
4. the representation should be done by actors for close actors and done by instanced static meshes for far away actors.
5. they should be able to recieve hits from actor using simple collision capsule give me a function for that
 
6. the heath should be calculated by taking into consideration armor fragment for each each entity using GameplayAbilitySystem , make a damage meta attribute , the attributes should be held by Mass and the GAS should only be used for calculating and logic 
### GAS is for:

- Combat math
    
- Damage resolution
    
- Buffs / debuffs
    
- Tags like Stunned, Shielded, Dead
    

### Mass is for:

- Scale
    
- Movement
    
- Avoidance
    
- LOD
    
- Representation
    
- Collision queries

GAS logic, Mass storage

use default fragments and processors provided by mass plugins where can , but copy them and relevant classes , structs needed and name them originalName_Copy and put in relevant folders

Add AoE damage with spatial queries, Add headshots / bone hits Integrate SmartObjects as attack targets

Health attribute , armor attribute , attack damage, attack speed , movement speed(shared for all), stunned tag, Dead tag 

the spawner should spawn entities wave after wave and the amount should be controllable

finally make a MyMechEntitySystemReadme.md with architechture structural dependencies and flow of data with ascii diagrams for the whole system
# asks
how is FRogueDebugMove being used and where

# PromptsMade
create a readme file , Analyze the project , bulid an architectural dependency map of MassEntity , frangments and processors, also create a flow diagram that shows which class spawns entities and which processors process them in order and which fragments do they read or write to  , generate an ASCII diagram, and add it to the readme file , 


[[README]]
[[README2]]
[[readme3]]


[[SplineActorFromDeveloperSettings]]
[[RogueAIDebugModule]]