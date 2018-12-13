---
title: 通用
---

# 通用

通用函数库，存放了一些杂项，且很难被分类的辅助函数。

## Encoding

全局编码类型。

```csharp
Util.Encoding;
```

---
##### 函数原型

```csharp
Encoding Encoding { get; set; }
```

## GetPriority

获取被[Priority]标记的优先级数据。

```csharp
int priority = Util.GetPriority(typeof(Test), "MethodName");
```

---
##### 函数原型

```csharp
int GetPriority(Type type, string method = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `type`            | 类型      |
| `method`          | 如果输入值，则识别方法上的优先级标记，反之识别类上的优先级标记。      |

## MakeRandom

构建一个随机数生成器。

```csharp
Random random = Util.MakeRandom();
```

---
##### 函数原型

```csharp
Random MakeRandom(int? seed = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `seed`            | 随机种子，默认值为随机生成。      |

## MakeSeed

随机生成一个随机种子。

```csharp
int seed = Util.MakeSeed();
```

---
##### 函数原型

```csharp
int MakeSeed();
```