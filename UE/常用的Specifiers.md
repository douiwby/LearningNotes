# 常用的Specifiers

## UPROPERTY

- 蓝图访问  
  BlueprintReadOnly/BlueprintReadWrite

- 属性编辑  
  EditAnywhere/EditDefaultsOnly/EditInstanceOnly
  VisibleAnywhere/VisibleDefaultsOnly/VisibleInstanceOnly
  EditInline
  Instanced（可配合EditInline）

- Multicast Delegates：  
  BlueprintAssignable/BlueprintCallable/BlueprintAuthorityOnly

- 网络  
  Replicated/NotReplicated

- 其他  
  AdvancedDisplay
  Category="TopCategory|SubCategory|..."

- Meta  
  ClampMin="N"/ClampMax="N"
  DisplayName="Property Name"
  DisplayThumbnail="true"
  EditCondition="BooleanPropertyName"
  InlineEditConditionToggle
  ExposeOnSpawn="true"
  AllowedClasses="Class1, Class2, .."

## UCLASS

- Abstract
- Blueprintable
- EditInlineNew

Meta:

- DisplayName="Blueprint Node Name"
- BlueprintSpawnableComponent

## UFUNCTION

- BlueprintCallable/BlueprintImplementableEvent/BlueprintNativeEvent
- BlueprintPure
- CallInEditor
- Category = "TopCategory|SubCategory|Etc"

Meta:

- DisplayName="Blueprint Node Name"
- AdvancedDisplay=N/AdvancedDisplay="Parameter1, Parameter2, .."
- DevelopmentOnly
- ExpandEnumAsExecs="Parameter"（enum需以引用传入，来将其识别为输出pin）
