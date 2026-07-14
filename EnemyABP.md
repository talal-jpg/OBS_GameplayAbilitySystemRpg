make a ABP with the template as its skeleton
AnimInit TryGetPawnOwner > cast to EnemyChar> GetCharMovementComp> promote as variable
AnimUpdate GetCharMovementComp> getVelocity> getLengthxy > promote to var speed
make a generic blendSpacePlayer node in stateMachine >addstate > use speed as x comp of blendSpacePlayer

inherit respective ABPs with specific skeleton 
