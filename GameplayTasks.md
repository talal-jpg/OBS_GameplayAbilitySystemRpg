[[DifferenceBetweenPointersAndReferences]]

A function that returns a newAbilityTask<UAT_foo>(OwningAbility,TEXT("Name"));

implemented AT_GetLocationUnderCursor by sending data from clients and receiving it on the server,on the client using ServerSetReplicatedTargetData to set the targetData to be replicated to the server , then on server binding callback function to the delegate AbilityTargetDataSetDelegate that is called when target data is set on client and callback function broadcast the delegate, and callReplicatedTargetDataDelegatesIfSet on server to make sure if the data from client already reached before the delegate was bound returns a bool if false SetWaitingOnRemotePlayerData


![[AbilityTask]]