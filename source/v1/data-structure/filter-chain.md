---
title: 过滤器链
---

# 过滤器链

过滤器链就是将会数据传递到一个任务序列中，过滤器链扮演者流水线的角色，数据在这里被处理然后传递到下一个步骤。

过滤器链可以将复杂的流程分解成多个独立的子任务。每个独立的任务都是可复用的，因此这些任务可以被组合成复杂的流程。

## 构造过滤器链

```csharp
var filterChain = new FilterChain<T>();
```

#### 支持的模版形式

过滤器链支持3种形式的模版重载：

```csharp
new FilterChain<TIn>();
new FilterChain<TIn, TOut>();
new FilterChain<TIn, TOut, TException>();
```

> 后续文档，全部`new FilterChain<TIn>()`模版作为示例

## Add

增加一个处理器到过滤器链中，处理器根据优先级进行排序。

``` csharp
filterChain.Add((data, next)=>
{
},100);
```

---
##### 函数原型

```csharp
IFilterChain<TIn> Add(Action<TIn, Action<TIn>> filter, int priority = int.MaxValue);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `filter`            | 添加的过滤器处理器。      |
| `priority`          | 优先级，值越小越优先。      |

## Do

将数据传入过滤器链并依次执行处理器，回调函数为最终处理的结果。

``` csharp
filterChain.Do("hello-world" , (data)=>
{
    //todo
});
```

---
##### 函数原型

```csharp
void Do(TIn inData, Action<TIn> then = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `inData`            | 初始的输入数据      |
| `then`              | 当所有处理器都执行完成后执行的回调函数，如果处理器遇到异常，这个函数不会被执行。      |

## FilterList

获取过滤器链中的所有处理器。

``` csharp
var list = filterChain.FilterList;
```

---
##### 函数原型

```csharp
Action<TIn, Action<TIn>>[] FilterList { get; }
```