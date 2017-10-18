---
title: Socket
type: guide
order: 261
enable: true
---

## Socket

Socket组件为您封装了一些常用的网络连接器，如：`TCP`，`KCP`，`WebSocket(即将到来)`等。

> 这是一个底层网络组件您应该使用 [网络组件](network.html) 来管理您的网络链接，除非您对这个组件非常的了解。

### 如何使用

通过`ISocketManager`接口获取Socket管理器，Socket管理器可以用于管理活动的Socket。

```csharp
var socketManager = App.Make<ISocketManager>();
```

### 通过NSP构建

通过向Socket管理器的`Make`方法传入`NSP`（网络服务提供商）来构建出Socket。

Socket组件已经为您定义好了以下NSP：

- tcp://`{host}`:`{port}`
- kcp://`{conv}`@`{host}`:`{port}`


`{host}` : 服务器Host
`{port}` : 服务器端口号
`{conv}` : 会话Id（uint）（部分协议可能需要如：kcp）
  
```csharp
var tcpSocket = socketManager.Make("tcp://127.0.0.1:7777","default-tcp");
```

### 获取已经构建完成的Socket

通过`Get`方法，传入一个`name`来获取一个已经被完成构建的Socket

```csharp
var tcpSocket = socketManager.Get("default-tcp");
```

### 建立链接

您可以通过`Connect`方法发起链接，`Connect`方法是一个异步方法，但返回一个`IAWait`接口可以被用于同步等待完成。

```csharp
var waitStatus = tcpSocket.Connect();
```

`IAwait`作为`Connect`函数的返回值，可能有以下几种状态的返回：

- `IsDone`为`True`意味着内部执行状态终止（可能由于链接完成，但也可能由于异常）
- `Result`为`布尔值`且为`True`代表函数执行成功
- `Result`为`Exception`类型，意味着在执行过程中出现了异常。

### 监听Socket消息

您可以通过`On`方法来监听Socket通讯消息。

能够被监听的消息有：

- `SocketEvents.Connect` : 当链接成功时（载荷为：自身连接器实例）
- `SocketEvents.Disconnect` : 当主动引发断开连接时（载荷为：自身连接器实例）
- `SocketEvents.Message` : 当收到网络消息时（载荷为：网络数据byte[],可能不是接受完全的）
- `SocketEvents.Sent` : 当数据发送完成时（载荷为：自身连接器实例）。
- `SocketEvents.Closed` : 当连接器关闭时（载荷为：自身连接器实例）。
- `SocketEvents.Error` : 当连接器出现异常时（载荷为：Exception）

```csharp
tcpSocket.On(SocketEvents.Message , (data)=>
{
    //todo:
});
```

> 通过`Off`可以对监听的消息进行释放。

### 触发外部定义的Socket事件

通过Trigger可以主动触发Socket事件。

```csharp
tcpSocket.Trigger("Your Events", "hello world");
```

### 发送数据

可以通过`Send`方法来向服务器发送一条数据，`Send`返回一个`IAwait`可以用于同步等待发送完成。

```csharp
var sendData = System.Text.Encoding.Default.GetBytes("hello world");
var wait = tcpSocket.Send(sendData);
```

### 断开Socket链接

您可以通过`Disconnect`方法断开Socket链接。

```csharp
tcpSocket.Disconnect();
```

### 从管理器中释放Socket

您可以通过`Release`方法从管理器中释放链接，释放链接时如果没有断开Socket链接，那么会自动断开。

```csharp
socketManager.Release("default-tcp");
```

### 拓展Socket

您可以通过`Extend`方法来定制自己的Socket。

```csharp
socketManager.Extend(()=>{
    return new KcpConnector(91828,"127.0.0.1" , 7777){
        SendWindow = 256,
        RecvWindow = 128,
        NoDelay = true,
        IntervalTimer = 10,
        Resend = 1,
        CongestionControl = true
    };
}, "my-kcp");
```

```csharp
var kcpSocket = socketManager.Get("my-kcp");
```

### 组件依赖

- [时钟](tick.html)