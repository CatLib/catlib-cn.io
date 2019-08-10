---
title: 数组
---

# 数组

数组方法库允许您通过简单的方式访问和操作数组。

## Merge

将多个数组合并成一个数组。

```csharp
Arr.Merge(new []{"1","2"} , new []{"3"}); // ["1" , "2" , "3"]
```
---
##### 函数原型

```csharp
T[] Merge<T>(params T[][] sources);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `sources`     | 规定数组      |

## Rand

从规定数组中获取一个或者指定数量的随机值。

```csharp
var result = Arr.Rand(new []{"1", "2", "3"});
```

> 如果获取的元素数量大于1，那么在随机过程中不会重复获得已经随机到的值
> 如果传入的数量大于提供的元素数量，将会返回`default(T)`空值

---
##### 函数原型

```csharp
T[] Rand<T>(T[] source, int number = 1)
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `sources`     | 规定数组      |
| `number`      | 需要获取的元素数量      |



## Shuffle

将规定数组中的元素打乱(洗牌算法)。

```csharp
var result = Arr.Shuffle(new []{"1", "2", "3"});
```

给定随机种子：

```csharp
var result1 = Arr.Shuffle(new []{"1", "2", "3"}, 100); // ["2", "1", "3"]
var result2 = Arr.Shuffle(new []{"1", "2", "3"}, 100); // ["2", "1", "3"]
```
> 随机种子一致，则同元素顺序多次随机也将会一致。

---
##### 函数原型

