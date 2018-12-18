---
title: 服务容器
---

# 服务容器

Container 服务容器是一个用于管理`类的依赖`和`执行依赖注入`的强大工具。其实质是通过反射来对`构造函数`或者标记为`[Inject]`特性的属性选择器进行注入。

## 简介

几乎所有的服务绑定都是在服务提供者中完成。如果一个`服务`没有基于任何接口那么就没有必要将其绑定到容器(除非他是组件内部使用的类)。

容器并不需要被告知如何构建对象，因为它会使用反射技术自动解析出具体的对象实例。

在服务提供者中我们可以使用`Bind()`或者`Singleton()`方法来注册一个`服务`的绑定。

在CatLib核心库中[应用程序](application.html)继承自服务容器，所以在任何位置，您可以通过`App`全局变量访问容器。

## 构建服务

您可以通过`Make()`方法来构建服务。如果通过容器去生成一个没有被绑定的服务，那么容器会根据策略尝试解决需要被生成的服务，如果尝试解决失败，那么将会抛出一个`UnresolvableException`异常。

```csharp
var fileSystem = App.Make<IFileSystem>();
```

#### 构建服务时传入自定义参数

CatLib支持在构建一个服务时为其提供自定义的参数，您只需要这样做：

```csharp
var fileSystem = App.Make<IFileSystem>("params-1", "params-2");
```

## 绑定服务

要使用服务容器的强大特性您首先需要将服务绑定到服务容器，如果您不进行绑定,那么您将没有任何服务可用，这些绑定一般都是在[服务提供者](service-provider.html)中完成的。

绑定服务分为以下两种类型：

- `实例绑定` 每次调用都会产生一个全新实例。
- `单例绑定` 将类或者接口绑定到服务容器，一旦单例绑定被构建，每次调用总是返回相同的实例。

#### 实例绑定

通过`Bind()`方法允许将服务以实例绑定的方式进行绑定。

``` csharp
App.Bind<IFileSystem, FileSystem>();
```

我们通过上述代码将`IFileSystem`接口绑定到`FileSystem`实现。这样您就可以使用使用接口来构建服务了：

``` csharp
var fileSystem1 = App.Make<IFileSystem>();
var fileSystem2 = App.Make<IFileSystem>();
// fileSystem1 != fileSystem2
```

#### 单例绑定

服务容器通过`Singleton`方法来对服务进行单例绑定，一旦单例绑定被构建，每次调用总是返回相同的实例。

``` csharp
App.Singleton<IFileSystem, FileSystem>();
```

```csharp
var fileSystem1 = App.Make<IFileSystem>();
var fileSystem2 = App.Make<IFileSystem>();
// fileSystem1 == fileSystem2
```

> 通过[门面](facade.html)和服务容器的结合，颠覆了传统的单例实现方式，以一种更新，更易于维护的方式来实现传统的单例模式。

## 绑定服务-如果不存在

您可以使用`BindIf`或`SingletonIf`进行条件判断绑定，如果绑定的服务不存在，那么则执行绑定，反之则不进行任何操作。

```csharp
App.BindIf<IFileSystem, FileSystem>();
```

```csharp
App.SingletonIf<IFileSystem, FileSystem>();
```

## 索引器

