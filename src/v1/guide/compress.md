---
title: 压缩解压缩
type: guide
order: 233
---

## 压缩解压缩

CatLib 为开发者内置了`GZip`,`LZMA`的压缩和解压缩支持。您可以通过简单的接口调用即可完成对数据的压缩解压缩。

### 初始配置

您可以通过初始配置来配置初始参数，您可以通过初始配置来配置：

| 配置名                            | 是否必须 | 配置描述(可以点击查看详细)                 |
| -------------------------------- |:------:|:--------------------------------------:|
| `CompressProvider.DefaultLevel`  | 否      | 压缩等级（如果适用的话）（默认值6）  |

### 预制的解压缩器

| 解压缩格式    | 名字                     |
| -----------  |:------------------------:|
| `GZip`       | `默认名字(null)`,`gzip`   |
| `LZMA`       | `lzma`                   |
 
### 基本使用

**对数据进行压缩**

您可以通过`Compress`方法来对数据进行压缩。

```csharp
var manager = App.Make<ICompressManager>();
byte[] compressData = manager.Compress(data);
```

**对数据进行解压缩**

```csharp
var manager = App.Make<ICompressManager>();
byte[] data = manager.Decompress(compressData);
```

### 扩展一个解压缩器

您可以通过`Extend`方法来对解压缩器进行扩展

```csharp
var manager = App.Make<ICompressManager>();
manager.Extend(() => new LzmaAdapter() , "lzma");
```