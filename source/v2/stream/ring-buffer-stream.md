---
title: 环型缓冲区
---

# 环型缓冲区

环型缓冲区是一种用于表示一个固定尺寸、头尾相连的缓冲区的数据结构，适合缓存数据流。

## 构造环型缓冲区

```csharp
var ringBuffer = new RingBufferStream();
```

---
##### 函数原型

```csharp
RingBufferStream(int capacity = 8192, bool exposable = true);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `capacity`            | 环状缓冲区的最大容量，为2的次方。如：传入12，则实际生效为16      |
| `exposable`           | 是否可以访问内部数组      |

## Capacity

缓冲区的实际最大容量。

```csharp
var capacity = ringBuffer.Capacity;
```

---
##### 函数原型

```csharp
int Capacity { get; }
```

## WriteableCount

可以被写入的容量。

```csharp
var writeableCount = ringBuffer.WriteableCount;
```

---
##### 函数原型

```csharp
int WriteableCount { get; }
```

## ReadableCount

可以被读取的容量。

```csharp
var readableCount = ringBuffer.ReadableCount;
```

---
##### 函数原型

```csharp
int ReadableCount { get; }
```

## GetBuffer

获取原始缓冲区，如果`exposable`为`false`则会引发一个`UnauthorizedAccessException`异常。

```csharp
ringBuffer.GetBuffer();
```

---
##### 函数原型

```csharp
byte[] GetBuffer();
```

## Read

读取环状缓冲区的数据。

```csharp
var read = ringBuffer.Read(buffer, offset, count);
```

---
##### 函数原型

```csharp
int Read(byte[] buffer, int offset, int count);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 读取到的缓冲区      |
| `offset`            | `buffer`缓冲区的起始偏移量      |
| `count`             | 希望读取的长度      |

## Peek

读取环状缓冲区的数据，但是不前移读取位置。

```csharp
var read = ringBuffer.Peek(buffer, offset, count);
```

---
##### 函数原型

```csharp
int Peek(byte[] buffer, int offset, int count);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 读取到的缓冲区      |
| `offset`            | `buffer`缓冲区的起始偏移量      |
| `count`             | 希望读取的长度      |

## Write

将数据写入到环型缓冲区，返回值为实际写入的长度。

```csharp
var write = ringBuffer.Write(buffer, 0, buffer.Length);
```

---
##### 函数原型

```csharp
int Write(byte[] buffer, int offset, int count);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 希望写入到环型缓冲区的数据      |
| `offset`            | `buffer`缓冲区的起始偏移量      |
| `count`             | 写入的长度      |

## Clear

清空环型缓冲区。

```csharp
ringBuffer.Clear();
```

---
##### 函数原型

```csharp
void Clear();
```