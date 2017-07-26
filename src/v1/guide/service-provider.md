---
title: 服务提供者
type: guide
order: 100
---

## 服务提供者

服务提供者是`组件`和`CatLib`联系的桥梁。同时也是CatLib启动的中心，所有的服务都是通过服务提供者定义的。

### 名词定义

- `组件` 组件与CatLib没有任何关系，她们可以独立的运行在不同的框架中。
- `服务` 是由服务提供者将一个或者多个`组件`组合而成，一组可以被开发者使用的接口。

### CatLib架构图

<img src="../../images/architecture-diagram.svg" width="100%"/>

### 实现服务提供者

您的服务提供者类必须继承自`CatLib.IServiceProvider`类 , 所有的服务提供者包含 `Register()`和`Init()` 2个方法。

在`Register()`方法中，你`唯一`要做的事情就是`绑定服务实现`到服务容器，不要尝试在其中执行任何其它功能。因为很有可能你就使用了一个没有被注册的服务。

当服务提供者的`Init()`方法被触发时，就意味着框架所有的服务提供者的`Register()`都已经被执行，也就是说我们可以在`Init()`中访问其他服务提供者所提供的服务。

``` csharp
using CatLib;
public class ConfigProvider : IServiceProvide
{
    public void Init()
    {
        // 初始化
    }
    public void Register()
    {
        // 注册服务
    }
}
```

### 注册服务提供者

> 注册了服务提供者并不意味着服务都会被立即实例化，通常情况下很多是延迟实例化的，只有真的用到它们的时候才会实例化。

如果框架使用者想要使用某个服务，那么必须先对这个服务进行注册。

``` csharp
App.Register(new ConfigProvider());
```

### 初始化优先级

您可以通过配置启动优先级来调整服务提供者启动流程的启动顺序，注意CatLib中的所有的优先级特性都是采用就近原则的。即为函数标记的优先级特性将会优先于类的优先级特性。

``` csharp
using CatLib;
[Priority(200)]
public class ConfigProvider : IServiceProvide
{
    [Priority(100)]
    public void Init()
    {
        // Init 的启动优先级会使用100而不是200 ， 由于就近原则
    }
    public void Register()
    {
        // 注册服务
    }
}
```

关于更多优先级相关资料，请参考：[优先级](application.html#优先级)