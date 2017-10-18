---
title: 过滤器链
type: detail
order: 103
enable: true
---

## 过滤器链

过滤器链就是将会数据传递到一个任务序列中，过滤器链扮演者流水线的角色，数据在这里被处理然后传递到下一个步骤。

过滤器链可以将复杂的流程分解成多个独立的子任务。每个独立的任务都是可复用的，因此这些任务可以被组合成复杂的流程。

### 使用场景

CatLib框架很多地方都是用到了过滤器链，其中最典型的就是路由系统的中间件。

我们需要实现过滤器链的地方绝大多数会处于项目底层的部分。

### 可用方法

- [Add](#Add)
- [Do](#Do)
- [FilterList](#FilterList)

### Add

增加一个处理器到过滤器链中，处理器根据优先级进行排序。

``` csharp
filterChain.Add((data, next)=>
{
    // todo:
},100);
```

### Do

将数据传入过滤器链并依次执行处理器。

``` csharp
filterChain.Do("helloworld" , (data)=>
{
    //todo
});
```

### FilterList

获取过滤器链中的所有处理器。

``` csharp
var list = filterChain.FilterList;
```

