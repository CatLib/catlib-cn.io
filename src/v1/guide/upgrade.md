---
title: 升级指南
type: guide
order: 1
---

## 升级指南

[RFC 2119](https://www.ietf.org/rfc/rfc2199.txt) 中的必须(MUST)，不可(MUST NOT)，建议(SHOULD)，不建议(SHOULD NOT)，可以/可能(MAY)等关键词将在本节用来做一些解释性的描述。

### 从0.8 升级至 1.0

这是一个主版本变化，意味着出现了不向下兼容的更新，我们尽可能保证升级的平滑，但是不可避免将会出现一些改动。

- 移除了 `ServiceProvider` 抽象类 ，您`必须`使用`IServiceProvider`代替
- 移除了 `Container.ReleaseAll`接口 , 您`可能`使用 `Flush` 代替（注意与ReleaseAll不同的是Flush将会清空一切绑定关系）
- 移除了旧的事件系统 , 这意味着您`必须`重新适配新的事件系统接口
- 移除了`Config.AddLocator`函数 ，您`必须`使用`Config.SetLocator`代替 ，由于移除了多数据源的支持，所以您`不可`重复的`SetLocator`
- 移除了`IStart`增强接口 , 您`不可`再使用这个接口。
- `Application.GetPriorities` 更名为 `Application.GetPriority`
- `Application.GetGuid` 更名为 `Application.GetRuntimeId`
- `App.Instance` 更名为 `App.Handler`
- `Timer.OnComplete` 更名为 `Timer.OnCompleted`
- 事件 `Application.OnStartComplete` 更名为 `Application.OnStartCompleted`
- `Application.Attach` 函数迁移至 `MonoDriver` 组件 ， 您`不可`再使用`App.Instance.Attach`
- `Application.Detach` 函数迁移至 `MonoDriver` 组件 ， 您`不可`再使用`App.Instance.Detach`
- `ServiceProvider.Init` 由异步逻辑变更为同步逻辑。如果服务需要异步等待请使用`IAWait`
- `Container.Make` 第二个参数拆分至 `Container.MakeWith` , 如果您的代码对容器构建有传参操作，您`必须`切换至`Container.MakeWith`
- `Application.Register` 要求传入的是一个具体实例而不是一个Type
- `Application.Bootstrap` 要求传入的是一个具体实例而不是一个Type