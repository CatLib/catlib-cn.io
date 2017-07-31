---
title: Mono Driver
type: guide
order: 300
---

## Mono Driver

Mono Driver 为非继承自MonoBehavior的类提供类模拟MonoBehavior的行为。

> 所有的Mono Driver增强接口都支持[优先级](application.html#优先级)特性

### 自动被驱动的条件

您定义的类需要满足下面条件才会被自动被MonoDriver挂载执行。

- 类必须通过`Container`进行`Make`
- 类在`Container`必须以单例进行绑定
- 类必须实现必须的MonoDriver提供的增强接口

#### **手动挂载**

对于非单例，自己手动New出的对象，如果需要挂在到MonoDriver执行，那么必须手动使用Attach接口进行挂载。

``` csharp
var driver = App.Make<IMonoDriver>();
driver.Attach(/*you object*/);
```

> 注意，如果您使用了手动挂载，您必须自己管理卸载。使用`Detach`可以卸载挂载的对象。

### 增强接口

下表描述了MonoDriver可以被实现的增强接口以及对应Unity Monobehavior的关系。

| 增强接口        | 对应MonoBehavior行为 |
| -------------- |:--------------------:|
| `IUpdate`      | `Update`             |
| `ILateUpdate`  | `LateUpdate`         |
| `IFixedUpdate` | `FixedUpdate`        |
| `IOnGUI`       | `OnGUI`              |
| `IOnDestroy`   | `OnDestroy`          |

### 在主线程中运行

您可以通过`MainThread`将一个执行调度到主线程执行。

``` csharp
var driver = App.Make<IMonoDriver>();
driver.MainThread(()=>{});
```

### 协同执行

MonoDriver 允许开发者定义的组件使用Unity提供的协同调度服务。通过`StartCoroutine`您可以执行一个协同程序

``` csharp
var driver = App.Make<IMonoDriver>();
driver.StartCoroutine(()=>{ yield return null; });
```

同理，通过`StopCoroutine`来终止一个协同。