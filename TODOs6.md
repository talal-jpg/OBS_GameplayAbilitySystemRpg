1.rightnow, the swarm is not being spawned even when the spawn swarm actor is placed in the level, my guess is because the nav volume is not present.is that the case and if nav mesh is necessary then(2. and 3. otherwise skip to 4.)
2. how the level works is, there is a pathspawner actor that spawns a spline segment when the leader player gets within a certain distance, and that spline segement is then used by a pcg to build the procedural environment.
3. figure out the best way (computationally) to spawn and destroy nav meshes with the segments and the nav meshes cover the whole procedural environment with some bounds padding and the swarm spawning at the corners of the nav meshes.
4. find the best way to link damage attribute of player character to the sentinel damage GE and link it.
5. the swarm spawns at the corners of the bounds of each segment either way.
6. the mass avoidance is strictly 2d? the one we use , is it modified to work in 3d?

7. how the level works is, there is a pathspawner actor that spawns a spline segment when the leader player gets within a certain distance, and that spline segement is then used by a pcg to build the procedural environment.
8. spawn the waves at the corners of the assumed bounds of each segments when the segment is spawned.
9. find the best way to link damage attribute of player character to the sentinel damage GE and link it.
10. the level is infinite, right now the attributes of each wave can only be configured till how many entries I make for each wave, make the attributes incremental with a multiplier for each wave (can give a curve that scales with the wave number with easier waves in begining and stronger towards end)
11. make the waves use and apply armor , speed boost ,attack damage boost with cooldown randomly and with visual feedback.

12. each time a player steps on to a platform , all the platforms that are at a lesser distance start to destroy and leader player and ranking is updated .
13. make a match type where there will be two teams either 2 v 2 or 1 v 1 , depending on the players in the lobby, coordinate with the thread that configured respawning and death , show leader boards for kill deaths and leader board for leading player in distance on the platforms.
14. 



15. can our custom avoidance use  FMassNavigationEdgesFragment, if so it can be updated each time a new segment is spawned and new wave is spawned and used to avoid platforms? the bounds of platforms are calculated in the pcg graph that spawns all the platforms. we can also calculate the bounds for each platform using the size and transforms of instances and the default size of the geometry being used as instance geo.

for me :
the attribs per wave figure out
the more level the more tentacles?

 is custom avoidance ( spatial 3d  hash grid  check in c++) 
