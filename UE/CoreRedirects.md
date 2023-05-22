# Core Redirects

开发过程中，在某些情况下需要重命名现有类、属性、函数名称或相似代码成员。 然而，如果存在大量受这些更改影响的资源，仅重命名代码成员然后重新编译项目会导致相当数量的数据丢失，因为虚幻引擎无法再识别现有资源。 为解决此问题，引擎使用 Core Redirects。  

[Unreal Document](https://docs.unrealengine.com/5.2/en-US/core-redirects-in-unreal-engine/)  

## 使用方法

在做出相应的更改后，在`DefaultEngine.ini`文件里添加对应的`[CoreRedirects]`。  
重新打开引擎后，需要**重新保存**相应的文件，确认无误后即可删去`[CoreRedirects]`。

## 更改变量

```ini
[CoreRedirects]
+PropertyRedirects=(OldName="MyActor.OldFloatProperty",NewName="NewFloatProperty")
+PropertyRedirects=(OldName="MyOldActor.OldIntProperty",NewName="MyNewActor.NewIntProperty")
```

## 更改类

```ini
[CoreRedirects]
; Game module, native class rename
+ClassRedirects=(OldName="/Script/MyModule.MyOldClass",NewName="/Script/MyModule.MyNewClass")
; Blueprint class redirect to native class
+ClassRedirects=(OldName="BP_Item_C", NewName="/Script/ActionRPG.RPGItem", OverrideClassName="/Script/CoreUObject.Class")
```
