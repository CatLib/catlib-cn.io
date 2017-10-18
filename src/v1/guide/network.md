---
title: 网络
type: guide
order: 260
enable: true
---

## 网络组件

网络组件是基于Socket组件的更高级的封装，为您提供了简单的API以实现：`心跳包`，`协议解包` 等处理。一般情况下您应该直接使用网络组件。

### 使用网络组件

通过`INetworkManager`接口获取网络管理器，网络管理器可以用于管理活动的`网络频道`。

```csharp
var networkManager = App.Make<INetworkManager>();
```

### 生成一个网络频道

通过`Make`方法可以生成一个网络频道，Make方法接受一个`NSP`(网络服务提供商)来确定使用的应用层协议和传输层协议。

Network组件已经为您定义好了以下`NSP`：

- tcp://`{host}`:`{port}`
- kcp://`{conv}`@`{host}`:`{port}`
- text.tcp://`{host}`:`{port}`
- text.kcp://`{conv}`@`{host}`:`{port}`
- frame.tcp://`{host}`:`{port}`
- frame.kcp://`{conv}`@`{host}`:`{port}`

```csharp
var channel = networkManager.Make("text.tcp://127.0.0.1:7777", "my-connect");
```

> `text协议` : 协议格式为 `数据包`+`换行符(\n)`，即在每个数据包末尾加上一个换行符表示包的结束。

> `frame协议` : 协议格式为 `总包长`+`包体`，其中包长(总包长+包体)为4字节网络字节序的整数，包体可以是普通文本或者二进制数据。

您还可以通过`Make`的重载来强制指定使用的`打包解包协议`：

> 所有的打包解包协议必须实现`IPacker`接口。

```csharp
var channel = networkManager.Make("tcp://127.0.0.1:7777", new TextPacker(), "my-connect");
```

### 网络事件

您可以通过网络事件来跟踪网络中的数据以及状态的变化以便于即使做出处理。以下是您可以通过IChannel监听的网络事件：

- `OnConnected` : 当网络链接完成后会触发这个事件，事件参数为：网络频道
- `OnError` : 当网络遇到错误时触发这个事件，事件参数为：网络频道，错误的异常
- `OnMessage` : 当接受到完整的（被解包）的数据包后触发这个事件，事件参数为：网络频道，解包后的数据包
- `OnDisconnect` : 当主动断开链接时触发（由于异常断开的链接不会触发），事件参数为：网络频道
- `OnClosed` : 当链接关闭时，事件参数为：网络频道，什么异常导致的关闭（null则正常关闭）
- `OnSent` : 当发送完成时，事件参数为：网络频道，实际发送的字节流
- `OnMissHeartBeat` : 当丢失心跳包时，事件参数为：网络频道，丢失的次数

```csharp
channel.OnMessage += (from , packet)=>{
    // todo:
};
```

全局网络事件(通过`INetworkManager`监听)除了以上事件可以被监听外还有：

- `OnReleased` ：当某个网络频道被释放时，事件参数为：被释放的网络频道

> 全局网络事件意味着可以监听所有网络频道所产生的事件。

```csharp
networkManager.OnClosed += (from , ex)=>{
    // todo:
};
```

### 设定网络频道的心跳包

您可以通过网络频道的`SetHeartBeat`来设定心跳包的间隔，如果在这个间隔内没有网络通讯来往，则开始触发心跳丢失事件。

函数所需求的参数为`检查间隔`，单位为：`毫秒(MS)`

> 注意触发心跳丢失事件本质上不会对链接有任何影响，您应该在应用层对这个事件做出正确的处理。

```csharp
channel.SetHeartBeat(15000);
```

### 链接到服务器

您可以通过`Connect`函数来链接到服务器，由于您在`NSP`中已经提供了对端服务器的地址，所以您可以直接链接。

函数返回的时一个`标准等待接口（IAwait）`您可以根据这个接口来判断是否已经完成了链接。

如果IAwait提供的参数 `IsDone` 和 `Result` 均为 `True` 则完成了链接，否则 `Result` 中返回异常

```csharp
var wait = channel.Connect();
```

如果您需要更改`NSP`所提供的初始化参数，那么您可以使用`Connect`的重载来替换服务器的地址。

```csharp
var wait = channel.Connect("127.0.0.1" , 8888);
```

### 断开服务器链接

您可以通过`Disconnect`来主动断开服务器链接。

```csharp
channel.Disconnect();
```

如果您为`Disconnect`传入一个异常那么在关闭事件中，这个异常会被附带。

以异常方式断开链接会触发一些事件，事件执行顺序：

- `OnError`
- `OnDisconnect`
- `OnClosed`

```csharp
channel.Disconnect(new Exception("closed"));
```

### 发送数据到服务器

您可以使用`Send`来发送数据包。

> 注意如果`打包解包协议`无法打包那么会直接抛出一个异常。

```csharp
channel.Send(Encoding.Default.GetBytes("helloworld"));
```

### 获取网络频道的名字

您可以通过`Name`属性来获取网络频道的名字

```csharp
var name = channel.Name;
```

### 获取基础套接字

您可以通过`Socket`属性来获取基础套接字。

```csharp
var socket = channel.Socket;
```

### 使用IPacker接口

`IPacker`为打包解包器，您可以自定义打包解包规范。接口需要3个函数：

> **int Input(byte[], out Exception)**
> 检查包的完整性, 如果能够得到包长，则返回包的在buffer中的长度(包含包头)，否则返回0继续等待数据。
> 如果协议有问题，则填入ex参数，当前连接会因此断开

<span></span>

> **byte[] Encode(object, out Exception)**
> 序列化消息包。
> 如果协议有问题，则填入ex参数，当前连接会因此断开。
> 如果需要抛弃数据包则返回null

<span></span>

> **object Decode(byte[], out Exception)**
> 反序列化消息包(包体)。
> 如果协议有问题，则填入ex参数，当前连接会因此断开
> 如果需要抛弃数据包则返回null。

### 组件依赖

- [Socket](socket.html)
- [时钟](tick.html)