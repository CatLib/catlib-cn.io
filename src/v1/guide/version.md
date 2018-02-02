---
title: 版本说明
type: guide
order: 2
enable: true
---

## 版本说明

> CatLib的版本标准是采用：[Semver语义化版本标准](http://semver.org/lang/zh-CN/)

### Core 更新指南

### v1.2.1 Beta

**性能优化**
- `Facade` 拥有更好的性能。
- `App.Make` 拥有更好的性能。

**接口调整**
- `App.Release` 可以获取一个返回值，表示是否成功的释放。

### v1.2.0 Beta

**新增内容**
- App类可以被继承。
- 容器可以为实现 `IDisposable` 接口的对象，在 `Release` 时会自动触发接口。
- 容器增加可变类型接口(`IVariant`)，允许将一个基础类型转为复杂类型。
- 容器增加`Params`数据结构，允许容器进行参数名参数匹配。
- 容器增加 `BindMethod` 方法允许向容器注册一个方法。
- 容器增加 `OnRebound` 和 `Watch` 函数，该函数用于监听当已经解决的服务发生重定义时触发的事件，这个事件对实例绑定的服务也会有效。
- Str辅助函数库增加了Method方法可以获取字符串中符合规则的函数名。
- Arr增加了Flash函数，该函数可以处理只在片段时间内产生作用的内容。
- 增加了字典辅助函数库（Dict）
-- Filter函数可以过滤返回值为false的键值对
-- Remove函数可以移除比较回掉返回为true的元素
-- Modify函数可以直接修改键值对的值
-- AddRange允许批量添加到字典
-- Map函数将回调的返回值作为新的字典
-- Keys函数可以获取字典的键名数组
-- Values函数可以获取字典的值数组
-- Get函数可以通过点状表达式访问深度字典
-- Set函数可以通过点状表达式写入深度字典
-- Remove函数可以通过点状表达式删除深度字典元素
- Arr 增加了 `RemoveAt` 函数，可以通过数组下标移除元素，并且返回被移除的元素值。
- `Container.Call` 支持直接为lambda表达式提供依赖注入。

**更新内容**
- 容器获得了更好的绑定(`Bind`)校验并及时的抛出异常。
- Facade(门面)机制优化， 拥有了更好的访问性能。
- 容器中`string`被认为是不可以被实例的对象。
- 全局事件系统关联服务容器，这意味着所有的函数绑定事件不会再限制参数了，容器会选择最合适的参数自动注入。
- 全局事件移除生命周期支持，因为全局事件的监听不应该频繁发生变化，生命周期功能会诱导开发者进行不正确的操作
- 优化了注入算法，能够尽可能的推导出注入实例。
- `OnResolving` 和 `OnRelease` 所加入的新的修饰器不会再处理已经生成的实例。实例的变化应该使用`Watch`来监控
- 容器注入的策略被调整为必然注入成功（不为null）（以前是可以为null）（如果希望提供默认值必须显示的申明）否则将抛出一个不能解决的异常
- 容器在无法解决注入时可以根据 `@变量名` 来推测类型了。
- `BindIf` API 不再返回已绑定的api，而是返回bool来表示是否成功绑定，`IBindData` 将以out参数的形式返回。
- 全局修饰器的策略调整为即时状态不会再为已经生成的对象进行修饰。因为旧的设计这会导致表达语意不明。
- 对于基础类型的注入变得更加严格，不能在直接注入基础类型的默认值了例如：int的默认值为0，因为我们认为这可能会导致一些不被发现的错误。
- 对容器进行了重构，使容器具备更加好的可拓展性及性能，同时也开放了大量可以直接更改容器行为的虚函数，这对于其他框架开发者或者高级定制的开发者可以更加轻松的修改容器行为。
- 用户参数注入的检查由绝对顺序调整为相对顺序，这意味着参数顺序的要求将会更加松散。

**Bug修复**
- 修复了一个bug这个bug导致在调用Instance时如果存在修饰器修改对象的情况，那么不能返回已经被修改的对象。
- 修复了 `App.Trigger` 返回值不正确的bug。

**移除内容**
- 移除了LRUCache，理由是由于性能不过关。
- 移除了Inject注入标记的Required选项，因为我们认为Required的存在使开发者不再注重对传入值的校验，这对于非catlib下使用组件是存在安全风险的。

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
