---
title: 管道流
---

# 管道流

管道流允许同步化的形式在两个不同线程的传递数据。

## 构建管道流

```csharp
var pipestream = new PipelineStream()
```

---
##### 函数原型

```csharp
PipelineStream(int capacity = 4096, int sleep = 1);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `capacity`            | 管道流的容量      |
| `sleep`           | 等待数据时线程`sleep`的时间      |

## OnRead

当读取时触发的事件,传入参数为管道流实例。

```csharp
pipestream.OnRead += (stream)=>{ }
```

---
##### 函数原型

```csharp
event Action<Stream> OnRead;
```

## CanRead

判断管道流是否可以被读取。

```csharp
var readable = pipestream.CanRead;
```

---
##### 函数原型

```csharp
bool CanRead { get; }
```

## CanWrite

判断管道流是否可以被写入。

```csharp
var writeable = pipestream.CanWrite;
```

---
##### 函数原型

```csharp
bool CanWrite { get; }
```

## Position

获取管道流实际传输的位置(流量)（已被读取）。

```csharp
var position = pipestream.Position;
```

---
##### 函数原型

```csharp
long Position { get; }
```

## Length

获取管道流需要传输的总长度。如果为0则表示没有限制。

```csharp
var length = pipestream.Length;
```

---
##### 函数原型

```csharp
long Length { get; }
```

## CanSeek

是否能够设定Position的位置，在管道流中这个值始终返回`false`。

```csharp
pipestream.CanSeek; // false
```

---
##### 函数原型

```csharp
bool CanSeek { get; }
```

## Closed

判断管道流是否已经被关闭。

```csharp
var closed = pipestream.Closed;
```

---
##### 函数原型

```csharp
bool Closed { get; }
```

## SetLength

设定管道流期望传输的总长度（预期长度）。

```csharp
pipestream.SetLength(100);
```

---
##### 函数原型

```csharp
void SetLength(long value);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `value`            | 预期需要传输的长度      |

## Read

读取管道流中的数据,返回值为实际读取的长度。

```csharp
var read = pipestream.Read(buffer, 0, buffer.Length);
```

---
##### 函数原型

```csharp
int Read(byte[] buffer, int offset, int count)
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 读取的缓冲区      |
| `offset`            | 读取缓冲区起始偏移量      |
| `count`             | 期望读取的长度      |

## Write

向管道流中写入数据,如果写入的数据不能完成写入，那么会挂起等待写入完成。

```csharp
pipestream.Write(buffer, 0, buffer.Length);
```

---
##### 函数原型

```csharp
void Write(byte[] buffer, int offset, int count);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `buffer`            | 写入的缓冲区      |
| `offset`            | 写入缓冲区起始偏移量      |
| `count`             | 期望写入的长度      |

## Close

关闭管道流,关闭管道流后不能够再写入数据，但是依旧可以读取完全部数据。

```csharp
pipestream.Close();
```

---
##### 函数原型

```csharp
void Close();
```

## Dispose

释放管道流，释放后将不能够读取和写入。

```csharp
pipestream.Dispose();
```

---
##### 函数原型

```csharp
void Dispose();
```