**Menu**
1.menu system , all options available in FFW , list them , and then implement.
1.follow the existing bind widget , and widget switcher convention.

~~**Environment**~~
~~export pilar 2 mesh from Hou.~~
~~configure bottom platforms to use , pillar2 mesh~~
~~scatter tree , house , then clouds / atmospherics.~~

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

~~**pathwayOnPlatforms**~~
~~make a  spline out of the point locations , subdivide according to bending and place/copy created points . (points create in patterns as pathway pattern)~~

~~PathWays converge to a square shape in middle , shape to pentagon or hexagon depending on number of pathways.~~
~~![[Pasted image 20261027162703.png]]~~

~~**bottomBridge**~~
~~stamp same points on top of bottom bridge~~
~~fix end tiles stretching on bottomBr~~
~~BottomBr GCs of the bottom part , make way less fracture pieces.~~

~~for the instances that are further away than threshold, either a , Vertex animated texture with time at which it is spawned at or a instanced static mesh with all frames and frame number in vertex color and animate with time.~~
~~a sim of props falling with frame number in vertex color (can do in UE also?)****~~

~~also add a ISM for instances further than 50m , bottom and top and custom prop on ISM that sends time at which it is activated and speed , Ill add a Static Mesh with all the frames and a frame number in vertex color , to play a baked anim for further away instances . and can you export a baked simulation for the tile prop in fbx or usd that I can import in Houdini and make a similar static mesh with all the frames + frame number ?~~

cloud sprites material multiply by tex sampled by uv  , offset by particle index

grapple disconnect thresh inc , range dec

reduce noise eating threshold on smaller platforms , reduce by grid size.
