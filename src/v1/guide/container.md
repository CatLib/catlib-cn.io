---
title: 服务容器
type: guide
order: 101
---

## 服务容器

Container 服务容器是一个用于管理类的依赖和执行依赖注入的强大工具。其实质是通过反射来对构造函数或者标记为`[Inject]`特性的属性选择器进行注入(将服务实现依赖注入到类中)。

### 简介

几乎所有的服务绑定都是在服务提供者中完成。如果一个服务或类没有基于任何接口或者别名那么就没有必要将其绑定到容器。

容器并不需要被告知如何构建对象，因为它会使用反射服务自动解析出具体的对象。在一个服务提供者中，可以通过`App`变量访问容器，然后使用`Bind()`方法注册一个服务或类的绑定。

<p class="tip">下面文档中所使用的代码均是局部代码。您需要根据使用场景酌情调整。</p>

### 构建服务容器
``` csharp
var container = new Container();
```
CatLib核心(`Application`)继承服务容器，她拥有服务容器的全部功能，所以下面的演示例子用到的都是这种上下文环境。

### 绑定服务

``` csharp
App.Instance.Bind<ConfigManager>().Alias<IConfigManager>().Alias("config.manager");
```

上述代码进行了一次简单的绑定，实现的作用是将`ConfigManager`绑定到容器，并且给定了`IConfigManager`(接口别名)和`config.manager`(字符串别名)的别名。

随后您就可以使用别名来构建服务了:

``` csharp
var manager1 = App.Instance.Make<IConfigManager>();
var manager2 = App.Instance.Make("config.manager") as IConfigManager;
```

### 绑定服务(单例)

在上面的绑定中我们会发现`manager1`和`manager2`是不相等的，这是由于使用常规绑定所导致的，常规绑定服务后，每次`Make()`都会生成新的服务实例。

下面我们将使用单例绑定来对服务进行绑定:

``` csharp
App.Instance.Singleton<ConfigManager>().Alias<IConfigManager>().Alias("config.manager");
```

### 绑定实例

CatLib还允许您对自己构建的实例进行绑定，随后对容器该实例的调用总是返回给定的实例。

``` csharp
App.Instance.Instance("config.manager" , new ConfigManager());
```
需要注意的是在以下几种情况将不能绑定:
- 服务在绑定服务时使用了常规绑定

### 别名

别名是服务容器非常重要的一项功能，它可以提供：接口绑定指定实现的能力。

``` csharp
App.Instance.Singleton<ConfigManager>().Alias<IConfigManager>().Alias("config.manager");
```

在高层使用时只需要使用别名就能获得ConfigManager的服务，而无需关注底层实现，从而可以实现无感知替换。

### 上下文绑定

有时侯我们可能有两个类使用同一个接口，但我们希望在每个类中注入不同实现,我们可以通过如下代码为它来指定实现。

``` csharp
App.Instance.Singleton<FileLog>().Alias("log.file");
App.Instance.Singleton<DatabaseLog>().Alias("log.database");
```

(`FileLog`和`DatabaseLog`均实现了`ILog`接口)

``` csharp
App.Bind<LogService1>().Needs<ILog>().Given("log.database");
App.Bind<LogService2>().Needs<ILog>().Given("log.file");
```

这样当生成`LogService1`服务时给定的ILog实例将会是`DatabaseLog` ，`LogService2`服务时给定的ILog实例将会是`FileLog`。

除此以外我们还可以使用`[Inject]`标记来对注入实例指定实现，一般情况下CatLib的注入上下文绑定功能是使用就近原则的（除非您对别名做了额外的上下文定义，这种罕见情况不在这份基础文档的描述范围之内）。

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

### 生成服务

您可以通过`Make()`方法来生成服务，服务名可以为接口或别名。如果通过容器去生成一个没有被绑定的服务，那么容器会尝试解决需要被生成的服务，如果尝试解决失败，那么将会返回一个`null`。

``` csharp
var manager = App.Instance.Make<IConfigManager>();
manager = App.Instance.Make("config.manager") as IConfigManager;
```

### 函数注入调用

容器允许您对函数进行依赖注入调用，同服务注入一样，容器会自动识别函数所需求的参数，并使用依赖注入的方式来填充需求参数。

``` csharp
var instance = new DemoService();
// DemoService 类中存在一个叫 FuncName 的方法
var result = App.Instance.Call(instance , "FuncName");
```

### 服务编组

