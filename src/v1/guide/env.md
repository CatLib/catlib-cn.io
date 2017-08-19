---
title: 游戏环境
type: guide
order: 301
---

## 游戏环境

游戏环境组件维护了游戏运行时所必须的环境数据。

### 初始配置

初始配置必须在框架初始化前完成配置，下面是环境组件需要的初始配置：

| 配置名                            | 是否必须 | 配置描述(可以点击查看详细)                |
| -------------------------------- |:------:|:--------------------------------------:|
| `EnvironmentProvider.DebugLevel`                      | 否      | [框架调试等级](#环境)                    |
| `EnvironmentProvider.AssetPath`                 | 否      | [资源文件路径](#环境)                    |

您可以通过`IEnv`获取环境信息。

``` csharp
var env = App.Make<IEnv>();
```

#### **SetAssetPath**

您可以通过`SetAssetPath`来设定资源路径，注意资源路径必须可以被写入。

- 如果传入的值为`null`或者`空字符串`则不视作开发者手动设定。

``` csharp
env.SetAssetPath(UnityEngine.Application.PersistentDataPath);
```

#### **AssetPath**

获取被设定的资源路径。`默认情况`下为：

在编辑器模式下：

- `生产环境`为：`UnityEngine.Application.PersistentDataPath`
- `仿真环境`为：`StreamingAssets`文件夹
- `开发者模式`为：`UnityEngine.Application.dataPath`

脱离编辑器模式：

- 自动设定为：`UnityEngine.Application.PersistentDataPath`

不论在任何模式下，如果开发者手动设定了`SetAssetPath`。那么始终返回开发者设定的，`AssetPath`。

``` csharp
var assetPath = env.AssetPath;
```

#### **SetDebugLevel**

设定CatLib的调试等级。CatLib的调试等级分为：

- `Prod`生产环境
- `Staging`仿真环境
- `Dev`开发者模式
- `Auto`自动模式，在Unity编辑器下为开发者模式，发布后为生产环境。

``` csharp
env.SetDebugLevel(DebugLevels.Prod);
```

#### **DebugLevel**

获取调试等级。

``` csharp
var debugLevel = env.DebugLevel;
```
