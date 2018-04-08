---
title: 事件系统
type: guide
order: 103
enable: true
---

## 事件系统

事件机制是一种很好的应用解耦方式。CatLib事件系统让我们可以订阅和监听程序中出现的各种事件。事件系统结合[服务容器](container.html)，将提供更加优秀的事件处理方式。

[CatLib核心](application.html)已经默认提供了事件系统，供给全局事件使用。如果您要定义私有范围的事件可以这么做：

``` csharp
var dispatcher = new Dispatcher(); // 构建一个单纯的事件调度器
```

### 名词解释

- `载荷`是指程序调用所附带的上下文信息。不同的调用者所提供的上下文信息各不相同。

### 注册普通监听器

通过 `App.On` 命令您可以注册一个事件的监听器，监听器函数会通过服务容器调用，自动解析并且注入合适的参数。

``` csharp
App.On("event.name" , (IApplication application)=>{
    //todo
});
```

除了通过lambda注册外事件监听器还能够通过对象方法注册，一共接收3个参数，第一个参数为事件名，第二个参数为触发事件后调用的对象，第三个参数为触发事件后调用对象的函数名（可选项），如果您没有传入第三个参数作为函数名，CatLib事件系统会通过[字符串方法分析](/v1/detail/support/str.html#Method)事件名给定一个函数名，这一特性在有完整的事件名规范时将会大大简化代码。

``` csharp
App.On("MyEvent.CallEvent" , cls); // 分析得出的函数名 : CallEvent
```

### 注册通配符监听器

CatLib事件系统允许使用通配符`*`对事件进行匹配。它让你在一个监听器中可以监听到多个事件。

> 注意：通配符事件每次事件调用都会被依次检查，您应该控制他们在项目中的数量。

``` csharp
App.On("event.*" , ()=>{
    //todo
});

// event.catlib
// event.catlib.version
```

### 触发事件

如果要触发事件，您可以通过`App.Trigger`来触发事件。这个函数将会把事件分发到它所有已经注册的监听器上。因为 App 是全局可访问的，所以你可以在项目中的任何地方调用它, CatLib的将使用容器来调度到方法，这意味这您可以直接展开传入的参数。

``` csharp
App.Trigger("event.name", /*Your params*/);
```

### 触发单次事件

通过`TriggerHalt`您可以限定这次事件的触发只要监听器返回了有效处理结果（不为null）那么就自动终止事件传递。

``` csharp
App.TriggerHalt("event");
```

### 返回处理结果

您可以通过`return`一个对象，这个对象将会被返回给调用者。

``` csharp
App.Listen("event" , ()=>{
    return "call complete";
});

App.Listen("event.*" , ()=>{
    return "call all";
});

App.TriggerHalt("event"); // call complete
App.Trigger("event.name"); // object[]{ "call complete" , "call all" }
```

### 监听器是否存在

您可以通过`HasListeners`来判断事件监听器是否存在，该方法接收两个参数，第一个参数为事件名，第二个参数为是否进行严格匹配（可选）, 如果进行严格匹配那么将禁用通配符匹配。

``` csharp
App.Listen("event.*" , ()=>{
    return "call complete";
});

App.HasListeners("event.name", false); // true
App.HasListeners("event.name", true);  // false
App.HasListeners("event.*", true); // true
```

### 终止事件传递

CatLib事件系统支持返回一个false来终止事件的传递。

``` csharp
App.Listen("event" , ()=>{
    return false;
});
```

### 解除监听器

您可以通过`Off`方法来解除事件监听器，该方法支持三种解除方式：

- 如果传入的是字符串(`string`)将会解除对应事件名的所有事件
- 如果传入的是事件对象(`IEvent`)那么解除对应事件
- 如果传入的是其他实例(`object`)会解除该实例下的所有事件

``` csharp
var handler = App.On("event.*" , (payload)=>{
    //todo
});
App.Off(handler);
```
