---
title: 配置
type: guide
order: 200
---

## 配置

CatLib允许通过配置系统来配置服务提供者所需要的功能参数。每一个服务提供者都会提供他们自己特有的配置信息。您可以从这些提供商的文档中了解相关配置信息。

CatLib的配置系统可帮助您查找，加载，自动填充，任意类型您所需要的配置。无论其来源可能是什么(代码内嵌，CSV，YAML，XML，INI文件或数据库)。

### 基本概念

CatLib的配置系统，允许您拓展多个互相独立的配置容器，每个容器都可以配置不同的配置定位器来获取配置。

什么是配置定位器？配置定位器是被用于从代码，CSV，YAML，XML，INI文件或数据库中来获取配置。

> 请注意，所有的CatLib组件都会从`默认`的配置定位器中获取数据

### 文档上下文说明

文档中演示的片段代码如果没有特殊说明均是使用下面所示的上下文关系和默认的配置容器:

``` csharp
var manager = App.Make<IConfigManager>();
IConfig config = manager.Default;
```

### 默认配置容器

CatLib的默认配置容器，自动挂载了：代码配置定位器(用于获取从代码中设定的配置)。

您新拓展的配置容器，不会挂载任何的配置定位器，需要您手动挂载。

### 获取配置

如果您需要从配置组件中获取配置，您可以通过`Get`方法来获取。第一个参数为`配置名`，第二个参数为`默认值`。当配置不存在的时候会使用默认值来填充缺失的配置。

``` csharp
string val = config.Get("config.name", string.Empty);
```

### 设定与保存配置

您可以通过`Set`方法来设定配置数据，第一个参数为`配置名`，第二个参数为`配置值`。

``` csharp
config.Set("config.name", "hello world");
```

单纯的`Set`并不会保存数据，您还需要调用`Save`来储存数据。

注意，并不是所有的配置定位器都支持保存数据的，例如：代码配置定位器就不会保存数据，但也不会引发异常。

``` csharp
config.Save();
```

### 设定定位器

设定配置定位器能够为配置系统提供访问不同区域数据的能力。

``` csharp
config.SetLocator(new UnitySettingLocator());
```

### 提供的定位器

CatLib为您提供了以下2种配置定位器，您可以根据场景来选择。

| 定位器名                | 描述                                                                     |
| ---------------------- |:-----------------------------------------------------------------------:|
| `CodeConfigLocator`    | 代码配置定位器，能够设定和读取通过代码设定的配置。代码配置定位器的配置不会被保存。    |
| `UnitySettingLocator`  | Unity 提供的 PlayerPrefs 所实现的配置定位器适配类。配置可以被保存               |

### 转换机制

CatLib的配置系统使用CatLib转换组件来对数据进行转换，转换相关内容请参考[转换器](converters.md)

### 拓展配置容器

CatLib允许您扩展配置容器，您可以通过扩展方法来定义额外的配置容器。

> 如果您要使用自定义的配置容器请实现`IConfig`接口。

``` csharp
manager.Extend(()=>
{
    return new Config();
}, "new")
```

如果需要获得刚刚扩展的配置容器，只需要使用`Get()`进行访问。

``` csharp
IConfig extendConfig1 = manager.Get("new");
IConfig extendConfig2 = manager["new"];
// extendConfig1 == extendConfig2
```

### 修改默认配置容器

您可以使用自定义的配置容器来代替默认的配置容器。修改后`Default`使用的配置容器将是您替换的配置容器。

> 注意! 由于CatLib提供的组件配置使用默认的配置容器，所以在没有了解组件的配置使用情况前，谨慎修改。

``` csharp
manager.SetDefault("new.default");
IConfig config = manager.Default;
```