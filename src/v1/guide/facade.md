---
title: 服务门面
type: guide
order: 3204
enable: true
---

## 服务门面

门面为服务容器内绑定的服务提供了`静态`的访问接口。相对于直接使用容器生成服务来使用，门面能在维护时能够提供更加易于测试、灵活、简明的特性。

CatLib所有的内置门面都放置于`Facade`组件中。如果您要使用门面请引用`CatLib.Facade`命名空间。

### 门面的原理

门面就是一个为容器中对象提供访问方式的类。该机制原理由`Facade`类实现。您需要继承自`Facade`类然后设定所需求的模板，这样门面就可以通过容器来获取服务。

``` csharp
public sealed class Router : Facade<IRouter>
{
}
```

这个例子中，这里我们设定了路由系统的门面。

``` csharp
Router.Instance.Dispatch("bootstrap://start");
App.Make<IRouter>().Dispatch("bootstrap://start");
// 以上2条语句等价
```

### 门面类列表(核心)

| 门面名                | 门面描述      |
| -------------------- |:------------:|
| `Compress`           | 压缩解压缩工具  |
| `Socket`             | 套接字管理器     |
| `Network`            | 网络管理器         |
| `Dispatcher`         | 全局事件调度器  |
| `Encrypter`          | 加密器         |
| `FileSystem`         | 文件系统管理器 |
| `Hashing`            | 哈希库        |
| `Router`             | 路由系统      |
| `Json`               | Json工具      |
| `Random`             | 随机库        |
| `I18N`               | 国际化        |

### 门面类列表(Unity)

| 门面名                | 门面描述      |
| -------------------- |:------------:|
| `Time`               | 时间管理器     |
| `Timer`              | 计时器管理器   |
