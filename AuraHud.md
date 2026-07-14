holds two main functions 
member var WidgetController

```cpp
GetOverlayWidgetController(FWidgetControllerParams WCParams){
	if(OverlayWidgetController == nullptr){
	OverlayWidgetController= NewObject<UOverlayWidgetController>(this,OverlayWidgetControllerClass);
	OverlayWidgetController.SetWidgetControllerParams(WCParams);
	
	}
	return OverlayWidgetController;
}

InitOverlay(APlayerState* PS,APlayerController* PC,UAbilitySystemComponent* ASC, UAttributeSet* AS){

	UUserWidget* Widget=CreateWidget<UUserWidget>(GetWorld(),OverlayWidgetClass);
	
	OverlayWidget= Cast<OverlayUserWidget>(Widget);

 FWidgetControllerParams WCParams= FWidgetControllerParams(PS,PC,ASC,AS);

WidgetController= GetOverlayWidgetController(WCParams);

OverlayWidget.SetWidgetController(WidgetController);

Widget.AddToViewPort
//or OverlayWidget.AddToViewPort;
}
```

[[AuraHud.h]]
[[AuraHud.cpp]]
![[Drawing 2025-04-26 19.03.31.excalidraw]]
