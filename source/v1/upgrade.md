---
title: 更新日志
---

# 更新日志

> CatLib的版本标准是采用：[Semver语义化版本标准](http://semver.org/lang/zh-CN/)

## 摘要

版本格式：主版本号.次版本号.修订号，版本号递增规则如下：

- 主版本号：当你做了不兼容的 API 修改，
- 次版本号：当你做了向下兼容的功能性新增，
- 修订号：当你做了向下兼容的问题修正。

> 先行版本号及版本编译信息可以加到`主版本号.次版本号.修订号`的后面，作为延伸。

## v1.3.0 Alpha.3

**框架相关**

- 修复初始化顺序不正确的异常，不同运行时环境不一致({% issues 102%} )
- copyright 名称变更为 CatLib({% issues 106%} )
- 为Type增加ToService扩展函数({% issues 105%} )
- 注释中的文档地址更新为 https({% issues 108%} )
- DebugLevels允许在Register中被使用（{% issues 109%} ）
- 提示内容优化({% issues 117%} )
- App门面增加IsResolved(string service)的支持({% issues 118%} )

**服务容器**

- 将IContainer接口缩减，缩减的接口将会被移动到扩展方法({% issues 111%} )
-- Factory
- 语意行为变更：Bind和Singleton相关({% issues 112%} )
- `IContainer.Release`接口参数由`string`变更为`object`,允许通过对象实例来进行释放({% issues 115%} )
- 优化UnresolvableException的异常提示消息 （{% issues 120%} ）
- 允许在Needs时提供需求的变量名，以支持变量名需求，改善变量名上下文关系的流程 ({% issues 122%} )
- 服务容器的构建策略可以通过虚函数覆写（{% issues 124%} ）
- 移除了Inject的别名支持 （{% issues 126%} ）

**其他**

- 清理了无用代码({% issues 119%} )

## v1.3.0 Alpha.2

- Type2Service 将不允许为虚方法。({% issues 96%} )
- 服务提供者的传入顺序为初始化顺序({% issues 97%} )
- Given 允许返回一个闭包 ({% issues 98%} )

## v1.3.0 Alpha.1

**框架**

- 升级为 c# 7.0 语法({% issues 64%})
- 增加`LogicException`逻辑错误异常({% issues 80 %})
- 增加`CodeStandardException`代码规范异常({% issues 62 %})
- `ServiceProvider.Register`不再是`abstract` 
- 增加了`ICoroutineInit` 协同初始化接口(默认的迭代器为同步迭代器和`Init`没有本质区别,在其他环境下可能受到环境影响)
- `ApplicationEvents.OnIniting`被标记为已过时，使用：`ApplicationEvents.OnProviderInit`替换
- 新增了`ApplicationEvents.OnProviderInit` 服务提供者初始化之前
- 新增了`ApplicationEvents.OnProviderInited` 服务提供者初始化完成
- Application Version现在是静态化的了({% issues 90 %})

**代码规范性检查** 

- 如果尝试为同一个单例化的实例注册不同的服务名，则会异常发一个异常({% issues 62 %})
- 如果尝试在注册流程中Make服务，那么会引发一个异常。 ({% issues 59 %})
- 通过容器Watch一个对象时，只有当对象时可以被构建时才允许被watch。因为我们认为观察的只能是一个已经被确定的服务，而不是一个未知服务。 ({% issues 59 %})
- 如果尝试在注册流程中Invoke函数，那么会引发一个异常。({% issues 62 %})

**容器服务**

- 服务容器增加了解决事件和释放事件的条件筛选({% issues 84 %})
- 服务容器增加OnAfterResolving的支持。({% issues 83 %})
- 使用this[] set对象时会进行隐式的对象绑定(bind)({% issues 82 %})
- 增加Extend函数，用于处理服务的覆盖及拓展关系，OnResolving不再允许覆盖对象。({% issues 79 %})
- 服务容器Watch接口从容器实现转移至扩展函数({% issues 81 %})
- 增加服务特殊字符`@`,`$`和`:`识别，如果包含则不允许注册。({% issues 61 %})
- 增加变量标签`$`同`@`一样，具备相同的变量映射作用。({% issues 61 %})
- 容器在`flush`对象时将会按照构建顺序的相反顺序释放服务（之前是需要通过辅助组件来完成）({% issues 55 %})
- 服务容器增加`App.Alias<TService, TAlias>()`的拓展方法支持({% issues 57 %})

**支持库**

- 增加 `SegmentStream` 分片流({% issues 74 %})
- 增加 `WrapperStream` 包装流({% issues 69 %})
- 增加 `CombineStream` 组合流({% issues 70 %})
- `Arr.Filter` 支持迭代器({% issues 67 %})
- `Arr.Map` 支持迭代器({% issues 66 %})
- 增加了`SortSet.ReverseIterator(bool forward)`指定反转方向的接口({% issues 56 %})
- StreamExtension.ToText() 加入对不能读取流的判断({% issues 75 %})
- 管理器模版支持OnAfterResolving({% issues 92 %})

## v1.2.12 Beta

- 增加模版方法的门面静态类
- 修复`Dict.Modify`的bug，这个bug导致不必要的字典实例 {% issues 52 %}
- `DebugLevels` 枚举命名调整
-- Prod标记为已过时
-- Dev标记为已过时
-- 增加 Production
-- 增加 Development
- 尝试重复调用引导(Bootstrap)时会引发一个`RuntimeException`而不是静默忽略。
- 尝试重复调用初始化(Init)时会引发一个`RuntimeException`而不是静默忽略。
- 增加了`ApplicationEvents.OnBootstrap`在引导开始之前
- 增加了`ApplicationEvents.Bootstrapping`在引导进行中
- 增加了`ApplicationEvents.OnInit`在初始化开始之前
- 增加了`ApplicationEvents.OnInited`在初始化完成之后
- 增加了`ApplicationEvents.OnRegisterProvider`注册服务提供者事件
- 允许`ApplicationEvents.Bootstrapping`返回一个不为`null`的值(一般是`false`)来终止当前引导激活。
- 允许`ApplicationEvents.OnRegisterProvider`返回一个不为`null`的值(一般是`false`)来终止服务提供者注册。

## v1.2.11 Beta

- PipelineStream支持SetLength
- 增加ToStream();

## v1.2.10 Beta

- 增加 `Stream.ToText();` 扩展函数
- `Arr.Merge`支持对于`null`元素的合并兼容
- 增加了`PipelineStream` 管道流，可以用于安全的线程间通讯

## v1.2.9 Beta

- `SingleManager` 中的扩展实现，支持释放接口
- `Arr`增加了`IndexOfAny`函数可以用于查找任意匹配的下标
- 管理器模版重构
- 修复了.NetStandard下dll缺失了部分功能的问题
- 增加 `int.ToPrime()` 扩展方法
- IBindData 增加了函数`Tag`，可以用于服务标记
- 新增了 `ThreadStatic` 线程静态变量辅助库

## v1.2.8 Beta

- 新增了 Stream 扩展函数 `AppendTo`
- 修复了 SortSet.GetRangeCount 在一定情况下引发异常的bug
- 增加了内存存储结构 MemoryStorage 
- 增加了存储流 StorageStream
- 修复了Facade中Instance导致40B的GC Alloc

## v1.2.7 Beta

- 移除了引用计数模块
- 修复一个bug，这个bug导致`SortSet`在存储相同的Value时有概率不能正确移除元素， 同等情况下获取排名不正确的问题
- `OnResolving`增加无参数Lambda支持
- 为`RingBuffer`增加了接口
- 删除了不必要的Using

## v1.2.6 Beta

- 增加了新的数据结构，环型缓冲区(RingBuffer)
- 增加了 `Arr.Cut` 裁剪函数，允许裁剪数组
- `SortSet` 支持自定义比较器来进行排序
- 优化了在容器构建发生异常后的错误提示
- 事件系统可以通过字符串注册获取私有的方法了
- 增加ServiceProvider抽象类, 意味着接口的Init将不是必须的了
- `Release`方法在通过类型释放时可以获得没有释放成功的类型了
- 对`Container`性能进行了优化
- 对事件系统进行了性能优化

## v1.2.5 Beta

- 优化了Listen，不在只允许监听返回值为object的函数
- 修复了一个bug，这个bug导致全局事件不能够正确的在Static函数上监听
- 修复了一个bug 这个bug导致如果作为参数筛选器，要求注入的是筛选器类型的话将会导致无效匹配
- Call(Action...)相关函数将支持自定义用户参数传入

## v1.2.4 Beta

- 无用代码清理
- Params特殊参数通过IParams来代替

## v1.2.3 Beta

- 紧急修复了Netstandard Dll库未能正确发布的问题

## v1.2.2 Beta

**新增内容**

- 增加了`Tag<TService>(string)`允许直接将类型标记
- 增加了`Release(params object[]...)`允许通过实例对象进行释放
- 增加了`Terminate()`函数，用于终止CatLib框架
- 增加了`ApplicationEvents.OnTerminate`用于监听框架终止之前的事件
- 增加了`ApplicationEvents.OnTerminated`用于监听框架终止之后的事件

**其他优化**

- `Facade`增加`HasInstance`来优化`App.HasInstance`(这是个内部优化)
- `OnResolving`允许只获取实例
- `OnRelease`允许只获取实例
- `OnResolving`允许不在获取返回值

## v1.2.1 Beta

**性能优化**

- `Facade` 拥有更好的性能。
- `App.Make` 拥有更好的性能。

**接口调整**

- `App.Release` 可以获取一个返回值，表示是否成功的释放。

## v1.2.0 Beta

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

## v1.1.1 RC

- 移除了在Unity环境下单元测试的支持 - 所有的基础类库都是可靠的无需在Unity下再进行额外测试。
- 兼容合并(直接使用Framework和Core的)源码导致的类型指定错误
- 对容器所产生的异常提供更加明确的错误提示

## V1.1.0 RC

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

## V1.0.0 RC

- 依赖注入容器
- 基础事件系统
- 标准库