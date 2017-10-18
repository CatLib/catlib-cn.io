---
title: 事件系统
type: detail
order: 150
enable: true
---

## 事件系统

事件机制是一种很好的应用解耦方式。CatLib事件系统让我们可以订阅和监听程序中出现的各种事件。

### 名词解释

- `载荷`是指程序调用所附带的上下文信息。不同的调用者所提供的上下文信息各不相同。

### 注册事件和监听器

通过 `App.On` 命令您可以注册一个事件的监听器，监听器接受一个参数`载荷`。

``` csharp
App.On("event.name" , (payload)=>{
    //todo
});
```

#### **通配符事件监听器**

CatLib事件系统允许使用通配符`*`对事件进行匹配。它让你在一个监听器中可以监听到多个事件。

``` csharp
App.On("event.*" , (payload)=>{
    //todo
});
```

#### **返回事件的处理结果**

您可以通过`return`一个具体的结果，来告知事件的调用者。

``` csharp
App.Listen("event.*" , (payload)=>{
    return "call complete";
});
```

#### **终止事件继续传递**

CatLib事件系统支持返回一个false来终止事件的传递。

``` csharp
App.Listen("event.*" , (payload)=>{
    return false;
});
```

#### **设定事件的生命**

您可以通过设定一个生命，来决定事件能够被触发的次数,`On`函数的第三个参数为生命，默认情况下，事件将会永久有效。

``` csharp
App.Listen("event.*" , (payload)=>{ return false; }, 10);
```

### 触发事件

如果要触发事件，您可以通过`App.Trigger`来触发事件。这个函数将会把事件分发到它所有已经注册的监听器上。因为 App 是全局可访问的，所以你可以在项目中的任何地方调用它：

``` csharp
App.Trigger("event.name", null);
```

#### **执行到第一次有效结果后终止**

通过`TriggerHalt`您可以限定这次事件的触发只要监听器返回了处理结果那么就自动终止事件传递。

``` csharp
App.TriggerHalt("event.name");
```

#### **得到事件的处理结果**

事件触发后您可以获取触发的返回值来获取事件监听器的对这次触发事件的处理结果。

``` csharp
object[] result = App.Trigger("event.name", null);
```

``` csharp
object result = App.TriggerHalt("event.name", null);
```

### 解除事件

您可以通过`Off`方法来解除事件。

``` csharp
var handler = App.On("event.*" , (payload)=>{
    //todo
});
handler.Off();
```
