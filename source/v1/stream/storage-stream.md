---
title: 存储流
---

# 存储流

存储流允许您以`Stream`的方式，来访问实现`IStorage`的存储介质。

## 构建存储流

```csharp
var storageStream = new StorageStream(new MemoryStorage());
```

---
##### 函数原型

```csharp
StorageStream(IStorage storage, bool writable = true, int timeout = 1000);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `storage`            | `IStorage`存储介质      |
| `writable`           | 是否是可写的      |
| `timeout`            | 锁定超时时间      |

## Position

流的偏移量。

```csharp
var position = storageStream.Position;
```

---
##### 函数原型

```csharp
long Position { get; set; }
```

## Length

存储介质的大小

```csharp
var length = storageStream.Length;
```

---
##### 函数原型

```csharp
long Length { get; }
```

## CanWrite

是否可以向存储介质写入数据

```csharp
var writeable = storageStream.CanWrite;
```

---
##### 函数原型

```csharp
bool CanWrite { get; }
```

## CanRead

是否可以从存储介质读取数据

```csharp
var readable = storageStream.CanRead;
```

---
##### 函数原型

```csharp
bool CanRead { get; }
```

## CanSeek

是否可以进行游标偏移。

```csharp
var seekable = storageStream.CanSeek;
```

---
##### 函数原型

```csharp
bool CanSeek { get; }
```

## Disposed

存储流是否已经被释放。

```csharp
var disposed = storageStream.Disposed;
```

---
##### 函数原型

```csharp
bool Disposed { get; }
```

## Seek

偏移游标到指定位置。

```csharp
storageStream.Seek(0, SeekOrigin.Begin);
```

---
##### 函数原型

```csharp
long Seek(long offset, SeekOrigin origin);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `offset`            | 指定的偏移位置      |
| `origin`           | 偏移方向      |

## Write

写入数据到存储介质。

```csharp
storageStream.Write(buffer, 0, buffer.Length);
```

---
##### 函数原型

```csharp
void Write(byte[] buffer, int offset, int count);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 写入的缓冲区      |
| `offset`           | 写入缓冲区的偏移量      |
| `count`           | 写入的长度      |

## Read

从存储介质读取数据。返回值为实际读取的长度。

```csharp
storageStream.Read(buffer, 0, buffer.Length);
```

---
##### 函数原型

```csharp
int Read(byte[] buffer, int offset, int count);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 读取的缓冲区      |
| `offset`           | 读取缓冲区的偏移量      |
| `count`           | 希望读取的长度      |