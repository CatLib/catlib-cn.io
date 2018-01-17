---
title: 服务容器
type: guide
order: 101
enable: true
---

## 服务容器

Container 服务容器是一个用于管理`类的依赖`和`执行依赖注入`的强大工具。其实质是通过反射来对`构造函数`或者标记为`[Inject]`特性的属性选择器进行注入。

### 简介

几乎所有的服务绑定都是在服务提供者中完成。如果一个`服务`没有基于任何接口那么就没有必要将其绑定到容器。

容器并不需要被告知如何构建对象，因为它会使用反射服务自动解析出具体的对象实例。在任何位置，您可以通过`App`全局变量访问容器。在服务提供者中我们可以使用`Bind()`或者`Singleton()`方法来注册一个`服务`的绑定。

``` csharp
var container = new Container(); // 构建一个单纯的服务容器
```

### 构建服务

您可以通过`Make()`方法来构建服务。如果通过容器去生成一个没有被绑定的服务，那么容器会根据策略尝试解决需要被生成的服务，如果尝试解决失败，那么将会抛出一个`UnresolvableException`异常。

> `CatLib Framework` 中所有可以直接通过`Make()`生成的服务请参考：[服务表](can-make.html)

### 绑定服务 - 实例

要使用服务容器的强大特性您首先需要将服务绑定到服务容器，如果您不不进行绑定您将没有任何服务可用，这些绑定一般性都是在 [服务提供者](service-provider.html) 中完成的。

绑定服务分为以下两种类型：

- 实例绑定 - 每次生成都会是一个全新实例
- 单例绑定 - 只在第一次生成实例，之后的调用都将返回这个实例

通过`Bind()`方法允许将服务以实例绑定的方式进行绑定。

``` csharp
App.Bind<SocketManager>().Alias<ISocketManager>();
```

我们通过上述代码将`SocketManager`这个服务绑定到了服务容器。同时赋予了`ISocketManager`的别名。

这样您就可以使用使用实例名或者别名来构建服务了，但是请注意：一般情况下我们都建议通过`ISocketManager`来生成服务。

``` csharp
var manager = App.Make<ISocketManager>(); // 等价与: var manager = new SocketManager();
```

可能至此您还没有明白服务容器的意义，没关系，请继续阅读。

### 绑定服务 - 单例

服务容器通过`Singleton`方法来对服务进行单例绑定。通过 [门面](facade.html) 和 服务容器 的结合，颠覆了传统的单例实现方式，以一种更新，更易于维护的方式来实现传统的单例模式。

``` csharp
App.Singleton<SocketManager>().Alias<ISocketManager>();

var manager1 = App.Make<ISocketManager>();
var manager2 = App.Make<ISocketManager>();
// manager1 == manager2
```

### 构造函数注入

在您简单的`Make()`操作中，服务容器已经对绑定的服务进行了一系列复杂操作（服务关系转换，服务构建，服务推测，上下文关系处理，依赖注入，服务修饰，单例化...），这一切对于开发者的您来说完全是透明的无感知的。

