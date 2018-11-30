---
title: 管理器模版
---

# 管理器模版

管理器模版是常见的管理器类的模板类，可以快速的帮助开发者完成一个管理器功能的实现。

## 管理器类型

管理器模版分为两类：`单例管理器模版`和`实例管理器模版`。

- `单例管理器模版`：每次构建的指定名字的实现将会是一个单例。
- `实例管理器模版`：每次构建的指定名字的实现将会是一个实例。

## 结构关系

所有的管理器类都是基于`Managed`类的。其中又被派生出`Manager`和`SingleManaged`

```markdown
- Managed
- - Manager
- - SingleManaged
- - - SingleManager
```

## Managed和Manager的区别

所有以`ed`结尾的管理器，不能够被外部获得管理器扩展的实(单)例。

而`er`结尾的管理器可以通过`Get`来获取管理器的扩展实(单)例

## 门面支持

管理器的同名门面可以在`CatLib.Facades.Template`命名空间中访问。

## 如何声明一个管理器

您只需要继承`Managed`,`Manager`,`SingleManaged`,`SingleManager`任何一个为基类就可以声明管理器。

管理器类需求一个模版类型，模版类型为扩展的类型。

```csharp
public class FileSystem : SingleManager<IDisk>
{ 
}
```

```csharp
var fileSystem = new FileSystem();
```

## 当扩展被解决时

可以通过`OnResolving`事件，获取管理器构建扩展的操作。

```csharp
fileSystem.OnResolving += (IDisk disk)=>
{
    //todo: 
};
```

## 当扩展被解决之后

可以通过`OnAfterResolving`事件，获取管理器构建扩展之后的操作。

```csharp
fileSystem.OnAfterResolving += (IDisk disk)=>
{
    //todo: 
};
```

## 自定义一个扩展构建器

您可以通过`Extend`来定义一个扩展的构建器。

```csharp
fileSystem.Extend(()=>
{
    return new Disk(new Local());
}, "local");
```

## 移除自定义的扩展构建器

您可以通过`RemoveExtend`来移除一个扩展构建器。

```csharp
fileSystem.RemoveExtend("local");
```

## 判断扩展构建器是否存在

通过`ContainsExtend`可以判断扩展构建器是否存在。

```csharp
fileSystem.ContainsExtend("local"); // return false
```

## 获取扩展构建器生成的扩展

本函数只对`er`结尾的管理器模版有效。

```csharp
fileSystem.Get("local");
fileSystem["local"];
```

## 当扩展被释放时

本函数只对`Single`单例管理器模版有效。

通过`OnRelease`可以获得扩展被释放时的事件。

```csharp
fileSystem.OnRelease += (IDisk disk)=>
{
  // todo:
};
```

## 释放扩展生成的单例

本函数只对`Single`单例管理器模版有效。

通过`Release`可以释放通过扩展构建器生成的单例。

```csharp
fileSystem.Release("local");
```

> 本操作会引发[当扩展被释放时](#当扩展被释放时)事件

## 判断单例扩展是否已经被生成

本函数只对`Single`单例管理器模版有效。

通过`Contains`允许开发者判断单例扩展是否已经被生成。

```csharp
fileSystem.Contains("local"); // return false;
```

## Default

本函数只对`SingleManager`单例管理器模版有效。

通过Default属性可以获得默认的扩展实现。

```csharp
var disk = fileSystem.Default;
```
