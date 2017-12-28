---
title: 数组辅助函数(Arr)
type: detail
order: 101
enable: true
---

## 数组辅助函数(Arr)

数组方法库允许您通过简单的方式访问和操作数组。

### 可用方法

- [Merge](#Merge)
- [Rand](#Rand)
- [Shuffle](#Shuffle)
- [Splice](#Splice)
- [Chunk](#Chunk)
- [Fill](#Fill)
- [Remove](#Remove)
- [Filter](#Filter)
- [Map](#Map)
- [Pop](#Pop)
- [Push](#Push)
- [Reduce](#Reduce)
- [Slice](#Slice)
- [Shift](#Shift)
- [Unshift](#Unshift)
- [Reverse](#Reverse)
- [IndexOf](#IndexOf)
- [Difference](#Difference)
- [RemoveAt](#RemoveAt)
- [Flash](#Flash)

### Merge

将多个数组合并成一个数组。

```csharp
Arr.Merge(new []{"1","2"} , new []{"3"}); // ["1" , "2" , "3"]
```

### Rand

从规定数组中获取一个或者指定数量的随机值。

```csharp
var result = Arr.Rand(new []{"1", "2" , "3"});
```

### Shuffle

将规定数组中的元素打乱。

```csharp
var result = Arr.Shuffle(new []{"1" , "2" , "3"});
```

### Splice

从数组中移除指定长度的元素，如果给定了替换数组参数，那么替换数组中的元素将会从起始位置开始插入。

```csharp
var data = new []{"1","2","3"};
var count = Arr.Splice(ref data , 1 , 1,new []{"4" , "5"});
// data : ["1" , "4" , "5" , "3"]
// count : 4
```

### Chunk

将数组分为新的数组块,其中每个数组的单元数目由大小参数决定。最后一个数组的单元数目可能会少几个。

```csharp
var result = Arr.Chunk(new []{"1","2","3"} , 2);
// result : [["1","2"],["3"]]
```

### Fill

对数组进行填充，如果传入了规定数组，那么会在规定数组的基础上进行填充

```csharp
var result = Arr.Fill(2,3,"100",new []{"1","2","3"});
// result : ["1","2","100","100","100","3"]
```

### Remove

将数组每个值传给回调函数，如果回调函数返回 true，则移除数组中对应的元素，并返回被移除的元素

```csharp
var data = new string[]{"1","2","3"};
var result = Arr.Remove(ref data,(v) => v == "2");
// result : ["2"]
// data: ["1","3"]
```

### Filter

输入数组中的每个值传给回调函数,如果回调函数返回 true，则把输入数组中的当前值加入结果数组中。

```csharp
var result = Arr.Filter(new[]{"1", "2" , "3" , "4" , "5"},(v) => (v % 2) == 0);
// result : ["2" , "4"]
```

### Map

将数组值传入用户自定义函数，自定义函数返回的值作为新的数组值。

```csharp
var result = Arr.Map(new[]{1 , 2 , 3} , (v) => v * 2);
// result : [2 , 4 , 6]
```

### Pop

删除数组中的最后一个元素，并将删除的元素作为返回值返回

```csharp
var data = new []{"1","2","3"};
var result = Arr.Pop(ref data);
// result : "3"
// data.Length : 2
```

### Push

将一个或多个元素加入数组尾端。

```csharp
var data = new []{"1","2"};
var count = Arr.Push(ref data, "3","4","5");
// data : ["1","2","3","4" ,"5"]
// count : 5
```

### Reduce

向用户自定义函数发送数组中的值，并返回一个字符串。
如果数组是空的且未传递初始化参数，该函数返回 null。
如果指定了初始化参数，则该参数将被当成是数组中的第一个值来处理，如果数组为空的话就作为最终返回值(string)。

```csharp
var result = Arr.Reduce(new []{"1" , "2" , "3"},(v1,v2)=> v1+"-"+v2,"0");
// result : 0-1-2-3
```

### Slice

在数组中根据条件取出一段值，并返回。

```csharp
var result = Arr.Slice(new []{"1","2","3","4","5"},1,3);
// result : ["2","3","4"]
```

### Shift

删除数组中第一个元素，并返回被删除元素的值

```csharp
var data = new []{"1","2","3"};
var result = Arr.Shift(ref data);
// result : "1"
// data.Length : 2
```

### Unshift

向数组插入新元素。新数组的值将被插入到数组的开头。

```csharp
var data = new []{"2","3"};
var count = Arr.Unshift(ref data , "0" , "1");
// count : 4
// data ["0" , "1" , "2" , "3"]
```

### Reverse

以相反的顺序返回数组。

```csharp
var result = Arr.Reverse(new []{"1" , "2" ,"3"});
// result : ["3" , "2" , "1"]
```

### IndexOf

从数组中检索指定的值并返回所在的下标，如果返回-1则代表没有出现

```csharp
var result = Arr.IndexOf(new []{"1" , "2" ,"3" , "4" , "5"} , new []{ "2" , "3" });
// result : 1
```

### Difference

从数组中排除指定的值

```csharp
var result = Arr.Difference(new []{"1" , "2" ,"3" , "4" , "5"} , new []{ "2" , "3" });
// result : ["1","4","5"]
```

### RemoveAt

从数组中移除并返回指定下标的元素。

```csharp
var src = new object[]{ "1" , "2" , "3" };
var result = Arr.RemoveAt(ref src, 1); // 2
// src : object[] { "1" , "3" }
```

### Flash

临时性的回调元素，如果遇到异常或者完成回调后会进行回滚元素回调

```csharp
var src = new object[]{ "a", "b" , "c" };
Arr.Flash(src,
(element)=> Console.WriteLine("start:" + element),
(element)=> Console.WriteLine("rollback:" + element), 
()=> { Console.WriteLine("Completed"); });
// start:a
// start:b
// start:c
// Completed
// rollback:c
// rollback:b
// rollback:a
```