---
title: 有序集
type: detail
order: 100
---

## 有序集

有序集合和集合一样也是元素的集合,且不允许重复的成员。不同的是每个元素都会关联一个可以被排序的分数。有序集合是通过分数来为集合中的成员进行从小到大的排序。有序集合的成员是唯一的,但分数却可以重复。

### 可用方法

- [Add](#Add)
- [Clear](#Clear)
- [Count](#Count)
- [Contains](#Contains)
- [First](#First)
- [GetElementByRank](#GetElementByRank)
- [GetElementByRevRank](#GetElementByRevRank)
- [GetElementRangeByRank](#GetElementRangeByRank)
- [GetElementRangeByScore](#GetElementRangeByScore)
- [GetScore](#GetScore)
- [GetRank](#GetRank)
- [GetRevRank](#GetRevRank)
- [GetRangeCount](#GetRangeCount)
- [Last](#Last)
- [Pop](#Pop)
- [Remove](#Remove)
- [RemoveRangeByRank](#RemoveRangeByRank)
- [RemoveRangeByScore](#RemoveRangeByScore)
- [ReverseIterator](#ReverseIterator)
- [Shift](#Shift)
- [ToArray](#ToArray)

### Add

添加一个`element`到有序集合，或者如果它已经存在则更新其分数，并排序到正确的位置上。

``` csharp
sortset.Add("this is element" , 10);
```

### Clear

清空有序集中的所有元素。

``` csharp
sortset.Clear();
```

### Count 

获取有序集的`element`数量。

``` csharp
var num = sortset.Count;
```

### Contains

判断有序集中是否存在`element`。

``` csharp
var isExists = sortset.Contains("this is element");
```

### First

获取有序集中的第一个元素。如果有序集为空则引发`InvalidOperationException`异常。

``` csharp
var element = sortset.First();
```

### GetElementByRank

根据排名获取元素 (有序集成员按照Score从小到大排序)

``` csharp
var element = sortset.GetElementByRank(10);
```

### GetElementByRevRank

根据排名获取元素 (有序集成员按照Score从大到小排序)

``` csharp
var element = sortset.GetElementByRevRank(10);
```

### GetElementRangeByRank

根据`排名`获取范围内的元素列表，排序按照排名从小到大。(默认包括等于`min`或`max`)

``` csharp
var list = sortset.GetElementRangeByRank(10,20);
```

### GetElementRangeByScore

根据`分数`获取范围内的元素列表,排序按照分数从小到大。(默认包括等于`min`或`max`)

``` csharp
var list = sortset.GetElementRangeByScore(100,200);
```

### GetScore

返回有序集中`element`的分数，如果`element`不存在则返回`default(TScore)`。

``` csharp
var score = sortset.GetScore("this is element");
```

### GetRank

获取`element`的排名 , 有序集成员按照Score`从小到大`排序。如果返回-1则表示没有找到元素。

``` csharp
var rank = sortset.GetRank("this is element");
```

### GetRevRank

获取`element`的排名 , 有序集成员按照Score`从大到小`排序。如果返回-1则表示没有找到元素。

``` csharp
var rank = sortset.GetRevRank("this is element");
```

### GetRangeCount

获取分数区间内的元素数量。(默认包括等于`min`或`max`)

``` csharp
var num = sortset.GetRangeCount(10,100);
```

### Last

获取有序集中的最后一个元素。如果有序集为空则引发`InvalidOperationException`异常。

``` csharp
var element = sortset.Last();
```

### Pop

移除并返回有序集尾部的元素。如果有序集为空则引发`InvalidOperationException`异常。

``` csharp
var element = sortset.Pop();
```

### Remove

移除有序集合中的指定的`element`，如果移除失败则返回`false`。

``` csharp
var isSuccess = sortset.Remove("this is element");
```

### RemoveRangeByRank

根据指定的`排名范围`移除范围内的所有`element`，并返回移除元素的数量。(默认包括等于`min`或`max`)

``` csharp
int remove = sortset.RemoveRangeByRank(10，20);
```

### RemoveRangeByScore

根据指定的`分数范围`移除范围内的所有`element`，并返回移除元素的数量。(默认包括等于`min`或`max`)

``` csharp
int remove = sortset.RemoveRangeByScore(100，200);
```

### ReverseIterator

反转迭代器迭代顺序。

> 注意，这并不是真正意义反转整个有序集，只是反转了迭代顺序。

``` csharp
sortset.ReverseIterator();
```

### Shift

移除并返回有序集头部的元素。如果有序集为空则引发`InvalidOperationException`异常。

``` csharp
var element = sortset.Shift();
```

### ToArray

将有序集转为数组。

``` csharp
var array = sortset.ToArray();
```