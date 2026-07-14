
Grant GameplayAbilities after InitAbilityActorInfo

# Binding functions to actions whenever an action is being Pressed , Held or Released

![[ActivatingAbilities]]
![[MappingInputToAbilitiesDynamically]]

Data Asset that we create based on the class "UMyDataAsset" that we made derived from UDataAsset is an instance of the class UMyDataAsset
Hence >>
DataAssets based on a class UDataAsset are referenced by UDataAssetClass*

**Todo;**
*visualize click to run and auto running logic and revise Attribute menu logic also*
*and then gameplayTasks* 


![[Ability activation through input press and try activate in asc]]


AbilitySpec replicates auto only if ASC Rep true
	Have to MarkSpecDirty in every func after adding or removing tag from GASpec to let know ASC	
	Rep in class defaults reps UObjectu 
	
	
AbilityInputTagPressedFunc() the whole func runs together !!!, so if i call TryActivate and then InputPress invoke and wait for input press in Ability activation , it wont work .
	GASpecInputPress = localInputPress
	InvokeInputPressEvent= Replicate
	TryActivate = actual activation
	can be configured to rep on GASPecinputPress .