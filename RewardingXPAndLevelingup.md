![[Excalidraw/RewardingXPAndLevelingUp]]

![[HandleIncomingXP]]

void UMyOverlayWidgetController::OnXPChangedCallback(int32 NewXP)
{
	
	
	UDA_LevelUpInfo* LevelUpInfo = GetMyPlayerState()->LevelUpInfo;
	checkf(LevelUpInfo, TEXT("Unabled to find LevelUpInfo. Please fill out AuraPlayerState Blueprint"));
	
	const int32 Level = LevelUpInfo->FindLevelForXP(NewXP);
	const int32 MaxLevel = LevelUpInfo->LevelUpInfos.Num();
	
	if (Level <= MaxLevel && Level > 0)
	{
		const int32 LevelUpRequirement = LevelUpInfo->LevelUpInfos[Level].LevelUpXPRequirement;
		const int32 PreviousLevelUpRequirement = LevelUpInfo->LevelUpInfos[Level - 1].LevelUpXPRequirement;
	
		const int32 DeltaLevelRequirement = LevelUpRequirement - PreviousLevelUpRequirement;
		const int32 XPForThisLevel = NewXP - PreviousLevelUpRequirement;
	
		const float XPBarPercent = static_cast<float>(XPForThisLevel) / static_cast<float>(DeltaLevelRequirement);
	
		OnXPPercentChangedDelegate.Broadcast(XPBarPercent);
		UKismetSystemLibrary::PrintString(GetWorld(),FString::Printf(TEXT("Level Up: %d/%d"),Level,MaxLevel));
	}
}





#include "AbilitySystem/Data/DA_LevelUpInfo.h"

int32 UDA_LevelUpInfo::FindLevelForXP(int32 InXp)
{
	int32 Level=1;
	bool bSearching=true;
	while (bSearching)
	{
		if (LevelUpInfos.Num()-1<=Level) return Level;
		
		if (InXp>=LevelUpInfos[Level].LevelUpXPRequirement)
		{
			Level++;
		}
		else
		{
			bSearching=false;
		}
	}
	return Level;
}

	TODO Getting IncomingXP From WaitingGA ,UpdateXP on PlayerState And LevelUp PlayerIF func