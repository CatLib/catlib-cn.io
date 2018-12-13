---
title: 字典
---

# 字典

字典辅助方法库允许您以更加便捷的方式对字典进行操作。

## Filter

将输入字典中的每个值传给回调函数,如果回调函数返回 true，则把输入字典中的当前键值对加入结果字典中。

```csharp
var src = // { "a" : "1"} , { "b" : "2" }
Dict.Filter(src, (key, val)=> val == "2"); // { "b" : "2" }
```

---
##### 函数原型

```csharp
IDictionary<TKey, TValue> Filter<TKey, TValue>(IDictionary<TKey, TValue> source, Func<TKey, TValue, bool> predicate);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`            | 规定字典      |
| `predicate`         | 判定回调函数，如果为`true`则将当前键值对加入结果集      |

## Modify

将输入字典中的每个值传给回调函数，回调函数的返回值用于修改元素的值。

```csharp
var src = // { "a" : "1"} , { "b" : "2" }
Dict.Modify(src, (key , val) => "3");
// src:
// { "a" : "3" },
// { "b" : "3" }
```

---
##### 函数原型

```csharp
void Modify<TKey, TValue>(IDictionary<TKey, TValue> source, Func<TKey, TValue, TValue> callback);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`            | 规定字典      |
| `callback`          | 回调函数，回调函数的结果会作为当前键值对的新值      |


## AddRange

将元素批量添加到字典。

```csharp
var src = // { "a" : "1" }, { "b" : "2" }
var added = // { "c" : "3" }
Dict.AddRange(src, added); 
// { "a" : "1" }, { "b" : "2" }, { "c" : "3" }
```

---
##### 函数原型

```csharp
void AddRange<TKey, TValue>(IDictionary<TKey, TValue> source, IDictionary<TKey, TValue> added, bool replaced = true);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`            | 规定字典      |
| `added`             | 需要添加的元素      |
| `replaced`          | 遇到重复是否替换，如果不进行替换遇到重复将会抛出一个异常      |

## Map

将字典值传入用户自定义函数，自定义函数返回的值作为新的字典值，加入到结果集。

与`Modify`函数不同的是，Map不会导致原始字典的值发生修改。

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

---
##### 函数原型

```csharp
IDictionary<TKey, TValue> Map<TKey, TValue>(IDictionary<TKey, TValue> source, Func<TKey, TValue, TValue> callback);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`            | 规定字典      |
| `callback`          | 回调函数，回调函数的结果会作为键值对的新值加入到结果集      |

## Keys

获取字典的键数组。

```csharp
var src = // { "a" : "1" }, { "b" : "2" }
Dict.Keys(src);
// { "a" : "b" }
```

---
##### 函数原型

```csharp
TKey[] Keys<TKey, TValue>(IDictionary<TKey, TValue> source);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`            | 规定字典      |

## Values

获取字典的值数组。

```csharp
var src = // { "a" , "1" }, { "b" , "2" }
Dict.Values(src);
// { "1" , "2" }
```

---
##### 函数原型

```csharp
TKey[] Values<TKey, TValue>(IDictionary<TKey, TValue> source);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`            | 规定字典      |

## Get

使用点（.）来访问深度字典（`IDictionary<string, object>`）。一般性Json的返回结果会被使用深度字典表示。

```csharp
var src = // { "hello" : { "world" : "test" } }
Dict.Get(src, "hello.world"); // test
Dict.Get(src, "hello.none", "null"); // null  
```

---
##### 函数原型

```csharp
object Get(IDictionary<string, object> source, string key, object def = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`            | 规定字典      |
| `key`               | 键，支持使用点(`.`)来进行深度访问      |
| `def`               | 如果值不存在，返回的默认值      |

## Set

使用点（.）来访问深度字典，并为其指定位置设定一个值。

```csharp
var src = // { "hello" : { "world" : "test" } }
Dict.Set(src, "hello.test" , "hello");
// { "hello" : { {"world" : "test"} , { "test" , "hello"} } }
```

---
##### 函数原型

```csharp
void Set(IDictionary<string, object> source, string key, object val);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`            | 规定字典      |
| `key`               | 键，支持使用点(`.`)来进行深度访问      |
| `val`               | 设定的值      |

## Remove

### Remove(IDictionary<,>, Func<,,>)

将输入字典中的每个值传给回调函数，如果回调函数返回 true，则移除字典中对应的元素并返回被移除的元素

```csharp
var src = // { "hello" : "world" }
var result = Dict.Remove(src, (key,value) => true);
// result:  { "hello" : "world" }
// { }
```

---
##### 函数原型

```csharp
KeyValuePair<TKey,TValue>[] Remove<TKey, TValue>(IDictionary<TKey, TValue> source, Func<TKey, TValue, bool> predicate);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`            | 规定字典      |
| `predicate`         | 回调函数，如果为`true`则移除元素      |


### Remove(IDictionary<,>, string)

使用点（.）来访问深度字典，并移除其中指定的值,返回一个`bool`值来指定是否成功

```csharp
var src = // { "hello" : { {"world" : "catlib"} , { "catlib" , "hello"} } }
Dict.Remove(src, "hello.catlib");
// { "hello" : { "world" : "catlib" } }
```

---
##### 函数原型

```csharp
bool Remove(IDictionary<string, object> source, string key);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`            | 规定字典      |
| `key`               | 键，支持使用点（.）来进行深度访问      |