服务容器能够自动识别您构造函数中的参数，并为其注入合适的服务实例。注入方案请参考：[依赖注入规则](container.html#依赖注入规则)

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

> 简而言之，您只需要向服务容器绑定好您开发的服务，服务容器将会在您生成服务时自动处理好您的服务依赖关系。

### 属性选择器注入

服务容器采用`特性标记`的方法来识别哪些属性选择器是允许被注入的。

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

> 注意，您不能在`构造函数`中访问被标记为注入的`属性选择器`的实例，因为属性选择器的注入流程会在构造函数之后进行，如果您在构造函数中访问了被标记为注入的属性选择器，这将会导致一个`NullReferenceException`异常。

### 服务别名

`服务别名`是服务容器非常重要的一项功能，它可以提供：接口绑定指定实现的能力。

``` csharp
App.Singleton<SocketManager>().Alias<ISocketManager>();
```

通过`App.Make<ISocketManager>()`您可以获得`SocketManager`的实现，由于您面向接口编程，无需关注底层实现，从而服务可以实现无感知替换。

### 服务构建事件

服务容器在每一次新构建对象时都会触发服务构建的事件，可以使用`OnResolving()`方法监听该事件。

该事件可以进行对被构建服务的修饰处理，允许你在对象被传递给开发者之前为其设置额外属性。

构建事件分为2种类型，`局部构建事件`将会优于`全局构建事件`被调用，构建事件的特性如下：

- 基于指定服务的构建事件，只有当指定服务被构建时才会被触发。
- 基于全局的构建事件，在任何服务被构建时都会触发。
- 静态服务的构建事件只会在第一次构建时触发一次。
- 使用`Instance()`直接设定服务实例会触发服务构建事件。

**基于服务的构建事件**

``` csharp
App.Singleton<SocketManager>().OnResolving((binder, instance) =>
{
    var socketManager = (SocketManager)instance;
    //todo: 对于SocketManager的实例进行修饰
    return instance;
});
```

**基于全局的构建事件**

``` csharp
App.OnResolving((binder, instance) =>
{
    //todo: 对于所有被构建的服务进行修饰
    return instance;
});
```

### 服务释放事件

容器允许您使用`Release`方法为已经生成的静态服务进行释放。

``` csharp
App.Release<ISocketManager>();
```

在释放静态服务后会触发服务释放事件。在释放事件中，您可以对服务进行最后的处理（如：资源释放）。请注意：通过实例绑定的服务被释放并不会触发这个事件。

释放事件分为2种类型，`局部释放事件`将会优于`全局释放事件`被调用，您可以使用`OnRelease()`方法监听该事件。以下行为都可能引发释放事件:

- 通过`Instance()`为静态服务赋予一个全新的服务，老的服务将会被触发释放事件。
- 通过`Release()`直接释放静态服务。
- 通过`Unbind()`解除服务绑定关系。
- 当容器发生`Flush()`时会触发释放事件。

**基于服务的释放事件**

``` csharp
App.Singleton<SocketManager>().OnRelease((binder, instance) =>
{
    //todo: 对于SocketManager的静态实例释放时
});
```

**基于全局服务的释放事件**

``` csharp
App.OnRelease((binder, instance) =>
{
    //todo: 任何被释放的静态实例都会触发
    return instance;
});
```

### 重定义事件

当服务实现发生变化时将会触发重定义事件，通过关注这个事件将可以获得服务的变化，重定义事件对单例绑定和实例绑定的服务均有效，下面这些行为将会触发重定义事件：

- 对一个已经绑定，并且被构建的服务重新绑定时。
- 使用`Instance()`更改已经被构建的服务实现。

下面行为将不会引发重定义事件：

- 任何没有被构建的服务发生服务实例变更不会引发重定义事件
- 当服务实例被释放时
- `Instance()`修改一个由`Instance()`直接托管的服务时

您可以通过 `OnRebound` 或者 `Watch` 方法对服务重定义进行关注，Watch方法为OnRebound的扩展别名方法。

``` csharp
App.Watch<ISocketManager>((instance)=>{
    // instance 为新的服务实现
});
```

### 静态化托管

服务容器可以通过`Instance`托管您自己生成的实例，随后对容器该实例的调用总是返回被托管的实例。

``` csharp
App.Instance("socket" , new SocketManager());
```

需要注意的是，如果您使用了`Bind`为指定的服务进行实例绑定，那么如果您尝试将这个服务静态化，服务容器将会抛出一个`RuntimeException`异常:

``` csharp
App.Bind("socket", (container, userParams)=> new SocketManager());
App.Instance("socket" , new SocketManager()); // throw RuntimeException
```

### 绑定函数

服务容器除了可以为服务进行绑定外还可以进行函数绑定，您可以通过`BindMethod`方法来为函数进行绑定，服务容器会自动对绑定函数所需求的参数进行分析，并提供合适的注入参数。

``` csharp
App.BindMethod("CustomizeFunction" , ()=>{
    // todo: 你的函数行为
});
```

如果您需要为绑定的函数参数进行上下文绑定，您可以这么做：

``` csharp
var binder = App.BindMethod("CustomizeFunction" , ()=>{ });
binder.Needs<INetwork>.Given<ISocketManager>();
```

当函数参数需求一个INetwork类型服务时将会将服务重定向为ISocketManager

### 调用绑定函数

通过`Invoke`方法可以调用一个被绑定的服务。您可以为被调用的函数传入参数，这些参数会根据服务容器的注入规则选择合适的实例注入到函数参数中，如果函数参数存在不能被解决的参数，那么将会抛出一个`UnresolvableException`异常。

``` csharp
App.Invoke("CustomizeFunction", /* Your params*/);
```

### 解除绑定的函数

通过`UnbindMethod`方法可以解除一个绑定的方法，该方法支持以下几种解除方式：

- 如果为字符串(`string`)则作为方法名解除对应方法名的函数。
- 如果为方法绑定对象(`IMethodBind`)则解除对应函数。
- 如果为其他对象(`object`)则作为调用目标,解除所有基于这个调用目标的函数

``` csharp
App.UnbindMethod("CustomizeFunction");
```

### 依赖注入调用

通过`Call`方法可以对任何函数或者Lambda发起依赖注入调用，容器将会根据规则自动解决函数所需求的参数。

``` csharp
App.Call(()=>{
    // todo
});
```

### 上下文绑定

有时侯我们可能有两个服务使用同一个接口，但我们希望在每个服务中注入不同实现, 我们可以通过上下文绑定来解决这个问题。

``` csharp
App.Singleton<FileLog>().Alias("log.file");
App.Singleton<DatabaseLog>().Alias("log.database");
```

(`FileLog`和`DatabaseLog`均实现了`ILog`接口)

``` csharp
App.Bind<LogServiceDatabase>().Needs<ILog>().Given("log.database");
App.Bind<LogServiceFile>().Needs<ILog>().Given("log.file");
```

这样当生成`LogServiceDatabase`服务时给定的ILog实例将会是`DatabaseLog` ，`LogServiceFile`服务时给定的ILog实例将会是`FileLog`。

除此以外我们还可以使用`[Inject]`标记来对注入实例指定实现，一般情况下服务容器的注入上下文绑定功能是使用就近原则的（除非您对别名做了额外的上下文定义，这种罕见情况不在这份基础文档的描述范围之内）。

``` csharp
public class LogService
{
    [Inject("log.database")]
    public ILog Log { get; set; }
    public LogService([Inject("log.file")]ILog logWithFile)
    {
    }
}
```

### 服务编组

您可以为多个服务进行编组，编组后将允许您一次生成多个服务。这在一些场景中是非常有用的比如：日志将会被记录到文件日志服务和网络日志服务。

``` csharp
App.Singleton<FileLog>().Alias("log.file");
App.Singleton<NetworkLog>().Alias("log.network");
App.Tag("log" , "log.file" , "log.network");
var logServices = App.Tagged("log"); // object[]{ FileLog, NetworkLog }
```

### 解除服务绑定

您可以通过`UnBind()`来对已经绑定的服务进行解除绑定。如果被绑定的类是静态的，那么解除绑定会自动释放已经被生成的静态实例，同时触发`OnRelease()`释放事件。

``` csharp
var binder = App.Singleton<SocketManager>();
binder.UnBind();
```

### 当查询类型时

当容器尝试去推测一个没有被绑定过的服务时，会通过遍历`OnFindType()`注册的查询函数来查询服务的`Type`。

一般情况下，这种使用场景多数用于跨程序集的Type查找。

``` csharp
App.OnFindType((finder)=> { return Type.GetType(finder); } , 200);
App.OnFindType((finder)=> { return MyType.GetType(finder); } , 100);
```

`OnFindType()` 允许传入一个优先级，优先级高的会被优先调用。

> 注意，当生成的服务依赖于`OnFindType()`来推测服务时，服务构建将会使用c#的反射服务，这一过程对性能的损耗非常严重。所以您应该尽可能使用静态绑定而不是依赖推测来动态生成。

### 暂时性托管

服务容器允许您使用`Flash`方法来临时托管服务实例，在回调区间完成后这些临时的托管实例将会被释放。

``` csharp
App.Flash(()=>{

}, /* Service name */, /* Your service instance */);
```

### 可变类型

使用`IVariant`接口标记的类将会被容器认为是可变类型，可变类型允许容器使用开发者传入的基础类型参数进行变换（即将基础参数变化为可变类型）。

默认的基础类型参数为：`Boolean`,`Byte`,`SByte`,`Int16`,`Int32`,`UInt32`,`Int64`,`IntPtr`,`UIntPtr`,`Char`,`Double`,`Single`,`String`.

```csharp
public class ItemModel : IVariant
{
    public int itemId;
    public ItemModel(int itemId)
    {
        this.itemId = itemId;
    }
}
```

```csharp
public class ItemUI
{
    public string name;
    public ItemModel model;
    public ItemUI(string name, ItemModel model)
    {
        this.name = name;
        this.model = model;
    }
}
```

```csharp
var ui = container.Make<ItemUI>("道具详情", 10);
// ui.name : 道具详情
// ui.model.itemId : 10
```

### 重置服务容器

您可以使用`Flush`来重置服务容器，清除服务容器中的一切数据恢复到初始状态，这是一个危险操作所以我们将API隐藏在Handler中。

通过Flush重置容器的过程中，会触发`OnRelease`事件。

``` csharp
App.Handler.Flush();
```

### 依赖注入规则

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

### 其他常规函数

- Type2Service(type); 将类型转为服务名 
- GetBind(service); 获取服务的绑定数据,如果绑定不存在则返回null
- HasBind(service); 是否已经绑定了服务
- HasInstance(service) : 是否存在静态化实例
- IsResolved(service) : 服务是否已经被解决过
- CanMake(service); 是否可以生成服务
- IsStatic(service); 服务是否是静态化的,如果服务不存在也将返回false
- IsAlias(service); 服务名是否是别名
- BindIf(service, concrete); 当服务不存在时才绑定(实例绑定)
- SingletonIf(service, concrete); 当服务不存在时才绑定(单例绑定)
- Wrap(lambda,params); 包装一个触发可以以依赖注入形式调用的方法
- Factory(service); 包装一个触发可以获得指定服务的方法

### 容器行为定制

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

### 性能优化相关

最优的方案是使用[Facade](facade.html)来访问静态绑定的服务，这样您将获得和源生代码直接访问一样的性能。

一般情况下我们推荐使用静态绑定的方式来对无依赖注入的服务进行实例。以便于能够获得更好的性能。

下面的代码将进行静态绑定：

``` csharp
App.Singleton<SocketManager>((binder, param)=> new SocketManager());
```

下面的代码将进行动态绑定：

``` csharp
App.Singleton<SocketManager>();
```

除了进行静态绑定外，我们还需要尽可能将服务设置为单例绑定，服务容器对于单例服务的优化要优于实例服务。

我们已经测试了服务生成调用所花费的时间以供您参考，所有的测试均建立于：`100万`次服务生成测试（测试`100次`的平均值），和`Debug`编译的程序集上。

- 静态绑定生成(进行单例绑定): `329 ms`
- 静态绑定生成(进行实例绑定): `844 ms`
- 动态绑定生成(进行单例绑定): `365 ms`
- 动态绑定生成(进行实例绑定): `11537 ms`