# 平衡蓝图与C++

[Document](https://docs.unrealengine.com/4.27/zh-CN/Resources/SampleGames/ARPG/BalancingBlueprintAndCPP/)

## 逻辑与数据

在考虑使用蓝图或者C++时，除了逻辑，也要考虑数据。  
数据除了C++类，也可以用以下几种方式：  

- 配置文件：如ini, Console Variables
- 蓝图类
- Data Assets：用于无法实例化且不需要数据继承的对象
- Data Tables/Curve Tables

## C++与蓝图各自的优势

C++:

- 运行时性能
- 封装控制：可以更精确地控制要公开的内容
- 更广泛的访问：使用C++定义的函数和变量（并且正确公开），可以从所有其他系统访问，非常适合**在不同系统之间传递信息**。故C++也可以更充分地利用引擎功能。
- 更强的数据控制：在加载和保存数据时，C++可以使用更具体的功能。这让你能够以高度自定义的方式应对版本变更和**序列化**。
- 可以严格控制网络复制
- 数学运算
- 方便Diff/Merge

蓝图:

- 快速创建
- 快速迭代：所有"可调整"值都应尽可能存储在资源中
- 流程更直观：尤其是延迟和异步运行节点
- 更简单的数据使用：蓝图类存储的数据比C++类更简单安全，适用于混合数据和逻辑的类

## 性能问题

执行单个节点的代码比一行C++代码要慢，但一旦进入节点内，则速度与C++一致。故蓝图里的多次循环或者是总共几百个节点的宏，都应该考虑用C++实现。  
尤其要注意蓝图Tick里的节点，应尽量使用timer或delegate来避免使用蓝图Tick。  
从Profiler里可以观察蓝图类的占用时间，若self花了大部分时间，则表面其为蓝图消耗所导致的性能下降。

## 其他注意事项

- 避免到性能成本较高的蓝图Cast：蓝图转换（或将蓝图声明为变量类型或函数参数）会在添加依赖关系。每次加载时，即使转换会失败，也必须加载依赖的所有资源。应尽量定义重要函数和变量的native基类或最小蓝图基类，而将性能消耗较大的蓝图作为子类。
- 避免循环引用蓝图： 过多的循环蓝图引用会使编辑器加载和编译时间变长（也可能会导致Nativizing Blueprints功能失效），与上面的转换类似。
- 避免从C++类引用资源：这种方式引用的资源将在项目启动时加载。应创建一些"Game Data"资源或蓝图类型，并使用Asset Manager或配置文件加载它们。
- 避免使用字符串引用资源：应在C++类中使用 FSoftObjectPath 或 TSoftObjectPtr 类型，从ini或蓝图类设置这些类型，然后根据需要或通过异步加载进行加载。
- 注意用户结构体和枚举值：如果不止一两个蓝图使用某些项，则应在Native C++中实现。
- 考虑异步加载：多使用[Soft References](https://docs.unrealengine.com/5.2/en-US/referencing-assets-in-unreal-engine/)或PrimaryAssetIds，用于代替Hard References。使用AssetManager和StreamableManager来[异步加载](AsynLoading与AssetManager.md)资源。
