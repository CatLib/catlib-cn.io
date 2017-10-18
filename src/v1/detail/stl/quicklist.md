---
title: 快速列表
type: detail
order: 101
enable: true
---

## 快速列表

快速列表按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边）。

快速列表宏观上是一个双向链表，微观为数组。相对于传统列表，避免了修改造成的大规模的数据迁移。

### 可用方法

- [Add](#Add)
- [Clear](#Clear)
- [Count](#Count)
- [First](#First)
- [GetRange](#GetRange)
- [InsertAfter](#InsertAfter)
- [InsertBefore](#InsertBefore)
- [Last](#Last)
- [Length](#Length)
- [Pop](#Pop)
- [Push](#Push)
- [Remove](#Remove)
- [ReverseIterator](#ReverseIterator)
- [Shift](#Shift)
- [Trim](#Trim)
- [UnShift](#UnShift)

### Add

将`element`插入到列表尾部。

``` csharp
quicklist.Add("hello world");
```

### Clear

清空快速列表

``` csharp
quicklist.Clear();
```

### Count

获取快速列表的`element`数量。

``` csharp
var num = quicklist.Count;
```

### First

获取快速列表中的第一个元素。如果有序集为空则引发`InvalidOperationException`异常。

``` csharp
var element = quicklist.First();
```

### GetRange

获取区间内的所有`element`。

``` csharp
var range = quicklist.GetRange(10,20);
```

### InsertAfter

在指定`element`之后插入新的`element`。

``` csharp
quicklist.InsertAfter("hello world" , "new hello world");
```

### InsertBefore

在指定`element`之前插入新的`element`。

``` csharp
quicklist.InsertBefore("hello world" , "new hello world");
```

### Last

获取快速列表中的最后一个元素。如果有序集为空则引发`InvalidOperationException`异常。

``` csharp
var element = quicklist.Last();
```

### Length

获取快速列表的`节点`数量。(快速列表宏观上为双向链表，每个节点可以有N个element)

``` csharp
var num = quicklist.Length;
```

### Pop

移除并返回列表尾部的`element`。如果列表为空则引发`InvalidOperationException`异常。

``` csharp
var element = quicklist.Pop();
```

### Push

将`element`插入到列表尾部。(和Add是等价的)

``` csharp
quicklist.Push("hello world");
```

### Remove

根据参数`count`的值，移除列表中与参数`element`相等的元素。
如果 `count > 0`从表头开始向表尾搜索，移除与`element`相等的元素，数量为`count`。
如果 `count = 0`从表头开始向表尾搜索，移除全部与`element`相等的元素。
如果 `count < 0`从表尾开始向表头搜索，移除与`element`相等的元素，数量为`count`。

返回值为被移除元素的数量。

``` csharp
var num = quicklist.Remove("hello world" , 10);
```

### ReverseIterator

反转迭代器迭代顺序。

> 注意，这并不是真正意义反转整个快速列表，只是反转了迭代顺序。

``` csharp
quicklist.ReverseIterator();
```

### Shift

移除并返回列表头部的`element`。如果列表为空则引发`InvalidOperationException`异常。

``` csharp
var element = quicklist.Shift();
```

### Trim

对列表进行修剪，只保留下标范围内的元素（包括min和包括max）。返回值为被移除元素的数量。

``` csharp
quicklist.Trim(10,200);
```

### UnShift

将`element`插入到列表头部。

``` csharp
quicklist.UnShift("hello world");
```

