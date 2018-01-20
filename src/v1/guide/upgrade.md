---
title: 升级指南
type: guide
order: 1
enable: true
---

## 升级指南

[RFC 2119](https://www.ietf.org/rfc/rfc2199.txt) 中的必须(MUST)，不可(MUST NOT)，建议(SHOULD)，不建议(SHOULD NOT)，可以/可能(MAY)等关键词将在本节用来做一些解释性的描述。

### 从1.1 升级到 1.2

> 1.2 版本的改动轻微不兼容与 1.1.x 您需要根据提示修改。

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

### 从1.0 升级到 1.1

这个版本您可以平滑升级。所有需要您手动修正的问题都会主动抛出警告和修正建议。

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
- 配置名已经被统一调整,涉及到的组件有[调试控制台](console.html),[环境](env.html),[路由](routing.html),[国际化](translation.html)