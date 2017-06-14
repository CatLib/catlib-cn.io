---
title: 服务接口
type: guide
order: 103
---

## 服务接口

服务接口是指框架提供的一系列定义核心组件服务的接口(下面简称:服务)。如：`IRouter` 接口定义了路由服务可以被使用的方法。

每一个接口框架都有提供相应实现。例如，CatLib为文件系统提供了多种驱动实现。

所有的CatLib服务都在简单的接口中定义，很容易来判断给定服务所提供功能，程序接口也可以充当框架特性的简明文档。

CatLib所有核心接口全部放于`CatLib.API`命名空间下。

### 何时使用

#### **松耦合**

在这个类中，代码和给定文件系统实现紧密耦合（这是一个`反面例子`），由于我们需求一个具体实现类，如果组件的API变了，那么相应的，我们的代码必须做修改。

``` csharp
public class Service
{
    private FileSystem localFileSystem;
    public Service()
    {
        localFileSystem = new FileSystem();
    }
}
```

类似的，如果我们想要替换底层的文件服务为别的技术实现，我们将再一次不得不修改我们的代码来适应新的文件服务。

所以为了避免上述情况，我们可以基于一种简单的、与提供者无关的接口来优化我们的代码，从而替代上述那种实现：

``` csharp
public class Service
{
    [Inject]
    public IFileSystem LocalFileSystem { get; set; }
}
```

现在代码就不与任何特定提供者耦合，甚至与 CatLib 都是无关的。由于API不包含任何实现和依赖，你可以轻松的为给定接口编写可替换的实现代码，你可以随意替换文件服务实现而不用去修改任何高层消费代码。

#### **简明**

此外，基于简单接口，代码也更容易理解和维护。

在一个庞大而复杂的类中，与其追踪哪些方法是有效的，不如转向简单、干净的接口。

### 如何使用

您可以为属性选择器标注`[Inject]`或者在类的`构造函数`中定义变量类型为接口类型。

``` csharp
public class Service
{
    [Inject]
    public IFileSystem LocalFileSystem { get; set; }

    public Service(IFileSystem fileSystem)
    {
        //构造函数 和 属性选择器 都可以被注入
    }
}
```

### 接口列表

下面列出了Catlib核心中可以被直接`Application.Make()`的接口列表(适用于CatLib.Unity):

| 接口名                | 接口描述      |
| -------------------- |:------------:|
| `IConfigManager`     | 全局配置     |
| `IEnv`               | 环境        |
| `IFileSystemManager` | 文件系统     |
| `IRouter`            | 路由        |
| `ITimeManager`       | 时间        |
| `ITimerManager`      | 计时器      |