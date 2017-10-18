---
title: 命名规范
type: guide
order: 4
enable: true
---

## 命名规范

本文档描述了CatLib框架中的命名规范。

> 如果您要为为 CatLib 发起贡献，您必须遵循这份命名规范文档。

　
> 如果没有明确指定`访问级别(public|private等)`则表示对所有访问级别生效。

### 概述

- `代码`必须使用`4个空格`缩进，而不要使用制表符(tab)
- 使用`using XXX`，而不是`new XXX.xxx()`
- `{}`必须换行,且内部代码`顶格书写`
``` csharp
if(true)
{
    var tf = true;
}
```

- 使用`///`对`代码`进行注释
``` csharp
/// <summary>
/// 这里对代码进行了注释
/// </summary>
public string VariableName = "VariableName";
```

- 对于容易`产生歧义的表达式`您应该使用`括号`包裹
``` csharp
if(variable1 + (++variable2) > 0)
{
}
```

- `模板`必须为`TStudlyCaps`（驼峰式大写）以`T开头`
``` csharp
public class Bootstrap<TType>
{
}
```

- `接口`必须为`IStudlyCaps`（驼峰式大写）以`I开头`
``` csharp
public interface IBootstrap 
{ 
}
```

### 类名及函数

- `类名`必须为`StudlyCaps`（驼峰式大写）
``` csharp
public class Bootstrap
{ 
}
```

- `函数名`必须为`StudlyCaps`（驼峰式大写）
``` csharp
public void MyFunc()
{
}
```

### 变量,常量及属性

- 类`公共变量名`必须为`StudlyCaps`（驼峰式大写）
``` csharp
public string VariableName = "hello";
```

- 类`受保护变量名`必须为`StudlyCaps`（驼峰式大写）
``` csharp
internal string VariableName = "hello";
protected string VariableName = "hello";
protected internal string VariableName = "hello";
```

- 类`私有变量名`必须为`camelCase`（驼峰式小写）
``` csharp
private string variableName = "hello";
```

- 类`属性名`必须为`StudlyCaps`（驼峰式大写）
``` csharp
public string VariableName{ get; set; }
```

- `静态变量`和`常量`必须为`StudlyCaps`（驼峰式大写）
``` csharp
public const string VariableName = "hello";
public static readonly string VariableName = "hello";
public static string VariableName = "hello";
internal const string VariableName = "hello";
protected const string VariableName = "hello";
private const string VariableName = "hello";
```

- `参数`必须为`camelCase`（驼峰式小写）
``` csharp
public void FunctionName(Action callFunction)
{
    var localVariable = callFunction;
}
```

### 枚举

- `枚举名`必须为`StudlyCaps`（驼峰式大写）
``` csharp
public enum ApplicationEvents
{ 
}
```

- `枚举元素名`必须为`StudlyCaps`（驼峰式大写）
``` csharp
public enum ApplicationEvents
{
    OnStart  = 1,
    OnInited = 2,
}
```

### 命名空间

- `代码`必须在`项目名`的根命名空间中
``` csharp
namespace CatLib
{
}
```

- 组件`代码`必须在`项目名.组件名`的命名空间中
``` csharp
namespace CatLib.Routing
{
}
```

### 文件
- `文件名`必须和`类名`一致
- `文件`必须只使用`UTF-8`而不使用BOM代码
- 一个文件中不能出现`2个`及以上的类，除非它是内部类或者类的重载