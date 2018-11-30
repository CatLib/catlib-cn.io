---
title: 包装流
---

# 包装流

包装流是对`Stream`的包装，主要被用于当做其他基础流的基类。

## 构建包装流

```csharp
var wrapperStream = new WrapperStream(new MemoryStream());
```

---
##### 函数原型

```csharp
WrapperStream(Stream stream);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `stream`            | 基础流(原始)      |


## BaseStream

获取包装流的基础流

```csharp
var baseStream = wrapperStream.BaseStream;
```