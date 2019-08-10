---
title: 分片流
---

# 分片流

分片流可以用于包装指定分片的流。

使指定分片的流访问起来就像传统流那样从开头到结尾。

## 构建分片流

```csharp
var segmentStream = new SegmentStream(baseStream, 1024);
```

> 分片流的起始位置为基础流的position，在构建分片流时会被记录。

---
##### 函数原型

```csharp
SegmentStream(Stream stream, long partSize = 0);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `stream`              | 基础流        |
| `partSize`            | 分片大小      |

## Length

获取分片流的长度。

```csharp
var length = segmentStream.Length;
```

---
##### 函数原型

```csharp
long Length { get; }
```

## Position

获取分片流的偏移量

```csharp
var position = segmentStream.Position;
```

---
##### 函数原型

```csharp
long Position { get; }
```

## Seek

偏移游标到指定位置。

```csharp
segmentStream.Seek(0, SeekOrigin.Begin);
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

读取分片流中的数据,返回值为实际读取的长度。

```csharp
var read = segmentStream.Read(buffer, 0, buffer.Length);
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

分片流不支持写入，会引发一个`NotSupportedException`异常。

## SetLength

分片流不支持设定长度，会引发一个`NotSupportedException`异常。