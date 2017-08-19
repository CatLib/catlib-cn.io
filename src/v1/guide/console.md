---
title: 控制台
type: guide
order: 203
---

## 控制台

CatLib控制台系统允许您在web端对您的设备进行控制，监控，日志输出。

公共的控制台：[console.catlib.io](http://console.catlib.io)

CatLib的日志工具使用了 [RFC 5424](https://www.ietf.org/rfc/rfc5424.txt) 所描述的日志等级。

### 初始配置

初始配置必须在框架初始化前完成配置，下面是控制台组件需要的初始配置：

| 配置名                            | 是否必须 | 配置描述(可以点击查看详细)                 |
| -------------------------------- |:------:|:--------------------------------------:|
| `DebuggerProvider.WebConsoleEnable`     | 否      | 是否启用Web控制台  |
| `UnityDebuggerProvider.UnityConsoleLoggerHandler`  | 否      | 是否启用Unity Log  |
| `DebuggerProvider.ConsoleLoggerHandler`| 否      | 是否启用控制台输出  |
| `DebuggerProvider.WebConsoleHost`       | 否      | Web控制台监听地址  |
| `DebuggerProvider.WebConsolePort`       | 否      | Web控制台监听端口(默认:9478)  |
| `UnityDebuggerProvider.MonitorPerformance`   | 否      | 是否启动性能监控  |
| `UnityDebuggerProvider.MonitorScreen`        | 否      | 是否启动屏幕监控  |
| `UnityDebuggerProvider.MonitorScene`         | 否      | 是否启动场景监控  |
| `UnityDebuggerProvider.MonitorSystemInfo`    | 否      | 是否启动系统信息监控  |
| `UnityDebuggerProvider.MonitorPath`          | 否      | 是否启动路径监控  |
| `UnityDebuggerProvider.MonitorInput`         | 否      | 是否启动输入监控  |
| `UnityDebuggerProvider.MonitorInputLocation`| 否      | 是否启动定位监控  |
| `UnityDebuggerProvider.MonitorInputGyroscope`    | 否      | 是否启动陀螺仪监控  |
| `UnityDebuggerProvider.MonitorInputCompass` | 否      | 是否启动罗盘监控  |
| `UnityDebuggerProvider.MonitorGraphics`      | 否      | 是否启动显卡监控  |

### 公共的控制台服务器

CatLib已经为您部署了公共的控制台服务器，您可以通过[console.catlib.io](http://console.catlib.io)来访问公共控制台，CatLib 的控制台程序是基于您的浏览器发起控制指令，所以哪怕使用的是公共的控制台服务器也不会导致您的资料外泄。

### 内网搭建控制台服务器

除了访问公共的控制台服务器外，您还可以在内网搭建自己的控制台服务器，首先Clone出 [https://github.com/CatLib/console.CatLib.io](https://github.com/CatLib/console.CatLib.io) 控制台项目。

安装必须的[Nodejs](http://nodejs.cn/) , 随后执行下面指令即可启动服务器:

``` shell
npm install
npm run dev
```

#### **编译控制台**

您可以通过 `npm run build` 指令来编译出可以直接使用的html文件。

### 控制台日志搜索

控制台日志搜索允许您输入关键词来搜索对应日志，搜索输入除了常规关键字外还允许输入特殊指令进行搜索：

- 通过`ns@关键字`来对命名空间进行搜索

### 控制台指令执行

控制台允许开发这输入指令，这个指令会使用[路由系统](routing.md)进行调度。（您可以通过输入：`debug://util/echo/helloworld` 来进行回显测试）

您除了可以执行被路由系统识别的指令外控制台还内嵌的如下的常规操作指令：

- 通过`@clear`清屏
- 通过`↑`翻上一条输入的指令
- 通过`↓`翻下一条输入的指令

### 监控搜索

CatLib控制台监控和日志搜索一样内嵌了一些特殊搜索指令和特殊搜索方法：

- 通过`tag@标签名`进行标签搜索
- 通过`;`来分隔多条件搜索

### 打印一条日志

您可以通过`Log`方法来输出日志，Log方法需要3个参数，第一个参数是日志的等级，第二个参数是日志的消息，第三个参数是日志携带的上下文。

``` csharp
var logger = App.Make<ILogger>();
logger.Log(DebugLevels.Debug, "helloworld");
```

CatLib日志系统遵循[RFC 5424](https://www.ietf.org/rfc/rfc5424.txt)描述的日志等级。所以您还可以使用下面这写快捷函数：

- `Debug()` 调试级日志
- `Info()` 信息级日志
- `Notice()` 通知级日志
- `Warning()` 警告级日志
- `Error()` 错误级日志
- `Critical()` 关键级日志
- `Alert()` 警报级日志
- `Emergency()` 紧急级日志
