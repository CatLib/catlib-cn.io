---
title: 有序集
---

# 有序集

有序集合和集合一样也是元素的集合,且不允许重复的成员。不同的是每个元素都会关联一个可以被排序的分数。

有序集合是通过分数来为集合中的成员进行从小到大的排序。有序集合的成员是唯一的,但分数却可以重复。

> 下面的示例代码中:`abc/10`表示`TElement`为`abc`,`TSource`为`10`

## 构造有序集

```csharp
var sorset = new SortSet<string, int>();
```

---
##### 函数原型

```csharp
SortSet<TElement,TSource>(double probable = 0.25, int maxLevel = 32);
```

```csharp
SortSet<TElement,TSource>(IComparer<TScore> comparer, double probable = 0.25, int maxLevel = 32);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `probable`            | 可能出现层数的概率系数(0-1之间的数)(如：0.25即是25%)      |
| `maxLevel`           | 最大层级，默认值`32`      |
| `comparer`           | 自定义比较器，用于比较`TSource`      |

## Count

获取有序集中元素的数量。

```csharp
var count = sorset.Count;
```

---
##### 函数原型

```csharp
int Count { get; }
```

## SyncRoot

同步锁。

```csharp
lock(sortset.SyncRoot)
{
}
```

---
##### 函数原型

```csharp
object SyncRoot { get; }
```

## Clear

清空有序集

```csharp
// [ "1/0", "2/1", "3/2"]
sortset.Clear();
// [ ]
```

---
##### 函数原型

```csharp
void Clear();
```

## Add

添加一个元素到有序集合，或者如果它已经存在则更新其分数，并排序到正确的位置上。

```csharp
// [ "1/0", "2/5", "3/10" ]
sortset.Add("abc", 3);
// [ "1/0", "abc/3", "2/5", "3/10" ]
```

---
##### 函数原型

```csharp
void Add(TElement element, TScore score);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `element`            | 插入的元素      |
| `score`           | 用于排序的分数      |


## Contains

判断有序集中是否存在指定元素。

```csharp
// [ "a/0", "b/5", "c/10" ]
var exists = sortset.Contains("b");
// exists : true
```

---
##### 函数原型

```csharp
bool Contains(TElement element);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `element`            | 指定元素      |

## GetScore

返回有序集中指定元素的分数，如果元素不存在则引发`KeyNotFoundException`。

```csharp
// [ "a/0", "b/5", "c/10" ]
var score = sortset.GetScore("b");
// score : 5
```

---
##### 函数原型

```csharp
TScore GetScore(TElement element);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `element`            | 指定元素      |

## GetRangeCount

获取分数区间内的元素数量。(包含`min`或`max`)

``` csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var num = sortset.GetRangeCount(10, 100);
// num : 2
```

---
##### 函数原型

```csharp
int GetRangeCount(TScore start, TScore end);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `start`            | 起始分数(包含)      |
| `end`              | 结束分数(包含)      |

## Remove

移除有序集合中的指定的元素，如果移除失败则返回`false`。

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var result = sorset.Remove("c");
// result : true
// [ "a/0", "b/10", "d/101" ]
```

---
##### 函数原型

```csharp
bool Remove(TElement element);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `element`            | 指定元素      |

## RemoveRangeByRank

根据指定的`排名范围`移除范围内的所有元素，并返回移除元素的数量。(包含`startRank`或`stopRank`)

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var count = sorset.RemoveRangeByRank(1, 2);
// count : 2
// [ "a/0", "d/101" ]
```

---
##### 函数原型

```csharp
int RemoveRangeByRank(int startRank, int stopRank);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `startRank`            | 起始排名，以0为底，包含自身起始排名      |
| `stopRank`             | 结束排名，以0为底，包含自身结束排名      |

## RemoveRangeByScore

根据指定的`分数范围`移除范围内的所有元素，并返回移除元素的数量。(包含`startScore`或`stopScore`)

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var count = sorset.RemoveRangeByRank(10, 100);
// count : 2
// [ "a/0", "d/101" ]
```

---
##### 函数原型

```csharp
int RemoveRangeByScore(TScore startScore, TScore stopScore);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `startScore`            | 起始分数，包含起始分数      |
| `stopScore`             | 结束分数，包含结束分数      |

## GetRank

获取指定元素的排名 , 有序集成员默认按照Score`从小到大`排序（如果自定义比较函数则按照自定义比较函数顺序为主）。如果返回`-1`则表示没有找到元素。

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var rank = sorset.GetRank("c");
// rank : 2
```

