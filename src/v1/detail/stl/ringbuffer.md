---
title: 环形缓冲区
type: detail
order: 104
enable: true
---

## 环形缓冲区

环型缓冲区是一种用于表示一个固定尺寸、头尾相连的缓冲区的数据结构，适合缓存数据流。

> 快速列表实现接口:`IRingBuffer`

### 构造环型缓冲区

```csharp
var ringBuffer = new RingBuffer();
```

---
##### 函数原型

```csharp
RingBuffer(int capacity = 8192, bool exposable = true);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `capacity`            | 环状缓冲区的最大容量，为2的次方。如：传入12，则实际生效为16      |
| `exposable`           | 是否可以访问内部数组      |

### Capacity

缓冲区的实际最大容量。

```csharp
var capacity = ringBuffer.Capacity;
```

---
##### 函数原型

```csharp
int Capacity { get; }
```

### SyncRoot

同步锁定

```csharp
lock(ringBuffer.SyncRoot)
{
}
```

---
##### 函数原型

```csharp
object SyncRoot { get; }
```

### WriteableCapacity

可以被写入的容量。

```csharp
var writeable = ringBuffer.WriteableCapacity;
```

---
##### 函数原型

```csharp
int WriteableCapacity { get; }
```

### ReadableCapacity

可以被读取的容量。

```csharp
var readable = ringBuffer.ReadableCapacity;
```

---
##### 函数原型

```csharp
int ReadableCapacity { get; }
```

### CanRead

测试是否可以读取指定长度的数据

```csharp
ringBuffer.CanRead(16);
```

---
##### 函数原型

```csharp
bool CanRead(int count = 1);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `count`            | 希望读取的长度      |

### CanWrite

测试是否可以写入指定长度的数据

```csharp
ringBuffer.CanWrite(16);
```

---
##### 函数原型

```csharp
bool CanWrite(int count = 1);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `count`            | 希望写入的长度      |

### GetBuffer

获取原始缓冲区，如果`exposable`为`false`则会引发一个`UnauthorizedAccessException`异常。

```csharp
ringBuffer.GetBuffer();
```

---
##### 函数原型

```csharp
byte[] GetBuffer();
```

### Read

读取环状缓冲区的数据。

```csharp
var data = ringBuffer.Read();
```

---
##### 函数原型

```csharp
byte[] Read();
```

```csharp
int Read(byte[] buffer);
```

```csharp
int Read(byte[] buffer, int offset);
```

```csharp
int Read(byte[] buffer, int offset, int count);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 读取到的缓冲区      |
| `offset`            | `buffer`缓冲区的起始偏移量      |
| `count`             | 希望读取的长度      |

### Peek

读取环状缓冲区的数据，但是不前移读取位置。

```csharp
var data = ringBuffer.Peek();
```

---
##### 函数原型

```csharp
byte[] Peek();
```

```csharp
int Peek(byte[] buffer);
```

```csharp
int Peek(byte[] buffer, int offset);
```

```csharp
int Peek(byte[] buffer, int offset, int count);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 读取到的缓冲区      |
| `offset`            | `buffer`缓冲区的起始偏移量      |
| `count`             | 希望读取的长度      |

### Write

将数据写入到环型缓冲区，返回值为实际写入的长度。

```csharp
var write = ringBuffer.Peek(buffer, 0, buffer.Length);
```

---
##### 函数原型

```csharp
int Write(byte[] buffer);
```

```csharp
int Write(byte[] buffer, int offset);
```

```csharp
int Write(byte[] buffer, int offset, int count);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 希望写入到环型缓冲区的数据      |
| `offset`            | `buffer`缓冲区的起始偏移量      |
| `count`             | 写入的长度      |

### Flush

清空环型缓冲区。

```csharp
ringBuffer.Flush();
```

---
##### 函数原型

```csharp
void Flush();
```