```csharp
T[] Shuffle<T>(T[] source, int? seed = null)
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `sources`     | 规定数组      |
| `seed`        | 给定的随机种子      |

## Splice

从数组中移除指定长度的元素，如果给定了替换数组参数，那么替换数组中的元素将会从起始位置开始插入。

```csharp
var data = new []{"1", "2", "3"};
var count = Arr.Splice(ref data , 1 , 1, new []{"4", "5"});
// data : ["1", "4", "5", "3"]
// count : 4
```

---
##### 函数原型

```csharp
T[] Splice<T>(ref T[] source, int start, int? length = null, T[] replSource = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `sources`     | 规定数组      |
| `start`       | 删除元素的开始位置。如果为负数则从后向前计算。  |
| `length`      | 删除元素的个数，也是被返回数组的长度。如果为负数则保留到结尾指定元素数量，如：-1则意味着删除到数组的倒数第一个元素前。  |
| `replSource`  | 在删除起始位置插入的元素。  |

## Chunk

将数组分为新的数组块,其中每个数组的单元数目由大小参数决定。最后一个数组的单元数目可能会少几个。

```csharp
var result = Arr.Chunk(new []{"1", "2", "3"}, 2);
// result : [["1", "2"], ["3"]]
```

---
##### 函数原型

```csharp
T[][] Chunk<T>(T[] source, int size);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`        | 规定数组      |
| `size`       | 每个分块的大小  |

## Fill

对数组进行填充，如果传入了规定数组，那么会在规定数组的基础上进行填充

```csharp
var result = Arr.Fill(2, 3, "100", new []{"1", "2", "3"});
// result : ["1","2","100","100","100","3"]
```

---
##### 函数原型

```csharp
T[] Fill<T>(int start, int length, T value, T[] source = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `start`        | 起始下标      |
| `length`       | 填充长度  |
| `value`        | 填充的值  |
| `source`       | 规定数组  |

## Remove

将数组每个值传给回调函数，如果回调函数返回 true，则移除数组中对应的元素，并返回被移除的元素

```csharp
var data = new string[]{"1", "2", "3"};
var result = Arr.Remove(ref data,(v) => v == "2");
// result : ["2"]
// data: ["1", "3"]
```

---
##### 函数原型

```csharp
T[] Remove<T>(ref T[] source, Predicate<T> predicate);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`        | 规定数组      |
| `predicate`     | 判断函数，如果返回值为`true`则被删除  |


## Filter

输入数组中的每个值传给回调函数,如果回调函数和期望值相同，则把输入数组中的当前值加入结果数组中。

```csharp
var result = Arr.Filter(new[]{"1", "2", "3", "4", "5"},(v) => (v % 2) == 0);
// result : ["2", "4"]
```

---
##### 函数原型

```csharp
T[] Filter<T>(T[] source, Predicate<T> predicate, bool expected = true);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`        | 规定数组      |
| `predicate`     | 判断函数，如果返回值和期望值相同则将结果加入数组  |
| `expected`      | 期望值 | 

## Map

将数组值传入用户自定义函数，自定义函数返回的值作为新的数组值加入到结果集，该操作不会导致原始数组的值发生修改。

```csharp
var result = Arr.Map(new[]{1, 2, 3} , (v) => v * 2);
// result : [2, 4, 6]
```

---
##### 函数原型

```csharp
TReturn[] Map<T, TReturn>(T[] source, Func<T, TReturn> callback);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`        | 规定数组      |
| `callback`      | 处理函数，返回值会被认为是新值  |

---
##### 函数原型

```csharp
TReturn[] Map<T, TReturn>(IEnumerable<T> source, Func<T, TReturn> callback);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`        | 规定迭代器      |
| `callback`      | 处理函数，返回值会被认为是新值  |

## Pop

删除数组中的最后一个元素，并将删除的元素作为返回值返回

```csharp
var data = new []{"1", "2", "3"};
var result = Arr.Pop(ref data);
// result : "3"
// data.Length : 2
```

---
##### 函数原型

```csharp
T Pop<T>(ref T[] source);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`        | 规定数组      |

## Push

将一个或多个元素加入数组尾端。

```csharp
var data = new []{"1","2"};
var count = Arr.Push(ref data, "3","4","5");
// data : ["1","2","3","4" ,"5"]
// count : 5
```

---
##### 函数原型

```csharp
int Push<T>(ref T[] source, params T[] elements);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `elements`        | 需要添加到尾端的元素      |

## Reduce

向用户自定义函数发送数组中的值，并返回一个字符串。
如果数组是空的且未传递初始化参数，该函数返回 null。
如果指定了初始化参数，则该参数将被当成是数组中的第一个值来处理，如果数组为空的话就作为最终返回值(string)。

```csharp
var result = Arr.Reduce(new []{"1", "2", "3"},(v1, v2)=> v1 + "-" + v2, "0");
// result : 0-1-2-3
```

---
##### 函数原型

```csharp
Reduce<T>(T[] source, Func<object, T, string> callback, object initial = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `callback`        | 自定义处理函数，第一个参数为前序处理的结果值，第二个参数为规定数组元素，返回值为字符串      |
| `initial`         | 第一次触发自定义处理函数时，提供的前序处理的初始值      |

## Slice

在数组中根据条件取出一段值，并返回。

```csharp
var result = Arr.Slice(new []{"1", "2", "3", "4", "5"},1,3);
// result : ["2", "3", "4"]
```

---
##### 函数原型

```csharp
T[] Slice<T>(T[] source, int start, int? length = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `start`        | 起始位置，如果为负数则从后向前获取起始位置      |
| `length`        | 截取的长度，如果为负数则从后向前计算终止的位置。      |

## Shift

删除数组中第一个元素，并返回被删除元素的值

```csharp
var data = new []{"1", "2", "3"};
var result = Arr.Shift(ref data);
// result : "1"
// data.Length : 2
```

---
##### 函数原型

```csharp
T Shift<T>(ref T[] source);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |

## Unshift

向数组插入新元素。新数组的值将被插入到数组的开头。

```csharp
var data = new []{"2","3"};
var count = Arr.Unshift(ref data, "0", "1");
// count : 4
// data ["0", "1", "2", "3"]
```

---
##### 函数原型

```csharp
int Unshift<T>(ref T[] source, params T[] elements);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `elements`        | 需要被插入到数组开头的元素      |

## Reverse

以相反的顺序返回数组。

```csharp
var result = Arr.Reverse(new []{"1", "2","3"});
// result : ["3", "2", "1"]
```

---
##### 函数原型

```csharp
T[] Reverse<T>(T[] source, int start = 0, int? length = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `start`           | 起始位置，如果为负数则从后向前获取起始位置      |
| `length`          | 处理的长度(终止位置)，如果为负数则从后向前计算终止的位置。      |

## IndexOf

从数组中检索指定的值并返回所在的下标，如果返回-1则代表没有出现

```csharp
var result = Arr.IndexOf(new []{"1", "2", "3", "4", "5"} , new []{ "2", "3" });
// result : 1
```

---
##### 函数原型

```csharp
int IndexOf<T>(T[] source, params T[] match);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `match`           | 要匹配的值，如果有多个，只有全部的匹配才算匹配      |

## IndexOfAny

从数组中检索指定的值并返回所在的下标，如果返回-1则代表没有出现，与`IndexOf`相比，只要有一个元素匹配则认为匹配成功。

```csharp
var result = Arr.IndexOfAny(new []{"1", "2", "3", "4", "5"} , new []{ "5", "2" });
// result : 1
```

---
##### 函数原型

```csharp
int IndexOfAny<T>(T[] source, params T[] match);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `match`           | 要匹配的值，有一个元素匹配则认为匹配成功      |

## Difference

从数组中排除指定的值

```csharp
var result = Arr.Difference(new []{"1", "2", "3", "4", "5"} , new []{ "2", "3" });
// result : ["1", "4", "5"]
```

---
##### 函数原型

```csharp
T[] Difference<T>(T[] source, params T[] match);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `match`           | 需要排除掉的值      |

## RemoveAt

从数组中移除并返回指定下标的元素。

```csharp
var src = new object[]{ "1", "2", "3" };
var result = Arr.RemoveAt(ref src, 1); // 2
// src : object[] { "1", "3" }
```

---
##### 函数原型

```csharp
T RemoveAt<T>(ref T[] source, int index);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `index`           | 移除的元素下标      |

## Cut

裁剪指定的数组，正数为从前向后裁剪，负数为从尾部裁剪

```csharp
var data = new char[] { '1', '2', '3', '4', '5' };
Arr.Cut(ref data, 1);
// data[0] == '2'
```

尾部裁剪
```csharp
var data = new char[] { '1', '2', '3', '4', '5' };
Arr.Cut(ref data, -2);
// data[0] == '1'
// data[1] == '2'
// data[2] == '3'
// data.length = 3
```

---
##### 函数原型

```csharp
void Cut<T>(ref T[] source, int count);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `count`           | 裁剪范围，负数为从尾部裁剪      |

## Test

将规定数组传递给检查器进行检查。

只有当所有元素通过检查器均为`false`，那么函数返回`false`。

```csharp
var src = new string[]{ "1", "2", "3" };
var result = Arr.Test(src, (item)=> item == "2"); // true
```

---
##### 函数原型

```csharp
bool Test<T>(T[] source, Predicate<T> predicate);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `predicate`           | 检查器      |

## Set

查找规定数组中的指定元素，如果找到了则使用替代值替换，否则在规定数组尾部增加替换值。

```csharp
var set = new[] { "a", "ab", "abc", "abcd", "abcdef" };
Arr.Set(ref set, (element) => element == "none", "hello");
// { "a", "ab", "abc", "abcd", "abcdef", "hello" }
```

---
##### 函数原型

```csharp
void Set<T>(ref T[] source, Predicate<T> predicate, T value);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`          | 规定数组      |
| `predicate`           | 返回true则覆盖当前元素内容      |
| `value`  | 替换(设定)值 |