您可以为多个服务进行编组，编组后将允许您一次生成多个服务。这在一些场景中是非常有用的比如：日志将会被记录到文件日志服务和网络日志服务。

``` csharp
App.Instance.Singleton<FileLog>().Alias("log.file");
App.Instance.Singleton<NetworkLog>().Alias("log.network");
App.Instance.Tag("log" , "log.file" , "log.network");
var logServices = App.Instance.Tagged("log");
```

### 释放静态服务

容器允许您对已经生成的静态服务进行释放，以便于减少不必要的内存开销。

您可以通过服务名或者别名来对静态实例进行释放：

``` csharp
App.Instance.Release("config.manager");
```

### 解除绑定

您可以通过`UnBind()`来对已经绑定的服务进行解除绑定。如果被绑定的类是静态的，那么解除绑定会自动释放已经被生成的静态实例，同时触发`OnRelease()`释放事件。


``` csharp
var binder = App.Instance.Singleton<ConfigManager>().Alias("config.manager");
binder.UnBind();
```

### 当查询生成类型时

当容器尝试去推测一个没有被绑定过的服务时，会通过遍历`OnFindType()`注册的查询函数来查询服务的`Type`。

一般情况下，这种使用场景多数用于跨程序集的Type查找。

``` csharp
App.Instance.OnFindType((finder)=> Type.GetType(finder) , 200);
App.Instance.OnFindType((finder)=> MyType.GetType(finder) , 100);
```

`OnFindType()` 允许传入一个优先级，优先级高的会被优先调用。

<p class="tip">注意，当生成的服务依赖于`OnFindType()`来推测服务时，服务构建将会使用c#的反射服务，这一过程对性能的损耗非常严重。所以您应该尽可能使用静态绑定而不是依赖推测来动态生成。<p>

### 当服务解决(构建)时

服务容器在每一次新解析对象时都会触发服务解决的事件，可以使用`OnResolving()`方法监听该事件。

使用场景往往用于对于解决服务的修饰(初始化，监控等)，从而允许你在对象被传递给消费者之前为其设置额外属性。

解决事件分为2种类型，局部解决事件将会优于全局解决事件被调用：
- 基于指定服务的解决事件，只有当指定服务被解决时才会被触发。
- 基于全局的解决事件，任何服务的解决或者绑定实例都会触发。

``` csharp
// 基于服务的解决事件
App.Instance.Singleton<ConfigManager>().OnResolving((binder, obj) =>
{
    //todo: 对于ConfigManager这个类型的实例进行修饰
    return obj;
});
```

``` csharp
// 基于全局的解决事件
App.Instance.OnResolving((binder, obj) =>
{
    //todo: 对于解决所有服务或者进行绑定的实例进行修饰
    return obj;
});
```

### 当静态实例释放时

如果托管于服务容器的静态实例被释放时都会触发释放事件，可以使用`OnRelease()`方法监听该事件。

托管的静态实例包括:
- 通过`Instance()`绑定的静态实例
- 通过`Singleton()`绑定并生成的静态实例

``` csharp
// 基于服务的静态实例释放事件
App.Instance.Singleton<ConfigManager>().OnRelease((binder, obj) =>
{
    //todo: 对于ConfigManager这个类型的静态实例释放时
    return obj;
});
```

``` csharp
// 基于全局的静态实例释放事件
App.Instance.OnRelease((binder, obj) =>
{
    //todo: 对于解决所有服务或者进行绑定的静态实例释放时
    return obj;
});
```

### 性能优化相关

一般情况下我们推荐使用静态绑定的方式来对无依赖注入的服务进行实例。以便于能够获得更好的性能。

下面的代码将进行静态绑定：

``` csharp
App.Instance.Singleton<ConfigManager>((binder, param)=> new ConfigManager()).Alias("config.manager");
```

下面的代码将进行动态绑定：

``` csharp
App.Instance.Singleton<ConfigManager>().Alias("config.manager");
```

除了进行静态绑定外，我们还需要尽可能将服务设置为单例绑定，服务容器对于单例服务的优化要优于实例服务。

我们已经测试了服务生成调用所花费的时间以供您参考，所有的测试均建立于：`100万`次服务生成测试（测试`100次`的平均值），和`Debug`编译的程序集上。

- 静态绑定生成(进行单例绑定): `329 ms`
- 静态绑定生成(进行实例绑定): `844 ms`
- 动态绑定生成(进行单例绑定): `365 ms`
- 动态绑定生成(进行实例绑定): `11537 ms`