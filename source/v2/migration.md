---
title: 迁移指南
---

# 迁移指南

[RFC 2119](https://www.ietf.org/rfc/rfc2199.txt) 中的必须(MUST)，不可(MUST NOT)，建议(SHOULD)，不建议(SHOULD NOT)，可以/可能(MAY)等关键词将在本节用来做一些解释性的描述。

## 从 1.4 迁移到 2.0

1.4 到 2.0 是一个大版本更新，更新中存在不向下兼容的部分。涉及到类更替，命名空间结构调整，过时类删除等。

#### 事件系统

我们对事件系统进行了重构，对于事件系统的命名发生了变化。

| 1.x 事件名                | 2.0 事件名                 |    2.0 事件对象              |
| ------------------------ |:--------------------------:|:---------------------------:|
| `OnBootstrap`            | `OnBeforeBoot`             | `BeforeBootEventArgs`       |
| `Bootstrapping`          | `OnBooting`                | `BootingEventArgs`          |
| `OnBootstraped`          | `OnAfterBoot`              | `AfterBootEventArgs`        |
| `OnRegisterProvider`     | `OnRegisterProvider`       | `RegisterProviderEventArgs` |
| `OnInit`                 | `OnBeforeInit`             | `BeforeInitEventArgs`       |
| `OnIniting`              | 没有替代品                  | 无                          |
| `OnProviderInit`         | `OnInitProvider`           | `InitProviderEventArgs`     |
| `OnProviderInited`       | 没有替代品                  | 无                          |
| `OnInited`               | `OnAfterInit`              | `AfterInitEventArgs`        |
| `OnStartCompleted`       | `OnStartCompleted`         | `StartCompletedEventArgs`   |
| `OnTerminate`            | `OnBeforeTerminate`        | `BeforeTerminateEventArgs`  |
| `OnTerminated`           | `OnAfterTerminate`         | `AfterTerminateEventArgs`   |

#### 命名空间迁移

| 组件名称                  | 1.x 命名空间              | 2.0 命名空间                 |
| ------------------------ | ------------------------ |:--------------------------:|
| `依赖注入容器`             | `CatLib`                | `CatLib.Container`         |
| `事件系统`                | `CatLib`                | `CatLib.EventDispatcher`   |
| `通用异常`                | `CatLib`                | `CatLib.Exception`         |
| `实用程序`                | `CatLib.Support`        | `CatLib.Util`              |

#### 其他

- `App.Handler` 被重命名为 `App.That`
- `App.IsRegisted` 被重命名为 `App.IsRegistered`
- `Application.IsRegisted` 被重命名为 `Application.IsRegistered`
- `IVariant` 被 `VariantAttribute` 特性替代
- `RingBuffer` 被 `RingBufferStream` 替代
- `ExcludeFromCodeCoverageAttribute` 被系统 `ExcludeFromCodeCoverageAttribute` 替代
- `IAwait` 接口被系统`Task`替代

#### 被移除，且没有替代品

- `ISortSet` 接口，因为接口不具备实现价值
- `Application.Compare` 版本比较方法
- `Version`版本类
- `Template` 模版支持
- `FilterChain` 过滤器支持
- `Enum` 移除类支持，理由是计划中的移除
- `Container.Flash` 方法，理由是因为过于凌乱
- `Arr.Flash` 方法，理由是因为过于凌乱
- `Dict` 助手类，理由是因为过于凌乱
- `ThreadStatic` 助手类
- `QuickList` 移除快速列表，理由是因为使用量过少。
- `Storage` 移除内存存储支持，理由是因为使用量过少。
- `SystemTime` 移除系统时间助手类，理由是因为过于凌乱
- `ICoroutineInit` 移除迭代器初始化，因为Task可以更加合适的方式进行异步操作
- `priority` 凌乱的优先级概念被移除，加载顺序即优先级顺序
- `Util` 通用助手类被移除，转变为内部使用的Helper。
- `Str.Encoding` 属性被移除
- `IServiceProviderType` 接口被移除，核心类不再提供，迁移到引导库中
- `PipelineStream` 被移除，这是一个实验类，没有达到预期目的。

## 1.x 迁移指南

如果您正在使用低版本的CatLib，请访问：[1.x 迁移指南](../v1/migration.html)
