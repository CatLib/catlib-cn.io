---
title: 时钟
type: guide
order: 255
enable: true
---

## 时钟

时钟组件通过一个独立的线程模拟了内部Fps周期。

### 初始配置

初始配置必须在框架初始化前完成配置，下面是时钟组件需要的初始配置：

| 配置名                            | 是否必须 | 配置描述(可以点击查看详细)                 |
| -------------------------------- |:------:|:--------------------------------------:|
| `TickProvider.Fps`  | 否      | 时钟每秒调用频率(1-1000) , 默认值为：60 |

### 如何使用

实现`ITick`接口的类并且以`单例形式`在容器中进行绑定。通过`App.Make`构建后会自动加入时钟执行序列。

```csharp
private sealed class TickClass : ITick
{
    public void Tick(int elapseMillisecond)
    {
        // 时钟会定期调用
    }
}
```

```csharp
App.Singleton<TickClass>();
App.Make<TickClass>();
```