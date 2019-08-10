---
title: 事件系统
---

# 事件系统

事件机制是一种很好的应用解耦方式。CatLib事件系统让我们可以订阅和监听程序中出现的各种事件。

[应用程序](../architecture/application.html)已经默认提供了事件系统，供给全局事件使用。如果您要定义私有范围的事件可以这么做：

``` csharp
var dispatcher = new EventDispatcher();
```

## 名词定义

- `载荷`是指程序调用所附带的上下文信息。不同的调用者所提供的上下文信息各不相同。

## 注册普通监听器

通过 `AddListener` 命令您可以注册一个事件的监听器。

``` csharp
dispatcher.AddListener("event.name", (EventArgs args) =>
{
    //todo
});
```

## 触发事件

如果要触发事件，您可以通过`Dispatch`来触发事件。这个函数将会把事件分发到它所有已经注册的监听器上。

``` csharp
dispatcher.Dispatch("event.name", userParams);
```

## 判断监听器是否存在

您可以通过`HasListener`来判断事件监听器是否存在。

``` csharp
dispatcher.HasListener("event.name");
```

## 终止事件传递

CatLib事件系统支持实现一个`IStoppableEvent`接口来决定是否来终止事件的传递。

``` csharp
public class FooEventArgs : EventArgs, IStoppableEvent
{
    public bool IsPropagationStopped()
    {
        return true;
    }
}
```

## 解除事件监听

您可以通过`RemoveListener`方法来解除指定的事件监听器。

``` csharp
dispatcher.RemoveListener("event.name", listener);
```

#### 解除事件下全部监听器

```csharp
dispatcher.RemoveListener("event.name");
```