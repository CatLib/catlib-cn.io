---
title: 版本说明
type: guide
order: 2
---

## 版本说明

> CatLib的版本标准是采用：[Semver语义化版本标准](http://semver.org/lang/zh-CN/)

### V1.0 Beta

#### **新增组件**

- `MonoDriver` 组件用于模拟Monobehavior驱动行为
- `Events` 为事件系统。
- `Debugger` 为调试控制器组件
- `Json` 解析Json的类库
- `Translation` 国际化翻译组件
- `Env` 环境组件
- `Converters` 转换器组件

#### **新增的特性**

- `Application.Register` 在完成初始化后可以继续注册
- 新增 `Container.Flush` 函数用于清空容器
- `Container` 允许自定义注入标记
- 可以以Unity组件的方式来书写服务提供者了
- 新增版本对比接口`Application.Compare`
- `Config`新增`Watch`函数支持，用于观察配置的变化
- 对其他框架开发者更加友好 
- `路由系统` 的 `Dispatcher`增加了多线程调用安全支持
- `Container` 增加了`Type2Service`来更加明确Type到服务名的转化

#### **新增全局接口**

- `IAwait` 用于等待异步加载的服务
- `IServiceProvider` 服务提供者接口

#### **行为变化的函数**

- `ServiceProvider.Init` 由异步逻辑变更为同步逻辑。如果服务需要异步等待请使用`IAWait`
- `Container.Make` 第二个参数拆分至 `Container.MakeWith`
- `Application.Register` 要求传入的是一个具体实例而不是一个Type
- `Application.Bootstrap` 要求传入的是一个具体实例而不是一个Type

#### **函数迁移**

- `Application.Attach` 函数迁移至 `MonoDriver` 组件
- `Application.Detach` 函数迁移至 `MonoDriver` 组件

#### **函数/事件更名**

- `Application.GetPriorities` 更名为 `Application.GetPriority`
- `Application.GetGuid` 更名为 `Application.GetRuntimeId`
- `App.Instance` 更名为 `App.Handler`
- `Timer.OnComplete` 更名为 `Timer.OnCompleted`
- 事件 `Application.OnStartComplete` 更名为 `Application.OnStartCompleted`

#### **组件/函数移除**

- 移除了 `ServiceProvider` 抽象类
- 移除了 `Container.ReleaseAll`接口
- 移除了旧的事件系统
- 移除了`Config.AddLocator`函数
- 移除了`IStart`增强接口

### V0.8.3 Beta

- 修复了 `UnitySettingLocator` 无法正确存储及访问的问题
- 修复了 `Application.Version` 不一致的问题

### V0.8.2 Beta

- 修复了由于 `FileSystemManager` 门面模型的配置错误引发的bug

### V0.8.1 Beta

- 修复了版本名不一致的问题
- 修复了遗漏注册的文件服务提供者
- 修复了一个bug，这个bug会导致在路径`/`和`\`混用时无法正确定位
- 修复了一个bug，这个bug导致在没有给定`AssetPath`时不能返回默认的路径

### V0.8.0 Beta

> 本版本为 CatLib 预发行Beta版

- 新增CatLib核心
- 新增标准库 - 容器
- 新增标准库 - 有序集
- 新增标准库 - 快速列表
- 新增标准库 - 最少使用缓存
- 新增标准库 - 过滤器链
- 新增路由组件
- 新增文件系统
- 新增配置组件
- 新增时间组件
- 新增计时器组件
