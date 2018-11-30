---
title: 静态线程
---

# 静态线程

静态线程库放置了一些`[ThreadStatic]`标记的线程间静态的数据。

## Buffer

缓冲区，线程间安全，缓冲区的大小为`4096`。

```csharp
var buffer = ThreadStatic.Buffer;
```

---
##### 函数原型

```csharp
byte[] Buffer { get; }
```