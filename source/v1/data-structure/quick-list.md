---
title: 快速列表
---

# 快速列表

快速列表按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边）。

快速列表宏观上是一个双向链表，微观为数组。相对于传统列表(List)，避免了修改造成的大规模的数据迁移。

> 快速列表实现接口:`IQuickList`
> 所有示例中，`TElement`为`string`

## 构造快速列表

```csharp
var quicklist = new QuickList<TElement>();
```

---
##### 函数原型

```csharp
QuickList(int fill = 128);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `fill`            | 每个结点中元素的最大数量      |

## Count

获取列表元素的数量。

```csharp
var count = quicklist.Count;
```

---
##### 函数原型

```csharp
int Count { get; };
```

## Length

快速列表中的结点数量。

```csharp
var length = quicklist.Length;
```

---
##### 函数原型

```csharp
int Length { get; };
```

## Add

将元素插入到列表尾部(`Push`别名函数)。

```csharp
// [ "123" ]
quicklist.Add("hello world");
// [ "123", "hello world" ]
```

---
##### 函数原型

```csharp
void Add(TElement element);;
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `element`            | 需要添加的元素      |

## Push

将元素插入到列表尾部。

```csharp
// [ "123" ]
quicklist.Push("hello world");
// [ "123", "hello world" ]
```

---
##### 函数原型

```csharp
void Push(TElement element); 
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `element`            | 需要添加的元素      |

## UnShift

将元素插入到列表头部。

```csharp
// [ "123" ]
quicklist.UnShift("hello world");
// [ "hello world", "123" ]
```

---
##### 函数原型

```csharp
void UnShift(TElement element); 
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `element`            | 需要添加的元素      |

## Pop

移除并返回列表的尾部元素。

```csharp
// [ "hello world", "123" ]
var element = quicklist.Pop();
// element : "123"
// [ "hello world" ]
```

---
##### 函数原型

```csharp
TElement Pop();
```

## Shift

移除并返回列表头部的元素。

```csharp
// [ "hello world", "123" ]
var element = quicklist.Shift();
// element : "hello world"
// [ "123" ]
```

---
##### 函数原型

```csharp
TElement Shift();
```

## Trim

对列表进行修剪，只保留下标范围内的元素。

```csharp
// [ "1", "2", "3", "4", "5" ]
var count = quicklist.Trim(1, 3); 
// count: 2
// [ "2", "3", "4" ]
```

---
##### 函数原型

```csharp
int Trim(int start, int end);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `start`            | 起始下标(包含)，0为底      |
| `end`              | 结束下标(包含)             |

## Remove

移除指定的元素，如果给定了`count`那么则移除不多于`count`的指定元素。返回值为实际移除的元素数量

```csharp
// [ "a", "b", "a", "b", "a" ]
var num = quicklist.Remove("a", 2);
// num : 2
// [ "b", "b", "a" ]
```

```csharp
// [ "a", "b", "a", "b", "a" ]
var num = quicklist.Remove("a", -2);
// num : 2
// [ "a", "b", "b" ]
```

---
##### 函数原型

```csharp
int Remove(TElement element, int count = 0);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `element`            | 规定元素      |
| `count`              | 移除的数量，为`0`则移除全部元素，正数则从表头搜索，负数从表尾搜索            |

## GetRange

获取区间内的所有元素。

```csharp
// [ "a", "a", "c", "d", "e"]
var elements = quicklist.GetRange(1, 3);
// elements: [ "a", "c", "d" ]
```

---
##### 函数原型

```csharp
TElement[] GetRange(int start, int end);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `start`            | 起始位置(包含)      |
| `end`              | 结束位置(包含)            |


## this[int]

通过下标访问元素,如果传入的是一个负数下标那么从末尾开始查找。

```csharp
// [ "a", "b", "c", "d", "e"]
var element = quicklist[2];
// element : "c"
```

---
##### 函数原型

```csharp
TElement this[int index] { get; set; }
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `index`            | 下标，负数则从末尾查找。      |

## InsertAfter

在指定元素之后插入。

```csharp
// [ "a", "b", "b", "c", "d" ]
quicklist.InsertAfter("b", "abc");
// [ "a", "b", "abc", "b", "c", "d" ]
```

---
##### 函数原型

```csharp
void InsertAfter(TElement finder, TElement insert);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `finder`            | 查找的元素。      |
| `insert`            | 插入的元素。      |

## InsertBefore

在指定元素之前插入。

```csharp
// [ "a", "b", "b", "c", "d" ]
quicklist.InsertAfter("b", "abc");
// [ "a", ""abc"", "b", "b", "c", "d" ]
```

---
##### 函数原型

```csharp
void InsertAfter(TElement finder, TElement insert);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `finder`            | 查找的元素。      |
| `insert`            | 插入的元素。      |

## ReverseIterator

反转迭代顺序（不是从物理上反转整个列表）

```csharp
// 物理顺序：[ 'a', "b", "c" ]
// foreach迭代：[ 'a', "b", "c" ]
quicklist.ReverseIterator();
// 物理顺序：[ 'a', "b", "c" ]
// foreach迭代：[ 'c', "b", "a" ]
```

---
##### 函数原型

```csharp
void ReverseIterator();
```

## Clear

清空快速列表。

```csharp
// [ "a", "b", "c" ]
quicklist.Clear();
// [ ]
```

---
##### 函数原型

```csharp
void Clear();
```

## First

获取第一个元素。

```csharp
// [ "a", "b", "c" ]
var element = quicklist.First();
// element : "a"
// [ "a", "b", "c" ]
```

---
##### 函数原型

```csharp
TElement First();
```

## Last

获取最后一个元素。

```csharp
// [ "a", "b", "c" ]
var element = quicklist.Last();
// element : "c"
// [ "a", "b", "c" ]
```

---
##### 函数原型

```csharp
TElement Last();
```