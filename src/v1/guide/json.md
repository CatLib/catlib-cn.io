---
title: Json
type: guide
order: 203
---

## Json

CatLib 提供了Json的解析组件。

> CatLib的Json解析器使用 [facebook-csharp-sdk/simple-json](https://github.com/facebook-csharp-sdk/simple-json) 提供的解析支持

### 序列化对象

通过`Encode`您可以序列化对象

``` csharp
var json = App.Make<IJson>();
var jsonStr = json.Encode(/*your object*/);
```

### 反序列化对象

``` csharp
var json = App.Make<IJson>();
var jsonObject = json.Decode</*your object*/>(jsonStr);
```

### 替换Json解析组件

您可以使用`IJsonAware`接口来替换内部Json解析器实现

``` csharp
var jsonAware = App.Make<IJsonAware>();
jsonAware.SetJson(/*new json impl*/);
```