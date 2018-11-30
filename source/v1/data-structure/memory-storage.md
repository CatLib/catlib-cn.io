---
title: 内存存储
---

# 内存存储

内存存储避免了传统`MemoryStream`对于大块内存的连续占用，内存存储实现接口`IStorage`。

## 构造内存存储

```csharp
var storage = new MemoryStorage();
```

---
##### 函数原型

```csharp
MemoryStorage(int blockBuffer = 4096, int capacity = 16)
        : this(long.MaxValue, blockBuffer, capacity);
```

```csharp
MemoryStorage(long maxMemoryUsable, int blockBuffer = 4096, int capacity = 16);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `blockBuffer`            | 每个区块的内存块大小      |
| `capacity`               | 默认的区块数量，超过这个数量将会以2的次方扩容。      |
| `maxMemoryUsable`        | 最大内存使用限制，超出后将会引发`OutOfMemoryException`异常。      |

## Length

存储数据的长度。

```csharp
var length = storage.Length;
```

---
##### 函数原型

```csharp
long Length { get; };
```

## Locker 

读写锁

```csharp
var locker = storage.Locker;
```

---
##### 函数原型

```csharp
ReaderWriterLockSlim Locker { get; };
```

## Disabled 

内存存储是否已经被释放了。

```csharp
var disabled = storage.Disabled;
```

---
##### 函数原型

```csharp
bool Disabled { get; };
```

```csharp
var storage = new MemoryStorage();
```

## Write 

将指定缓冲区的数据写入到内存存储,数据必定全部写入，如果引发了异常则表示无法被写入。

```csharp
storage.Write(buffer, 0, buffer.Length, 0);
```

---
##### 函数原型

```csharp
void Write(byte[] buffer, int offset, int count, long index = 0);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 需要写入的缓冲区      |
| `offset`            | 缓冲区的偏移量。      |
| `count`             | 写入缓冲区的长度。      |
| `index`             | 内存存储中存储数据的写入起始位置。      |

## Append 

将指定缓冲区的数据追加到内存存储,数据必定全部写入，如果引发了异常则表示无法被写入。

```csharp
storage.Append(buffer, 0, buffer.Length);
```

---
##### 函数原型

```csharp
void Append(byte[] buffer, int offset, int count);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 需要写入的缓冲区      |
| `offset`            | 缓冲区的偏移量。      |
| `count`             | 写入缓冲区的长度。      |

## Read 

读取内存存储的数据到指定缓冲区，返回值为实际读取的长度。

```csharp
var read = storage.Read(buffer, 0, buffer.Length, 0);
```

---
##### 函数原型

```csharp
int Read(byte[] buffer, int offset, int count, long index = 0);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 需要读取的缓冲区      |
| `offset`            | 缓冲区的偏移量。      |
| `count`             | 读取缓冲区的长度。      |
| `index`             | 内存存储中存储数据的开始读取的位置。      |