---
##### 函数原型

```csharp
int GetRank(TElement element);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `element`            | 指定元素      |

## GetRevRank

获取指定元素的反向排名 , 有序集成员默认按照Score`从大到小`排序（如果自定义比较函数则按照自定义比较函数的反向顺序为主）。如果返回`-1`则表示没有找到元素。

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var rank = sorset.GetRevRank("c");
// rank : 1
```

---
##### 函数原型

```csharp
int GetRevRank(TElement element);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `element`            | 指定元素      |

## GetElementRangeByRank

根据`排名`获取范围内的元素列表，排序按照排名从小到大（给定自定义比较函数则按照自定义比较函数顺序）。(包含`startRank`或`stopRank`)

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var elements = sorset.GetElementRangeByRank(1, 2);
// elements[0] : "b/10"
// elements[1] : "c/95"
```

---
##### 函数原型

```csharp
TElement[] GetElementRangeByRank(int startRank, int stopRank);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `startRank`            | 起始的排名，排名以0为底，包含自身      |
| `stopRank`             | 结束的排名，排名以0为底，包含自身      |

## GetElementRangeByScore

根据`分数`获取范围内的元素列表，排序按照排名从小到大（给定自定义比较函数则按照自定义比较函数顺序）。(包含`startScore`或`stopScore`)

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var elements = sorset.GetElementRangeByScore(10, 100);
// elements[0] : "b/10"
// elements[1] : "c/95"
```

---
##### 函数原型

```csharp
TElement[] GetElementRangeByScore(TScore startScore, TScore stopScore);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `startScore`            | 起始的分数，包含自身      |
| `stopScore`             | 结束的分数，包含自身      |

## GetElementByRank

根据排名获取元素，排序按照排名从小到大（给定自定义比较函数则按照自定义比较函数顺序）

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var element = sorset.GetElementByRank(2);
// element : "c/95"
```

---
##### 函数原型

```csharp
TElement GetElementByRank(int rank);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `rank`                  | 排名，排名以0为底      |

## GetElementByRevRank

根据排名获取元素，排序按照排名从大到小（给定自定义比较函数则按照自定义比较函数的相反顺序）

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var element = sorset.GetElementByRevRank(2);
// element : "b/10"
```

---
##### 函数原型

```csharp
TElement GetElementByRevRank(int rank);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `rank`                  | 排名，排名以0为底      |

## GetIterator

获取迭代器，需要传入一个值决定是否是前序遍历。

```csharp
// 物理顺序：[ "a/0", "b/10", "c/95", "d/101" ]
// foreach顺序：[ "a/0", "b/10", "c/95", "d/101" ]
sorset.GetIterator();

// 物理顺序：[ "a/0", "b/10", "c/95", "d/101" ]
// foreach顺序：[ "d/101", "c/95", "b/10", "a/0" ]
sorset.GetIterator(false);
```

---
##### 函数原型

```csharp
void GetIterator(bool forward = true);
```

## First

获取第一个元素。

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var element = sorset.First();
// element : "a/0"
```

---
##### 函数原型

```csharp
TElement First();
```

## Last

获取最后一个元素。

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var element = sorset.Last();
// element : "d/101"
```

---
##### 函数原型

```csharp
TElement Last();
```

## Shift

移除并返回有序集头部的元素。

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var element = sorset.Shift();
// element : "a/0"
// [ "b/10", "c/95", "d/101" ]
```

---
##### 函数原型

```csharp
TElement Shift();
```

## Pop

移除并返回有序集尾部的元素。

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var element = sorset.Pop();
// element : "d/101"
// [ "a/0", "b/10", "c/95" ]
```

---
##### 函数原型

```csharp
TElement Pop();
```

## this[int]

获取指定排名的元素。

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var element = sorset[2];
// element : "b/10"
```

---
##### 函数原型

```csharp
TElement this[int rank] { get; }
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `rank`                  | 排名，排名以0为底      |

## ToArray

将有序集转为数组。

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
var elements = sorset.ToArray();
// elements : [ "a", "b", "c", "d" ]
```

---
##### 函数原型

```csharp
TElement[] ToArray();
```