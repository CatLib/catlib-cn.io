---
title: 版本说明
type: guide
order: 2
enable: true
---

## 版本说明

> CatLib的版本标准是采用：[Semver语义化版本标准](http://semver.org/lang/zh-CN/)

### Core 更新指南

### v1.1.1 RC

- 移除了在Unity环境下单元测试的支持 - 所有的基础类库都是可靠的无需在Unity下再进行额外测试。
- 兼容合并(直接使用Framework和Core的)源码导致的类型指定错误
- 对容器所产生的异常提供更加明确的错误提示

### V1.1.0 RC

**开发者体验改善**

- 错误提示对开发者更加友好和直观
- Container在手动给予参数时如果转换失败辣么会抛出异常
- Application事件名变得更有意义
- 鲁棒性增强

**错误修正**

- 错误的函数名修正: `Releases` 修正为 `Release`

**新增功能**

- 增加了启动引导完成事件: `OnBootstraped`
- `Arr` 增加 `IndexOf` 查找函数

**已过时**

- 已过时: `Str.RegexQuote()`

**其他改进**

- 将事件转移至Support（所有using CatLib; 的可以无缝升级）

### V1.0.0 RC

- 依赖注入容器
- 基础事件系统
- 标准库

### Framework 更新指南

### v1.1.0 Beta

- 移除了在Unity环境下单元测试的支持 - 所有的基础类库都是可靠的无需在Unity下再进行额外测试。
- 新增Network组件
- 新增Tick组件
- 新增了Socket组件
- 项目文件结构调整，及程序集更名
- Container，依赖查找对于实现IConvertible的参数会尝试进行转换
- 模版类增加了ContainsExtend方法，可以用于判断拓展是否存在
- SystemTime增加了Utc时间
- `配置组件`和`转换器组件`从核心框架中淘汰至扩展组件库

> 警告：1.0版本Framework使用1.1版本Core，路由机制可能出现转换失败的异常。请升级1.1版本的Framework修复这个问题

### v1.0.0 Beta.2

- bug fixed
- 为未覆盖断言的代码片段增加断言

### V1.0 Beta

#### **新增组件**

- `MonoDriver` 组件用于模拟Monobehavior驱动行为
- `Events` 为事件系统。
- `Debugger` 为调试控制器组件
- `Json` 解析Json的类库
- `Translation` 国际化翻译组件
- `Converters` 转换器组件
- `Random` 随机库
- `Hashing` 哈希库
- `Encryption` 加密库
- `Compress` 压缩库

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

#### **函数/事件/配置更名**

- `Application.GetPriorities` 更名为 `Application.GetPriority`
- `Application.GetGuid` 更名为 `Application.GetRuntimeId`
- `App.Instance` 更名为 `App.Handler`
- `Timer.OnComplete` 更名为 `Timer.OnCompleted`
- 事件 `Application.OnStartComplete` 更名为 `Application.OnStartCompleted`
- 配置名已经被统一调整,涉及到的组件有[调试控制台](console.html),[环境](env.html),[路由](routing.html),[国际化](translation.html)

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
