**Menu**
1.menu system , all options available in FFW , list them , and then implement.
1.follow the existing bind widget , and widget switcher convention.

**Environment**
export pilar 2 mesh from Hou.
configure bottom platforms to use , pillar2 mesh
scatter tree , house , then clouds / atmospherics.

**GameP**
Game mode configure to respawn , on the platform just ahead of the platform , the leading player is on , spawn respawn fx .

the rules will be in attrib set post ge exec etc. will have to deal damage will death by falling too.
game state will hold state .
game instance will store matches won (data that needs to be carried out of the current match)

Gameplay:
Each shot fired , update only if fatal , elim data. from FPSTut
every Leader update , set leader on GameState. 
go to FPSTut github , it sends shot fired data on each shot?, I want to replicate the elim data only if the player was eliminated .
list out the data being sent and let me choose .

OnFatal ,Xp give , Level update, Ability unlock.

GameplayEffects for Gunshoot , Beam .

**Grapple**
Grapple NS
CR_ add grapple arm move.
right hand up and shoot grapling hook , hang by right hand and shoot by left

**Visual:** 
Show Leading Player/s.
Show K/D counter.

Export and setup Thruster mesh
Export and setup Air Explo mesh

make a NS , and spawn the thruster mesh make it bend in the direction of moving in such a way that is seems the character is being pushed by it


**beam**
make billowy smoke mesh and thunder NS spawn in the dir of N of mesh + Dir of beam normed

**pathwayOnPlatforms**
make a  spline out of the point locations , subdivide according to bending and place/copy created points . (points create in patterns as pathway pattern)

**bottomBridge**
stamp same points on top of bottom bridge
fix end tiles stretching on bottomBr
