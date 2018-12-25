---
title: 服务提供者
---

# 服务提供者

服务提供者是`组件`和`CatLib`联系的桥梁。同时也是CatLib启动的中心，所有的服务都是通过服务提供者定义的。

## 名词定义

- `组件` 组件与CatLib没有任何关系，她们可以独立的运行在不同的框架中。
- `服务` 是由服务提供者将由`一个`或者`多个`组件组合而成，并提供一组可以被开发者使用的接口。
- `容器` CatLib 依赖注入容器。

## 架构图

<img src="../imgs/architecture-diagram.svg" alt="architecture-diagram" style="max-width:1200px;">

## 创建服务提供者

服务提供者是用来描述一个服务如何为使用者提供服务的，这些关系常常包括：服务是否是单例的，服务在构建后如何初始化等等。

服务提供者类的命名我们建议以：`Provider`开头，如：

- `ProviderFileSystem.cs`
- `ProviderNetwork.cs`

#### 为什么要使用服务提供者

- 服务提供者类可以从目录结构清晰的明确哪个类是服务注册类，如果不使用服务提供者类，那么开发者无法快速明确哪一个类为框架提供了服务。
- 服务提供者类似于胶水，粘连了服务和框架，依赖注入是通过构造函数自动进行的，这样在服务的开发过程中甚至可以完全不依赖于框架进行开发。
- 服务提供者类的存在使开发者可以更加专注于类本身的逻辑开发，而无需关注如何注册。

#### 通过IServiceProvider来建立服务提供者

您的服务提供者类必须实现`CatLib.IServiceProvider`接口, 接口中包含`Register()`和`Init()`2个方法。

在`Register()`方法中，你`唯一`要做的事情就是`绑定服务实现`到服务容器，不要尝试在其中执行任何其它功能，否则将会引发一个`CodeStandardException`异常。

``` csharp
public class ProviderFileSystem : IServiceProvider
{
    public void Init(){ }
    public void Register()
    {
        App.Singleton<IFileSystem, FileSystem>();
    }
}
```

`Init()`方法会在所有的服务提供者的`Register()`方法执行后执行，这意味着我们可以在`Init()`中访问其他服务提供者所提供的服务。

``` csharp
public class ProviderConfig : IServiceProvider
{
    public void Init()
    { 
        var fileSystem = App.Make<IFileSystem>();
    }
    public void Register(){ }
}
```

#### 通过ServiceProvider来建立

您还可以通过继承`ServiceProvider`来简化服务提供者的构建方式：

``` csharp
public class ProviderConfig : ServiceProvider
{
    public override void Init()
    { 
        // todo: 
    }
}
```

## 注册服务提供者

如果框架的使用者想要使用某个服务，那么必须先对这个服务进行注册：

``` csharp
App.Register(new ProviderFileSystem());
```

> 注册了服务提供者并不意味着服务都会被立即实例化，通常情况下很多是延迟实例化的，只有真的用到它们的时候才会实例化。

## 覆盖服务提供者

通过`CatLib.IServiceProviderType`接口可以覆盖服务提供者，这往往在需要GUI支持的服务提供者中会非常有用。

```csharp
public class ProviderConfig : IServiceProvider
{
    public void Init(){ }
    public void Register(){ }
}
```

```csharp
public class ProviderGUIConfig : IServiceProvider, IServiceProviderType
{
    public void Init(){ }
    public void Register(){ }
    public Type BaseType 
    { 
        get{ return typeof(ProviderConfig); }
    }
}
```

> 例如:在Unity中GUI可视化编辑界面需要替换基础的服务提供者。
> `ProviderGUIConfig` 注册到框架后，会被框架识别为：`ProviderConfig`

## 协同初始化

如果您希望协同初始化，那么需要实现`ICoroutineInit`或者覆盖`ServiceProvider`的`CoroutineInit`函数。

在原生CatLib.Core环境下协同初始化的调度器为同步调度器。在一些其他环境下可能会由环境的调度器来处理。

> 协同初始化是扩展支持，所以需要先实现`IServiceProvider`接口。

```csharp
public class ProviderConfig : IServiceProvider, ICoroutineInit
{
    public void Init(){ }
    public void Register(){ }
    public IEnumerator CoroutineInit()
    {
    }
}
```

## 完整的例子

- 设计服务接口`FileSystem/IFileSystem.cs`:
```csharp
public interface IFileSystem
{
    void HelloWorld();
}
```

- 开发服务实现`FileSystem/FileSystem.cs`:
```csharp
public class FileSystem : IFileSystem
{
    public void HelloWorld()
    {
        Console.WriteLine("hello world");
    }
}
```

- 创建服务提供者`FileSystem/ProviderFileSystem.cs`:
``` csharp
public class ProviderFileSystem : IServiceProvider
{
    public void Init(){ }
    public void Register()
    {
        App.Singleton<IFileSystem, FileSystem>();
    }
}
```

- 创建框架入口文件，`Main.cs`:
```csharp
var application = Application.New();
application.Bootstrap();
application.Register(new ProviderFileSystem());
application.Init();
```

- 使用服务`Main.cs`:
```csharp
App.Make<IFileSystem>().HelloWorld(); // 输出:hello world
```