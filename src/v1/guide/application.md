---
title: CatLib核心
type: guide
order: 102
---

## CatLib核心

`Application`是CatLib程序的核心，也是所谓的程序入口。应用程序通过引导来加载服务提供者和其他一些必须的资源。应用程序在一般情况下只允许启动一个，且只能在主线程中启动。

### 启动流程

`启动引导` -> `初始化` -> `完成`

启动引导一般用于加载服务提供者，初始配置或者一些其他资源，在引导过程中请不要通过框架生成任何服务（除非您非常熟悉框架的启动流程），否则您可以使用到一个未经注册的服务。

### 初始配置

初始配置必须在框架初始化前完成配置，下面是CatLib核心需要的初始配置：

| 配置名                            | 是否必须 | 配置描述(可以点击查看详细)                |
| -------------------------------- |:------:|:--------------------------------------:|
| `env.debug`                      | 否      | [框架调试等级](#环境)                    |
| `env.asset.path`                 | 否      | [资源文件路径](#环境)                    |

### 从零开始

现在我们将从零开始引导catlib框架启动以便于您能了解框架启动原理，事实上在真实项目中我们已经帮您写好了引导程序。

首先我们需要定义个一个引导程序，引导程序必须继承自`IBootstrap`接口，我们可以在引导程序中对服务提供者进行注册或者，执行其他引导业务。一般情况下我们建议优先完成服务提供者的注册。注意您只有在初始化之前能注册服务提供者，一旦框架初始化那么调用`Register()`将会导致一个异常。

``` csharp
public class GameBootstrap : IBootstrap
{
    public void Bootstrap()
    {
        App.Instance.Register(typeof(CoreProvider));
    }
}
```

随后我们需要一个入口程序，用于被Unity挂载并触发启动流程。在入口程序中，我们只需要实例一个CatLib应用程序，并设定对应引导程序（引导程序会根据设置顺序加载）.最后再执行初始化。

> 注意如果您的使用的是`CatLib.dll`会存在构建跨程序集问题，所以您需要提供`OnFindType()`方法来帮助框架获取当前程序集的类型。

``` csharp
public class Program : MonoBehaviour 
{
    public void Awake()
    {
        var application = new Application(this);
        application.OnFindType((t) => Type.GetType(t));
        application.Bootstrap(new Type[]{ typeof(GameBootstrap) });
        application.Init(()=>
        {
            // 框架启动完成
        });
    }
}
```

### 从CatLib.Unity开始

除去上文描述的方案外，CatLib已经为开发者准备好了引导程序，在CatLib.Unity项目中您可以通过配置`Providers.cs`文件来设定框架的服务提供者。

CatLib.Unity默认的用户代码入口在`Main.cs`文件中，当然你也可以同过路由标记来指定启动入口(这需要您删除Main.cs文件,事实上Main.cs的入口也是通过路由完成的)。

通过路由来标记启动入口,入口的uri固定为`bootstrap://main`：

``` csharp
[Routed]
public class Main
{
    [Routed("bootstrap://main")]
    public void Bootstrap()
    {
        //todo: user code here
        Debug.Log("hello world! user code here!");
    }
}
```

### 主线程调用

CatLib核心提供了子线程代码块调度到主线程执行的能力。这在游戏系统中是非常常用的。

``` csharp
App.Instance.MainThread(()=>
{
    //主线程中执行的代码块
});
```

当然您可也可以通过`IsMainThread`来判断是否处于主线程中。

``` csharp
var isMainThread = App.Instance.IsMainThread;
```

### 挂载执行

挂载执行允许您非继承自`MonoBehaviour`的类拥有`MonoBehaviour`的一部分能力

您需要实现任意一个增强接口才能进行挂载，可以被实现的增强接口有:`IUpdate`,`ILateUpdate`,`IDestroy`

``` csharp
public class Element : IUpdate
{
    public void Update()
    {
        //todo
    }
}
```

``` csharp
var ele = new Element();
App.Instance.Attach(ele);
```

一个对象只能被挂载一次，但您可以通过卸载来卸载挂载的对象：

``` csharp
App.Instance.Detach(ele);
```

除了手动挂载外，如果您已经将服务绑定到容器，且绑定类型为单例绑定，那么一旦您实现了增强接口，那么框架会自动帮您挂载执行，同理当您释放的时候也会自动进行卸载。

> 注意，只有当您的服务是单例的，实现了增强接口，且是通过容器进行生成，才会自动挂载（如果您自己`new`的对象可不会自动挂载哦）

### 协同

CatLib允许非继承自`MonoBehaviour`的类通过这个接口来启动一个协同程序。

``` csharp
App.Instance.StartCoroutine(/* todo: 你的协同函数 */);
```

### 优先级

CatLib所有的增强接口都支持优先级特性。您可以使用`[Priority()]`来定义优先级。

定义：在CatLib中优先级值越小越优先。

> 如果其他组件也使用了优先级那么她们的也遵循CatLib优先级的定义。

### 全局事件

CatLib核心提供的全局事件系统，您只需要通过简单的API就可以调用。

#### **触发全局事件**

``` csharp
App.Instance.Trigger("event");
```

#### **监听全局事件**

监听全局事件时允许您传入触发次数，如果您没有传入则表示事件一直有效，反正如果达到触发，则事件自动被撤销。

``` csharp
App.Instance.On("event" , (sender , e)=>
{
    // 接受到事件
}, 10)
```

#### **撤销监听全局事件**

``` csharp
var handler = App.Instance.On("event" , (sender , e)=>
{
    // 接受到事件
});
handler.Cancel();
```

#### **监听一次全局事件**

``` csharp
var handler = App.Instance.One("event" , (sender , e)=>
{
    // 接受到事件
});
```

### 程序内GUID

CatLib核心提供了获取当前核心实例内的唯一GUID。

``` csharp
var guid = App.Instance.GetGuid();
```

### 获取CatLib版本号

您可以通过`Version`获取当前CatLib核心版本号。

``` csharp
var version = App.Instance.Version;
```

### 环境

您可以通过`IEnv`获取环境信息。

``` csharp
var env = App.Instance.Make<IEnv>();
```

#### **SetAssetPath**

您可以通过`SetAssetPath`来设定资源路径，注意资源路径必须可以被写入。

``` csharp
env.SetAssetPath(UnityEngine.Application.PersistentDataPath);
```

#### **AssetPath**

获取被设定的资源路径。默认情况下为：

在编辑器模式下：

- `生产环境`为：`UnityEngine.Application.PersistentDataPath`
- `仿真环境`为：`StreamingAssets`文件夹
- `开发者模式`为：`UnityEngine.Application.dataPath`

脱离编辑器模式：

- 自动设定为：`UnityEngine.Application.PersistentDataPath`

不论在任何模式下，如果开发者手动设定了`SetAssetPath`。那么始终返回开发者设定的，`AssetPath`。

``` csharp
var assetPath = env.AssetPath;
```

#### **SetDebugLevel**

设定CatLib的调试等级。CatLib的调试等级分为：

- `Prod`生产环境
- `Staging`仿真环境
- `Dev`开发者模式
- `Auto`自动模式，在Unity编辑器下为开发者模式，发布后为生产环境。

``` csharp
env.SetDebugLevel(DebugLevels.Prod);
```

#### **DebugLevel**

获取调试等级。

``` csharp
var debugLevel = env.DebugLevel;
```