---
title: 字典辅助函数(Str)
type: detail
order: 102
enable: true
---

## 字典辅助函数(Str)

字典辅助方法库允许您以更加便捷的方式对字典进行操作。

### 可用方法

- [Filter](#Filter)
- [Modify](#Modify)
- [AddRange](#AddRange)
- [Map](#Map)
- [Keys](#Keys)
- [Values](#Values)
- [Get](#Get)
- [Set](#Set)
- [Remove](#Remove)

### Filter

将输入字典中的每个值传给回调函数,如果回调函数返回 true，则把输入字典中的当前键值对加入结果字典中。

```csharp
var src = // { "a" : "1"} , { "b" : "2" }
Dict.Filter(src, (key, val)=> val == "2"); // { "b" : "2" }
```

### Modify

将输入字典中的每个值传给回调函数，回调函数的返回值用于修改元素的值。

```csharp
var src = // { "a" : "1"} , { "b" : "2" }
Dict.Modify(src, (key , val) => "3");
// src:
// { "a" : "3" },
// { "b" : "3" }
```

### AddRange

将元素批量添加到字典。

```csharp
var src = // { "a" : "1" }, { "b" : "2" }
var added = // { "c" : "3" }
Dict.AddRange(src, added); 
// { "a" : "1" }, { "b" : "2" }, { "c" : "3" }
```

### Map

将字典值传入用户自定义函数，自定义函数返回的值作为新的字典值。

```csharp
var src = // { "a" : "1" }, { "b" : "2" }
Dict.Map(src, (key,val)=> "3");
// src:
// { "a" : "1"},
// { "b" : "2" }
// return:
// { "a" : "3" },
// { "b" : "3" }
```

### Keys

获取字典的键数组。

```csharp
var src = // { "a" : "1" }, { "b" : "2" }
Dict.Keys(src);
// { "a" : "b" }
```

### Values

获取字典的值数组。

```csharp
var src = // { "a" , "1" }, { "b" , "2" }
Dict.Values(src);
// { "1" , "2" }
```

### Get

使用点（.）来访问深度字典（`IDictionary<string, object>`）。一般性Json的返回结果会被使用深度字典表示。

```csharp
var src = // { "hello" : { "world" : "catlib" } }
Dict.Get(src, "hello.world"); // catlib
Dict.Get(src, "hello.none", "null"); // null  
```

### Set

使用点（.）来访问深度字典，并为其指定位置设定一个值。

```csharp
var src = // { "hello" : { "world" : "catlib" } }
Dict.Set(src, "hello.catlib" , "hello");
// { "hello" : { {"world" : "catlib"} , { "catlib" , "hello"} } }
```

### Remove

**将输入字典中的每个值传给回调函数，如果回调函数返回 true，则移除字典中对应的元素并返回被移除的元素**

```csharp
var src = // { "hello" : "world" }
var result = Dict.Remove(src, (key,value) => true);
// result:  { "hello" : "world" }
// { }
```

**使用点（.）来访问深度字典，并移除其中指定的值,返回一个bool值来指定是否成功**

```csharp
var src = // { "hello" : { {"world" : "catlib"} , { "catlib" , "hello"} } }
Dict.Remove(src, "hello.catlib");
// { "hello" : { "world" : "catlib" } }
```
