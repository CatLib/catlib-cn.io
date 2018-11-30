---
title: 时间
---

# 时间

时间辅助函数库提供了一些通用的时间处理方法。

## ToDateTime

将linux时间戳转为DateTime。

```csharp
SystemTime.ToDateTime(1541042051); // 2018/11/1 11:14:11
```

---
##### 函数原型

```csharp
DateTime ToDateTime(int time);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `time`            | linux时间戳      |

## Timestamp

将DateTime转为Linux时间戳。

```csharp
DateTime.Now.ToDateTime(); // 2018/11/1 11:14:11
```

---
##### 函数原型

```csharp
static int Timestamp(this DateTime time);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `time`            | DateTime      |

## UtcTime

Utc时间(1970年)

```csharp
var time = SystemTime.UtcTime;
```

---
##### 函数原型

```csharp
static readonly DateTime UtcTime = new DateTime(1970, 1, 1);
```