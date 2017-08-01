---
title: 计时器
type: guide
order: 350
---

## 计时器

使用计时器可以快速简单的实现`定时回调`,`重复执行`,`延迟执行`,`间隔执行`等一系列和计时相关的功能。

### 获取计时器管理器

``` csharp
var timerManager = App.Make<ITimerManager>();
```

### 创建一个定时器

您可以通过计时器管理器的`Make()`方法来创建计时器。

> `Make()`函数实质上在内部构建了一个匿名的定时器队列，而队列中只有一个定时器。

``` csharp
var timer = timerManager.Make(()=>
{
    // 计时器需要执行的任务
});
```

### 延迟执行

#### **延迟指定秒数后执行**

您可以使用`Delay()`来延迟指定的秒数后执行定时器的任务。

``` csharp
timer.Delay(3.25f);
```

#### **延迟指定帧数执行**

您可以使用`DelayFrame()`来延迟指定的帧数后执行定时器的任务。

``` csharp
timer.DelayFrame(3.25f);
```

### 循环执行

#### **在指定时间内循环执行**

您可以使用`Loop()`在指定的时长内，每帧都执行定时器任务。

``` csharp
timer.Loop(3.25f);
```

#### **循环执行，直到函数返回false**

您可以使用`Loop()`来定义一个循环执行，每帧都执行，直到函数返回false。

``` csharp
timer.Loop(()=>
{
    return false;
});
```

#### **循环执行指定帧数**

您可以使用`LoopFrame()`来定义一个循环执行，每帧都执行，直到到达指定帧数后停止。

``` csharp
timer.LoopFrame(10);
```

### 间隔执行

如果您要取消间隔执行，那么请使用句柄来取消任务。

#### **每隔指定秒数执行一次**

每次间隔指定秒数后执行一次定时器任务。

``` csharp
timer.Interval(3.25f);
```

#### **每隔指定帧数执行一次**

每次间隔指定帧数后执行一次定时器任务。

``` csharp
timer.IntervalFrame(10);
```

### 定时器队列

定时器队列是将多个定时器串联起来依次执行。只有前一个定时器完成任务后后一个定时器才会被执行。

您可以传入一个`优先级`来决定定时器队列在全局中执行的顺序。

``` csharp
timerManager.MakeQueue(()=>
{
    timerManager.Make(()=>
    {
        //定时器1的任务
    }).Delay(3);
    timerManager.Make(()=>
    {
        //定时器2的任务
    }).Loop(5);
},100);
```

### 取消执行

撤销定时器(队列)的执行。撤销后队列状态可不恢复。

``` csharp
var handler = timerManager.MakeQueue(()=>
{
    // todo
});
timerManager.Cancel(handler);
```

### 暂停执行

暂时停止定时器(队列)的执行。

``` csharp
var handler = timerManager.MakeQueue(()=>
{
    // todo
});
timerManager.Pause(handler);
```

### 恢复执行

恢复定时器(队列)的执行。

``` csharp
var handler = timerManager.MakeQueue(()=>
{
    // todo
});
timerManager.Play(handler);
```