服务容器支持通过索引器来绑定一些简单值，通过索引器进行的绑定总是以[Bind-实例绑定](#实例绑定)模式进行的。

#### 通过索引器绑定

```csharp
container["hello"] = "world";
```

#### 通过索引器获取

```csharp
var world = container["hello"];
```

## 构造函数注入

在您简单的`Make()`操作中，服务容器已经对绑定的服务进行了一系列复杂操作（服务关系转换，服务构建，服务推测，上下文关系处理，依赖注入，服务修饰，单例化...），这一切对于开发者的您来说完全是透明的无感知的。

服务容器能够自动识别您构造函数中的参数，并为其注入合适的服务实例。注入方案请参考：[依赖注入规则](#依赖注入规则)

``` csharp
public class CustomizeService
{
    public Service(IFileSystem fileSystem)
    {
        // fileSystem 会被自动注入实现

        // 如果您没有在服务容器中绑定过 IFileSystem 服务
        // 那么会抛出一个UnresolvableException异常
    }
}
```

> 注意,只有构造函数为`public`才能够被注入
> 简而言之，您只需要向服务容器绑定好您开发的服务，服务容器将会在您生成服务时自动处理好您的服务依赖关系。

## 属性选择器注入

服务容器采用`特性标记(Attribute)`的方法来识别哪些`属性选择器`是允许被注入的。

任何可以被注入的属性选择器必须满足以下规则：

- 属性选择器必须标记`[Inject]`特性
- 属性选择器`set;`的访问级别必须为`public`。

``` csharp
public class CustomizeService
{
    [Inject]
    public IFileSystem FileSystem { get; set; }
}
```

> 注意，您不能在`构造函数`中访问(调用)被标记为注入的`属性选择器`的实例，因为属性选择器的注入流程会在构造函数之后进行，如果您在构造函数中访问了标记为注入的属性选择器，这将会导致一个`NullReferenceException`异常。

## 服务别名

`服务别名`是服务容器非常重要的一项功能，它可以提供：将接口绑定指定实现的能力。

``` csharp
App.Singleton<IFileSystem, FileSystem>();
```

或将多个接口指向到同一服务

``` csharp
App.Singleton<IFileSystem, FileSystem>().Alias<IDisk>();
```

通过`App.Make<IFileSystem>()`您可以获得`FileSystem`实现，由于您面向接口编程，无需关注底层实现，从而服务可以实现无感知替换。

## 单例服务释放

容器允许您使用`Release`方法为已经生成的`单例服务`进行释放。释放后会触发[服务释放事件](#服务释放事件)

```csharp
App.Release<IFileSystem>();
```

#### IDisposable 释放接口

对于已经生成的单例服务进行释放，会检查服务实现是否实现了`IDisposable`接口。如果实现了该接口将会自动调用。

> 如果服务未能在容器中找到，那么会`Release`函数会返回`false`

## 解除服务绑定

您可以通过`UnBind()`来对已经绑定的服务进行解除绑定。如果被绑定的类是单例化的且已经被构建，那么解除绑定时会自动释放已经被生成的单例，同时触发[服务释放事件](#服务释放事件)。

``` csharp
var binder = App.Singleton<FileSystem>();
binder.UnBind();
```

## 服务构建事件

服务容器在每一次新构建对象时都会触发服务构建的事件，可以使用`OnResolving()`方法监听该事件。

该事件可以对构建的服务进行修饰处理或参数配置，并允许你在服务对象被传递给开发者之前，为其设置额外属性。

构建事件分为2种类型，`局部构建事件`将会优于`全局构建事件`被调用，构建事件的特性如下：

- 基于指定服务的构建事件，只有当指定服务被构建时才会被触发。
- 基于全局的构建事件，在任何服务被构建时都会触发。
- 单例服务的构建事件只会在第一次构建时触发一次。
- 使用`Instance()`直接设定服务实例会触发服务构建事件。

#### 基于服务的构建事件

``` csharp
App.Singleton<IFileSystem, FileSystem>()
   .OnResolving<FileSystem>((fileSystem) =>
   {
       //todo: 对于FileSystem的实例进行修饰
   });
```

#### 基于全局的构建事件

- 对全部服务进行修饰

``` csharp
App.OnResolving((instance) =>
{
    //todo: 对于所有被构建的服务进行修饰
});
```

- 筛选指定的服务进行修饰

```csharp
App.OnResolving<IFileSystem>((fileSystem) =>
{
    //todo: 对所有实现IFileSystem接口的服务进行修饰
});
```

## 服务构建事件之后

服务容器在每一次触发服务构建的事件后会引发`服务构建事件之后`事件，您可以使用`OnAfterResolving()`方法监听该事件。

同样，服务构建事件之后也包括`局部构建事件之后`和`全局构建事件之后`。

服务构建事件之后，对于一些需要在解决事件之后进行校验的程序非常有用。

#### 基于服务的构建事件之后

``` csharp
App.Singleton<IFileSystem, FileSystem>()
   .OnAfterResolving<FileSystem>((fileSystem) =>
   {
       //todo: 对于FileSystem的实例进行校验或其他操作
   });
```

#### 基于全局的构建事件之后

- 对全部服务进行修饰

``` csharp
App.OnAfterResolving((instance) =>
{
    //todo: 对于所有被构建的服务进行校验或其他操作
});
```

- 筛选指定的服务进行修饰

```csharp
App.OnAfterResolving<IFileSystem>((fileSystem) =>
{
    //todo: 对所有实现IFileSystem接口的服务进行校验或其他操作
});
```

## 服务释放事件

当容器使用`Release`方法或其他释放方法(UnBind解除绑定, Instance替换实例,Flash暂存,Flush重置)为已经生成的单例服务进行释放，会触发服务释放事件。

在释放事件中，您可以对服务进行最后的处理（如：资源释放，关闭关联服务）。

释放事件分为2种类型，`局部释放事件`将会优于`全局释放事件`被调用，您可以使用`OnRelease()`方法监听该事件。以下行为都可能引发释放事件:

- 通过`Instance()`为单例服务赋予一个全新的服务，老的服务将会被触发释放事件。
- 通过`Release()`直接释放单例服务。
- 通过`Unbind()`解除服务绑定关系。
- 当容器发生`Flush()`时会触发释放事件。
- `Flash`时的单例对象。

#### 基于服务的释放事件

``` csharp
App.Singleton<IFileSystem, FileSystem>()
   .OnRelease<FileSystem>((fileSystem) =>
   {
       //todo: 对于FileSystem的单例实例释放时
   });
```

#### 基于全局服务的释放事件

- 对全部服务进行修饰

``` csharp
App.OnRelease((instance) =>
{
    //todo: 对于所有被释放的服务进行处理
});
```

- 筛选指定的服务进行修饰

```csharp
App.OnRelease<IFileSystem>((fileSystem) =>
{
    //todo: 对所有实现IFileSystem接口的服务释放时，进行处理
});
```

> 请注意：通过[实例绑定](#实例绑定)的服务被释放时并不会触发这个事件。

## 重定义事件

当服务实现发生变化时会触发重定义事件，通过关注这个事件将可以获得服务的变化，重定义事件对单例绑定和实例绑定的服务均有效，下面这些行为将会触发重定义事件：

- 对一个已经绑定，并且被构建过的服务重新绑定时。
- 使用`Instance()`更改已经被构建的单例服务实现。

下面行为将不会引发重定义事件：

- 对从来没有被构建的服务进行重绑定时不会引发重定义事件
- 当单例服务实例被释放时

您可以通过 `OnRebound` 或者 `Watch` 方法对服务重定义进行关注，Watch方法为OnRebound的扩展别名方法。

``` csharp
App.Watch<IFileSystem>((instance)=>{
    // instance 为新的服务实现
});
```

> - 通过重定义事件可以让您的程序始终使用最新的服务实现。
> - 通过[门面](facade.html)获得的服务实现永远是最新的。

## 单例化对象

服务容器可以通过`Instance`单例化您自己生成的实例，随后通过容器获取该实例总是返回被托管的实例。

``` csharp
App.Instance("filesystem" , new FileSystem());
```

需要注意的是，如果您使用了`Bind`为指定的服务进行实例绑定，那么如果您尝试将这个实例单例化，服务容器将会抛出一个`LogicException`异常:

``` csharp
App.Bind("filesystem", (container, userParams)=> new FileSystem());
App.Instance("filesystem" , new FileSystem()); // throw LogicException
```

> - 单例化对象不会经过[扩展服务](#扩展服务)处理。
> - 需要单例化的实例允许为`null`值。

## 服务编组

您可以为多个服务进行编组，编组后将允许您一次生成多个服务,服务编组是使用`Tag`进行编组的。

这在一些场景中是非常有用的比如：

- 游戏战场内所需要的模块，可以通过服务编组统一加载和统一释放
- 日志将会被记录到文件日志服务和网络日志服务

```csharp
public class Tags
{
    public const BattleManagerTags = "BattleManagerTags";
}
```

- 对服务进行编组
``` csharp
App.Singleton<IBattleMonster, BattleMonsterManager>()
   .Tag(Tags.BattleManagerTags);
App.Singleton<IBattleMonster, BattleCameraManager>()
   .Tag(Tags.BattleManagerTags);
```

- 生成指定编组的服务
```csharp
App.Tagged(Tags.BattleManagerTags); // object[]{ BattleMonsterManager, BattleCameraManager }
```

#### 释放服务编组

您可以通过`Release`方法为编组的对象进行释放，这在一些场景下可能非常方便，比如:退出游戏战场时统一释放游戏战场的单例对象。

```csharp
var services = App.Tagged(Tags.BattleManagerTags);
```

```csharp
App.Release(ref services);
```

> 如果有任何一个服务未能从容器找到，将会返回`false`，同时`ref service`变量中将会出现未能在容器中被找到的服务实现。

## 上下文绑定

有时侯我们可能有两个不同的服务使用同一个接口，但我们希望在每个服务中注入不同实现, 我们可以通过上下文绑定来解决这个问题。

#### 通过绑定类型名来描述上下文

```csharp
App.Singleton<ScreenshotUpload>()
    .Needs<IDisk>()
    .Given(()=> FileSystem.Disk("aliyun-oss"))
```

当截图上传服务需求一个抽象磁盘时，使用闭包来为其提供一个文件系统管理器下的磁盘。

#### 通过绑定变量名来描述上下文

绑定的变量名必须以`$`开头，对于变量名的大小写敏感。

```csharp
App.Singleton<ScreenshotUpload>() 
    .Needs("$disk")
    .Given(()=> FileSystem.Disk("aliyun-oss"))
```

当构建截图上传服务时，框架查找截图上传服务构造函数中的disk变量，并为其给定指定实现。

## 扩展服务

服务容器允许通过`Extend`修改一个已经被构建过的单例服务或者为尚未被构建的服务建立额外的修饰处理。

如果`Extend`的是一个已经被构建的服务，那么该扩展代码即刻生效，但不会对后续的服务产生影响（指定构建的服务被释放后又被重新构建）。

如果`Extend`的服务是一个尚未被构建的服务，那么该扩展对未来构建的指定服务持续生效。

```csharp
App.Extend<IFileSystem>((fileSystem) =>
{
    // extend fileSystem
    return fileSystem;
});
```

## 绑定函数

服务容器除了可以为服务进行绑定外还可以进行函数绑定，您可以通过`BindMethod`方法来为函数进行绑定，服务容器会自动对绑定函数所需求的参数进行分析，并提供合适的注入参数。

``` csharp
App.BindMethod("CustomizeFunction" , ()=>{
    // todo: 你的函数行为
});
```

如果您需要为绑定的函数参数进行[上下文绑定](#上下文绑定)，您可以这么做：

``` csharp
var binder = App.BindMethod("CustomizeFunction" , (IFileSystem fileSystem)=>{ });
binder.Needs<IFileSystem>.Given(()=> new HttpFileSystem());
```

## 调用绑定函数

通过`Invoke`方法可以调用一个被绑定的函数。您可以为调用的函数传入自定义的参数，这些参数会根据服务容器的[依赖注入规则](#依赖注入规则)选择合适的实例注入到函数参数中，如果函数参数存在不能被解决的参数，那么将会抛出一个`UnresolvableException`异常。

``` csharp
App.Invoke("CustomizeFunction", /* Your params*/);
```

## 解除绑定函数

通过`UnbindMethod`方法可以解除一个绑定函数，该方法支持以下几种解除方式：

- 如果传入参数为字符串(`string`)则作为方法名解除对应方法名的函数。
- 如果传入参数为方法绑定对象(`IMethodBind`)则解除对应函数。
- 如果传入参数为其他对象(`object`)则作为调用目标,解除所有基于这个调用目标的函数

``` csharp
App.UnbindMethod("CustomizeFunction");
```

## 依赖注入调用方法

通过`Call`方法可以对任何`函数`或者`Lambda`发起依赖注入调用，容器将会根据[依赖注入规则](#依赖注入规则)自动解决函数所需求的参数。

``` csharp
App.Call((IFileSystem fileSystem)=>{
    // todo
});
```

## 当查询类型时

当容器尝试去推测一个没有被绑定过的服务时，会通过遍历`OnFindType()`注册的查询函数来查询服务的`Type`。

一般情况下，这种使用场景多数用于跨程序集的Type查找。

``` csharp
App.OnFindType((finder)=> { 
    return Type.GetType(finder); 
});
```

> `OnFindType()` 允许传入一个优先级，优先级高的会被优先调用。
> 注意，当生成的服务依赖于`OnFindType()`来推测服务时，服务构建将会使用c#的反射服务，这一过程对性能的损耗非常严重。所以您应该尽可能使用单例绑定而不是依赖推测来动态生成。

## 暂时性托管

服务容器允许您使用`Flash`方法来临时托管服务实例，在回调区间完成后这些临时的托管实例将会被释放。

``` csharp
App.Flash(()=>{

}, /* Service name */, /* Your service instance */);
```

> 由暂时性托管的单例实现被释放时会引发[服务释放事件](#服务释放事件)

## 可变类型

使用`IVariant`接口标记的类将会被容器认为是可变类型，可变类型允许容器将开发者传入的基础类型参数进行转换为可变类型。

默认的基础类型参数为：`Boolean`,`Byte`,`SByte`,`Int16`,`Int32`,`UInt32`,`Int64`,`IntPtr`,`UIntPtr`,`Char`,`Double`,`Single`,`String`.

```csharp
public class ItemModel : IVariant
{
    public string itemName;
    public ItemModel(int itemId)
    {
        this.itemName = (itemId == 10) ? "连衣裙" : "手套";
    }
}
```

```csharp
public class ItemUI
{
    public ItemUI(string name, ItemModel model)
    {
        // name : 道具详情
        // model.itemName : 连衣裙
    }
}
```

```csharp
App.Make<ItemUI>("道具详情", 10);
```

> 使用可变类型，您将可以隐藏繁琐的转换操作，框架将会自动完成类型间的转换。

## IConvertible转换

服务容器支持对于实现`IConvertible`接口对象的转换。

```csharp
public class ItemUI
{
    public ItemUI(string name)
    {
        // name : 10
    }
}
```

```csharp
App.Make<ItemUI>(10);
```

> 框架将会优先判定可变类型的转换，即如果可以通过可变类型转换，将优先使用可变类型转换。

## 重置服务容器

您可以使用`Flush`来重置服务容器，清除服务容器中的一切数据，恢复到初始状态，这是一个危险操作所以我们将API隐藏在Handler中。

``` csharp
App.Handler.Flush();
```

> 在Flush重置容器的过程中,如果是单例服务，将会引发[服务释放事件](#服务释放事件)。

## 参数表/参数匹配器

服务容器允许开发者传入参数表来对依赖注入的对象进行参数匹配，如果您需要使用参数表，那么在`Make`服务时需要提供实现`IParams`接口的数据结构。

框架已经提供了默认的参数表结构：`Params`，默认参数表是按照`变量名`进行匹配且大小写敏感。

- 参数表触发顺序为传入的参数表顺序。
- 参数表的匹配是最优先进行的。
- 参数表提供的参数类型如果和注入参数类型不一致则会尝试进行参数类型转换，如果转换失败则会使用[依赖注入规则](#依赖注入规则)。

```csharp
public class FileSystem : IFileSystem
{
    public FileSystem(IDisk disk)
    { 
        // disk = new LocalDisk();    
    }
}
```

```csharp
App.Make<IFileSystem>(new Params(){
    { "disk" , new LocalDisk() }
});
```

## 紧缩参数注入

服务容器允许通过特殊类型`object`或`object[]`来一次性接受用户传入的全部参数。

```csharp
public class Tight
{
    public Tight(object[] paramsObjects)
    { 
        // paramsObjects[0] == 1
        // paramsObjects[1] == "str"
    }
}
```

```csharp
App.Make<Tight>(1, "str");
```

如果接收类型为`object`，且传入参数个数为1个，那么参数将会被自动解构,否则将会是一个`object[]`数组。

```csharp
public class Tight
{
    public Tight(object paramsObject)
    { 
        // paramsObject == "str"
    }
}
```

```csharp
App.Make<Tight>("str");
```

## 依赖注入规则

- **执行构造函数注入**
- 遍历参数列表:
-- **通过参数匹配器获取可以用于被注入的参数**
----- 默认匹配器通过参数名和传入的匹配表进行匹配，返回一个有效的结果。
-- **检查是否为紧缩注入参数(`object`或`object[]`)，并且可以进行紧缩注入：**
----- 是：将用户传入的参数紧缩注入，清空用户传入的参数。
-- **从剩余的用户传入的参数中挑选并弹出合适的参数。**
-- 使用依赖注入容器进行分析：
----- **如果是：类，接口注入：**
--------- 执行服务构建，如果产生异常则执行后续步骤
--------- 使用参数名来推测服务实现
--------- 如果推测失败，则检查是否可以使用默认值。
--------- 如果均失败：抛出`UnresolvableException`异常
----- **如果是基础类型注入：**
--------- 如果可以构建则，执行服务构建
--------- 使用参数名来推测服务实现
--------- 如果推测失败，则检查是否可以使用默认值
--------- 如果均失败：抛出`UnresolvableException`异常
- **执行实例的特性注入**
- 遍历参数列表:
-- **检查可读写状态，不可写则跳过**
-- 使用依赖注入容器进行分析：
----- **如果是：类，接口注入：**
---------- 递归执行服务构建
---------- 如果构建失败：抛出`UnresolvableException`异常
----- **如果是基础类型注入：**
---------- 如果可以构建则，递归执行服务构建
---------- 使用属性名来推测服务实现
---------- 如果推测失败：抛出`UnresolvableException`异常
- 注入完成

## 其他常规函数

- **Type2Service(type)** 将类型转为服务名 
- **GetBind(service)** 获取服务的绑定数据,如果绑定不存在则返回null
- **HasBind(service)** 是否已经绑定了服务
- **HasInstance(service)** : 是否存在单例化实例
- **IsResolved(service)** : 服务是否已经被解决过
- **CanMake(service)** 是否可以生成服务
- **IsStatic(service)** 服务是否是单例化的,如果服务不存在也将返回false
- **IsAlias(service)** 服务名是否是别名
- **BindIf(service, concrete)** 当服务不存在时才绑定(实例绑定)
- **SingletonIf(service, concrete)** 当服务不存在时才绑定(单例绑定)
- **Wrap(lambda,params)** 包装一个触发可以以依赖注入形式调用的方法
- **Factory(service)** 包装一个触发可以获得指定服务的方法

## 容器行为定制

当开发者需要深度定制容器行为时可以重构下面提供的虚方法来调整容器默认行为。容器行为定制只有您非常了解虚函数对应的行为才能操作。

- **IsBasicType**：是否为默认的基础类型（默认情况下为：原始类型+string）
- **IsUnableType**：是否是无法被构建的类型
- **WrapperTypeBuilder**：包装一个类型，可以被用来生成服务
- **GetDependenciesFromUserParams**：从用户传入的参数中获取合适的参数进行注入。
- **ChangeType**：转换类型到指定类型。
- **GetPropertyNeedsService**：传入一个属性选择器对象(`PropertyInfo`)获取对应对象需求的服务名。
- **GetParamNeedsService**：传入一个参数对象(`ParameterInfo`)获取对应的参数需求的服务名
- **ResolveAttrPrimitive**：当解决属性选择器的基础类型时，返回值为需求的服务实例。
- **ResloveAttrClass**：当解决属性选择器类或者接口类型时，返回值为需求的服务实例。
- **ResolvePrimitive**：当解决构造函数的基础类型时，返回值为需求的服务实例。
- **ResloveClass**：当解决构造函数的类或者接口类型时，返回值为需求的服务实例。
- **SpeculationServiceByParamName**：根据一个参数信息来推测服务。返回值为需求的服务实例。
- **GetBuildStackDebugMessage**：获取编译堆栈的调试信息。
- **MakeBuildFaildException**：构建一个编译失败的异常。
- **MakeUnresolvablePrimitiveException**：构建一个未能解决基础参数的异常。
- **MakeCircularDependencyException**：构建一个循环依赖异常。
- **FormatService**：格式化(标准化)服务名。
- **CanInject**：检查实例是否可以被注入。
- **GuardUserParamsCount**：限制用户传入的参数数量。
- **GuardResolveInstance**：限制解决实例的有效条件。
- **SpeculatedServiceType**：根据服务名推测指定的服务类型。
- **AttributeInject**：对指定的实例进行属性注入。
- **CheckCompactInjectUserParams**：检查是否可以紧缩注入用户传入的参数。
- **GetParamsMatcher**：获取一个参数匹配器，参数匹配器匹配到的有效结果将作为最优参数值。
- **GetCompactInjectUserParams**：获取紧缩注入的用户参数。
- **GetDependencies**：获取指定参数列表的依赖注入结果。
- **GetConstructorsInjectParams**：选择一个合适的构造函数并且获取指定需要注入的参数。
- **GuardMethodName**: 验证函数方法名是否是有效的。
- **GuardServiceName**: 验证函数服务名是否是有效的。
- **GuardConstruct**: 验证当前状态是否是允许构建服务(或调用函数)的。
- **CreateInstance**: 通过反射创建指定类型的实现。
- **GetContextualClosure**： 根据上下文获取需求的闭包。
- **MakeFromContextualClosure**：根据上下文闭包来构建需求的实例。
- **GetContextualService**：根据上下文获取需求的服务名
- **MakeFromContextualService**：根据上下文需求服务名生成服务实例。
- **ResloveFromContextual**：通过上下文来解决服务实现。

## 性能优化相关

最优的方案是使用[Facade](facade.html)来访问单例绑定的服务，这样您将获得和源生代码直接访问差距不大的访问性能。

一般情况下我们推荐使用单例绑定的方式来对无依赖注入的服务进行实例。以便于能够获得更好的性能。

下面的代码将进行非反射生成的单例绑定：

``` csharp
App.Singleton<IFileSystem, FileSystem>((binder, param)=> new FileSystem());
```

下面的代码将进行反射生成的单例绑定：

``` csharp
App.Singleton<IFileSystem, FileSystem>();
```

除了进行单例绑定外，我们还需要尽可能将服务设置为单例绑定，服务容器对于单例服务的优化要优于实例绑定的服务。

我们已经测试了服务生成调用所花费的时间以供您参考，所有的测试均建立于：`100万`次服务生成测试（测试`100次`的平均值），和`Debug`编译的程序集上。

- 非反射生成(单例): `68 ms`
- `(常用)` 反射生成(单例，依赖注入): `68 ms` (1ms = 14705次调用Make)
- 反射生成(单例，无注入): `62 ms`
- 非反射生成(实例): `745 ms`
- 反射生成(实例，依赖注入): `4959 ms`
- 反射生成(实例，无注入): `1599 ms`
- `(常用)` Facade(门面)(单例，依赖注入): `61 ms` (1ms = 16393次调用Facade)
- Facade(门面)(实例，依赖注入): `5419 ms`
- Facade(门面)(实例，无注入): `2144 ms`

> 在测试环境下，原生单例模版访问的速度为：`21 ms` (1ms = 47619次原生调用)