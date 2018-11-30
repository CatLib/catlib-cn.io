---
title: 组合流
---

# 组合流

组合流允许将多个不同的流组合成一个流，这两个流可以分别为流式传输和非流式传输。

## 构建组合流

```csharp
var combineStream = new CombineStream()
```

---
##### 函数原型

```csharp
CombineStream(Stream left, Stream right, bool closed = false);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `left`             | 左侧的流      |
| `right`            | 右侧的流      |
| `closed`           | 是否在流读取完成后自动关闭流      |


```csharp
CombineStream(Stream[] source, bool closed = false);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`             | 多个流的数组，按照数组顺序排列      |
| `closed`             | 是否在流读取完成后自动关闭流      |

## Length

获取组合流的总长度，如果组合流中的任意一个流不能被获取长度，那么将会引发一个`NotSupportedException`。

```csharp
var length = combineStream.Length;
```

---
##### 函数原型

```csharp
long Length { get; }
```

## CanSeek

判断组合流是否能够偏移，如果组合流中的任意一个流不能够偏移，那么返回`false`。

```csharp
var canSeek = combineStream.CanSeek;
```

---
##### 函数原型

```csharp
bool CanSeek { get; }
```

## Position

获取组合流全局的偏移量

```csharp
var position = combineStream.Position;
```

---
##### 函数原型

```csharp
long Position { get; }
```

## CanRead

判断管道流是否可以被读取，如果组合流中的任意一个流不能够读取，那么返回`false`。

```csharp
var readable = combineStream.CanRead;
```

---
##### 函数原型

```csharp
bool CanRead { get; }
```

## CanWrite

判断管道流是否可以被写入。管道流永远返回`false`。

```csharp
var writeable = combineStream.CanWrite;
```

---
##### 函数原型

```csharp
bool CanWrite { get; }
```

## Seek

偏移游标到指定位置。

```csharp
combineStream.Seek(0, SeekOrigin.Begin);
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

## Read

读取组合流中的数据,返回值为实际读取的长度。

```csharp
var read = combineStream.Read(buffer, 0, buffer.Length);
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

组合流不支持写入，会引发一个`NotSupportedException`异常。

## SetLength

组合流不支持设定长度，会引发一个`NotSupportedException`异常。

## Flush

组合流不支持Flush，会引发一个`NotSupportedException`异常。