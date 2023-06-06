# AsynLoading与AssetManager

## Asyn Loading

[Document](https://docs.unrealengine.com/5.2/en-US/asynchronous-asset-loading-in-unreal-engine/)

### FSoftObjectPaths和TSoftObjectPtr

`FSoftObjectPath`是一个简单的结构体，其中有一个字符串包含资源的完整名称。如果您在类中添加这个类型的属性，它就会像`UObject *`属性一样显示在编辑器中。  
`TSoftObjectPtr`基本上是包含了`FSoftObjectPath`的`TWeakObjectPtr`。如果被引用资源存在于内存中，则`TSoftObjectPtr.Get()`将返回这个资源。如果不存在，可以调用`ToSoftObjectPath()`来找出它引用的资源，再调用加载函数。

### StreamableManager和异步加载

`FStreamableManager` 是最简单的实现异步加载的方法。  
创建 FStreamableManager类后，将其作为单例在运行时使用，例如在`DefaultEngine.ini`使用`GameSingletonClassName`指定对象。  
然后，可以将`FSoftObjectPath`传递给SynchronousLoad和RequestAsyncLoad来进行加载。

## Asset Manager

[Document](https://docs.unrealengine.com/5.2/en-US/asset-management-in-unreal-engine/)

### Primary Assets

虚幻引擎的资产管理系统将所有资产分为两类：主资产和次资产。  
资产管理器通过主资产的Primary Asset ID对其进行操作。覆盖`UObject::GetPrimaryAssetId`并返回一个有效的`FPrimaryAssetId`来讲该类构成的资产指定为主资产。  
次资产不由资产管理器直接处理，但其被主资产引用或使用后引擎便会自动进行加载。  
默认只有`UWorld`为主资产，所有其他资产均为次资产。  
如果在创建主资产时不需要蓝图功能（如继承），可以使用Data Assets。以这种方式创建的资产是类的实例，而非类本身。要访问类，需使用`GetPrimaryAssetObject`在加载之后访问。

### Asset Manager 与 Streamable Managers

Asset Manager对象是一个单例，负责管理主资产的发现与加载。Streamable Manager被包含在其中，执行实际的异步对象加载，并使用Streamable Handles将对象保存在内存中，直到不再需要后进行卸载。与单例的资产管理器不同，引擎不同区域中有多个可流送管理器，以便用于不同用途。

### Asset Bundle

Asset Bundle是与主资产相关特定资产的列表。用`AssetBundles`的 meta tag来指定`TSoftObjectPtr`或`FStringAssetReference`成员即可创建资产包。

```C++
UPROPERTY(EditDefaultsOnly, BlueprintReadOnly, Category = Display, AssetRegistrySearchable, meta = (AssetBundles = "TestBundle"))
TSoftObjectPtr<UStaticMesh> MeshPtr;
```

也可以使用`AddDynamicAsset`在运行时进行注册。  
在Project Settings的Asset Manager里可以手动注册主资产。
在代码中可以通过覆写`StartInitialLoading`函数并从该处调用 `ScanPathsForPrimaryAssets`来注册。  
使用`LoadPrimaryAssets`（对应的`UnloadPrimaryAssets`）来加载主资产。传入AssetBundle即可加载其指定的所有主资产。
