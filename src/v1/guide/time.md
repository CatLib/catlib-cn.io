---
title: 时间
type: guide
order: 501
enable: true
---

## 时间

CatLib时间组件允许建立自己独立的时间体系。

### 默认的时间

默认的时间是基于Unity的时间的简单包装。

### 时间API

下述内容的描述是基于`默认时间`的，如果使用了`扩展时间`，其意义可能发生变化。

#### **Time**

从游戏开始到现在所用的时间(秒)

``` csharp
var time = timeManager.Default.Time;
```

#### **DeltaTime**

上一帧到当前帧的时间(秒)

``` csharp
var deltaTime = timeManager.Default.DeltaTime;
```

#### **FixedTime**

游戏开始到现在的时间（秒）

通过固定时间更新

``` csharp
var fixedTime = timeManager.Default.FixedTime;
```

#### **TimeSinceLevelLoad**

从当前场景开始到目前为止的时间（秒）

``` csharp
var timeSinceLevelLoad = timeManager.Default.TimeSinceLevelLoad;
```

#### **FixedDeltaTime**

固定帧的更新时间

``` csharp
var fixedDeltaTime = timeManager.Default.FixedDeltaTime;
```

``` csharp
timeManager.Default.FixedDeltaTime = 0.2f;
```

#### **MaximumDeltaTime**

能获取的最大帧与帧之间的更新时间

``` csharp
var maximumDeltaTime = timeManager.Default.MaximumDeltaTime;
```

#### **SmoothDeltaTime**

平稳的更新时间，根据前N帧的加权平均值

``` csharp
var smoothDeltaTime = timeManager.Default.SmoothDeltaTime;
```

#### **TimeScale**

时间缩放系数

``` csharp
var timeScale = timeManager.Default.TimeScale;
```

``` csharp
timeManager.Default.TimeScale = 0.5f;
```

#### **FrameCount**

从游戏开始到目前为止的总帧数

``` csharp
var frameCount = timeManager.Default.FrameCount;
```

#### **RealtimeSinceStartup**

从游戏开始到目前为止的总时间（哪怕时间缩放系数为0也会增长）

``` csharp
var realtimeSinceStartup = timeManager.Default.RealtimeSinceStartup;
```

#### **CaptureFramerate**

每秒的帧率

``` csharp
var captureFramerate = timeManager.Default.CaptureFramerate;
```

``` csharp
timeManager.Default.CaptureFramerate = 30;
```

#### **UnscaledDeltaTime**

不计算时间缩放系数的帧与帧之间的更新时间。

``` csharp
var unscaledDeltaTime = timeManager.Default.UnscaledDeltaTime;
```

#### **UnscaledTime**

不考虑时间缩放系数，从游戏开始到目前为止的总时间

``` csharp
var unscaledTime = timeManager.Default.UnscaledTime;
```

### 扩展时间

您可以通过`Extend()`方法拓展出新的时间。

``` csharp
timeManager.Extend(()=>
{
    return new UnityTime();
},"NewTime");
```

通过`Get()`可以获得您拓展的时间。

``` csharp
var timeSystem = timeManager.Get("NewTime");
```