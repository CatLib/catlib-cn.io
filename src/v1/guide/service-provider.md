---
title: 服务提供者
type: guide
order: 100
---

## 服务提供者

服务提供者是`组件`和`CatLib`联系的桥梁。同时也是CatLib启动的中心，你自己的程序以及所有CatLib的核心服务都是通过服务提供者启动。

> 服务提供者是CatLib核心提供的功能，如果您需要将CatLib的组件移植到其他框架使用，那么您需要移除服务提供者。

### 实现服务提供者

您的服务提供者类必须继承自`CatLib.ServiceProvider`类 , 大部分服务提供者都包含两个方法：`Register()`和`Init()`，其中`Register()`是必须要实现的。

在`Register()`方法中，你`唯一`要做的事情就是`绑定服务实现`到服务容器，不要尝试在其中执行任何其它功能。因为很有可能你就使用了一个没有被注册的服务。

当服务提供者的`Init()`方法被触发时，就意味着框架所有的服务提供者的`Register()`都已经被执行，也就是说我们可以在`Init()`中访问其他服务提供者所提供的服务，只有当所有服务提供者的`Init()`执行完成后才会触发框架启动完成的事件。

``` csharp
using CatLib;
public class ConfigProvider : ServiceProvide
{
    public override IEnumerator Init()
    {
        yield return base.Init();
    }
    public override void Register()
    {
        // 注册服务
    }
}
```

### 注册服务提供者

> 注册了服务提供者并不意味着服务都会被立即实例化，通常情况下很多是延迟实例化的，只有真的用到它们的时候才会实例化。

如果框架使用者想要使用某个服务，那么必须先对这个服务进行注册，您只有在框架`Application.Init()`触发之前才能注册服务提供者:

``` csharp
App.Instance.Register(typeof(ConfigProvider));
```

服务提供者的注册一般情况下都应该在`Application`的引导流程(Bootstrap)中进行。

除了标准注册外，CatLib.Unity项目为开发者提供了快捷的注册方案，以便于您不需要书写标准的注册代码来注册服务提供者（如果您没有从CatLib.Unity开始您需要自行处理这部分功能），您可以通过`Game/Providers.cs`文件来配置服务提供者，您只需要配置`ServiceProviders`字段的返回值就能完成服务提供者配置。

``` csharp
public class Providers
{
    public static Type[] ServiceProviders
    {
        get
        {
            return new[]
            {
                typeof(ConfigProvider),
            };
        }
    }
}
```

### 初始化优先级

您可以通过配置启动优先级来调整服务提供者启动流程的启动顺序，注意CatLib中的所有的优先级特性都是采用就近原则的。即为函数标记的优先级特性将会优先于类的优先级特性。

``` csharp
using CatLib;
[Priority(200)]
public class ConfigProvider : ServiceProvide
{
    [Priority(100)]
    public virtual IEnumerator Init()
    {
        // Init 的启动优先级会使用100而不是200 ， 由于就近原则
        yield break;
    }
    public override void Register()
    {
        // 注册服务
    }
}
```

关于更多优先级相关资料，请参考：[优先级](application.html#优先级)

### 启动顺序

除了`Init()`可以指定具体的服务提供者之间的启动优先级外`Register()`是无序执行的。

他们的流程是这样的：所有注册服务提供者执行`Register()` -> 按照优先级依次执行`Init()` -> 框架启动完成。