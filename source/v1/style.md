---
title: 风格指南
---

# 风格指南

这是CatLib特有的代码风格指南，如果您在您的项目中使用CatLib，为了避免错误，降低沟通成本，小纠结和[反模式](anti-pattern.html)，阅读本指南是一份不错的选择。

我们不能保证风格指南中的所有内容，对于所有工程和团队都是理想的，所以根据项目环境，周围技术环境，风格出现偏差是可行的。

我们应该尽可能的遵守本风格指南提出的建议。

根据周围技术堆栈对于命名规范相关我们建议您阅读微软提供的：[框架设计指南](https://docs.microsoft.com/zh-cn/dotnet/standard/design-guidelines/index)


## 优先级定义

#### (A)必须

这些规范会帮你规避错误，您必须准守这些规范。这里可以存在例外，但是应该非常少见，只有您非常熟悉c#和周围技术栈且具备充足理由的情况下可以进行例外。

#### (B)强烈建议

这些规范能够在绝大多数项目中改善可读性和程序优雅度。即使你违反了，代码还是能照常运行，可以存在例外，但例外应该尽可能少且有合理的理由。

#### (C)建议

在这些规则里，我们提出一个默认的建议，如果理由充分，你可以随意在你的代码库中做出不同的选择。

#### (D)不建议

这些规则会导致代码变得难以维护甚至出现bug。这些规则列出了存在的潜在技术风险，并说明了它们什么时候不应该被使用。

## `(A)`项目名始终作为命名空间的开头

命名空间的第一个片段为项目的名字。这样做可以避免不同的第三方服务提供者提供的类发生冲突。

**错误的例子**

```csharp
namespace FileSystem
{
    public class FileSystem
    {
    }
}
```

**正确的例子**

```csharp
namespace CatLib.FileSystem
{
    public class FileSystem
    {
    }
}
```

## `(A)`服务的命名空间必须和服务的父级文件夹名一致

所有的服务(由多个类组成)命名空间必须和其父文件夹名字保持一致，以避免通过目录检索时无法明确服务位置，同时可以避免现在以及未来的类名相冲突。

**错误的例子**

- `Providers/CatLib.FileSystem/AdapterLocal.cs`

```csharp
namespace CatLib.IO
{
    public class AdapterLocal
    {
    }
}
```

- `Providers/CatLib.FileSystem/OSS/AdapterAliyunOSS.cs`

```csharp
namespace CatLib.IO.OSS
{
    public class AdapterAliyunOSS
    {
    }
}
```

**正确的例子**

- `Providers/CatLib.FileSystem/AdapterLocal.cs`

```csharp
namespace CatLib.FileSystem
{
    public class AdapterLocal
    {
    }
}
```

- `Providers/CatLib.FileSystem/OSS/AdapterAliyunOSS.cs`

```csharp
namespace CatLib.FileSystem.OSS
{
    public class AdapterAliyunOSS
    {
    }
}
```


## `(A)`文件名必须与类名一致

所有的类名必须和文件名一致，以避免通过目录来检索类时出现不一致的问题（这意味着我们不能将两个类写在同一个文件中，除非它是内部类）。

**错误的例子**

- `Providers/CatLib.FileSystem/AdapterLocal.cs`

```csharp
public class Local
{
}
```

**正确的例子**

- `Providers/CatLib.FileSystem/AdapterLocal.cs`

```csharp
public class AdapterLocal
{
}
```

## `(A)`内部类的访问级别不能高于protected

所有的内部类，不允许将访问级别设定为`public`或`internal`，因为如果内部类可以被外部访问，将会出现不可预测的问题。

**错误的例子**

```csharp
public class FileSystem
{
    public class Disk
    {
    }
}
```

**正确的例子**

```csharp
public class FileSystem
{
    private class Disk
    {
    }
}
```

## `(A)`对待编译器警告视同错误

所有的编译器警告都应该被处理，忽略编译器警告可能会导致一些隐藏的bug。

**错误的例子**

```csharp
int a = 0;
if(a == null) // 引发一个编译器警告
{
}
```

**正确的例子**

```csharp
int a = 0;
if(a == 0)
{
}
```

示例中会导致完全不同的两种结果。

## `(A)`对外服务的接口放在API命名空间下

对外服务的接口放在API命名空间下，这样在IED using的时候可以避免错误的引用实现代码而发生耦合。

一般来说我们放置在:`项目名`.`API`.`组件名`的命名空间下

**错误的例子**

- `Providers/CatLib.FileSystem/API/IFileSystem.cs`

```csharp
namespace CatLib.FileSystem
{
    public interface IFileSystem
    {
    }
}
```

**正确的例子**

- `Providers/CatLib.FileSystem/API/IFileSystem.cs`

```csharp
namespace CatLib.API.FileSystem
{
    public interface IFileSystem
    {
    }
}
```

## `(B)`完整单词的类名

类名应该倾向于完整单词，而不是单词的缩写。

代码编辑器的自动补全功能已经让书写长类名的成本变得非常的低，而其带来的的明确性是非常宝贵的，我们不应该使用缩写来代替长类名。

**错误的例子**

```tree
Providers/
  CatLib.FileSystem/
    ManagerFS.cs
    OptsFS.cs
```

**正确的例子**

```tree
Providers/
  CatLib.FileSystem/
    ManagerFileSystem.cs
    OptionsFileSystem.cs
```

## `(B)`紧密耦合的类名

如果一个类只在另外一个类的场景下有意义，这层关系应该体现在类名(包括文件名)上。因为编辑器通常会按字母顺序组织文件，所以这样做可以把相关联的文件排在一起。

**错误的例子**

```tree
Providers/
  CatLib.FileSystem/
    Handler.cs
    DirectoryHandler.cs
    FileForHandler.cs
```

**正确的例子**

```tree
Providers/
  CatLib.FileSystem/
    Handler.cs
    HandlerDirectory.cs
    HandlerFile.cs
```

## `(B)`类名的单词顺序

类名应该以`高级别`的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。

#### 为什么不遵循自然语意

在自然的英文里，形容词和其它描述语通常都出现在名词之前，否则需要使用连接词。

- sth with sb

如果你愿意，你完全可以在类名里包含这些连接词，但是单词的顺序很重要。

在你的类名中，所谓的`高级别`一般和语境有关，比如对于一个登录界面来说它可能包含下面这些类：

**错误的例子**

```tree
Providers/
  CatLib.LoginUI/
    AgreementCheckbox.cs
    LoginButton.cs
    PasswordInput.cs
    RegisterButton.cs
    TextInput.cs
```

我们可以发现我们很难发现那些类是针对`输入`这个功能的。现在我们根据单词顺序进行重命名：

**正确的例子**

```tree
Providers/
  CatLib.LoginUI/
    ButtonLogin.cs
    ButtonRegister.cs
    CheckboxAgreement.cs
    InputText.cs
    InputPassword.cs
```

因为编辑器通常会按字母顺序组织文件，所以将高级别的单词排在前类之间的重要关系一目了然。

## `(B)`多级目录与类命名

[类名的单词顺序](#B-类名的单词顺序)指出的问题可以通过多级目录进行解决，但是我们只推荐服务只有在非常大型的情况下(100+的类)才这么做。因为在多级目录中查找要比在单目录中查找更加花费精力，并且对于命名空间的引用将会变得复杂。

如果您使用了多级目录来区分那么可以忽略重名部分。

**错误的例子**

```tree
Providers/
  CatLib.LoginUI/
    Checkbox/
        CheckboxAgreement.cs
    Button/
        ButtonLogin.cs
        ButtonRegister.cs
    Input/
        InputPassword.cs
        InputText.cs
```

**正确的例子**

```tree
Providers/
  CatLib.LoginUI/
    Checkbox/
        Agreement.cs
    Button/
        Login.cs
        Register.cs
    Input/
        Password.cs
        Text.cs
```

## `(B)`函数名和类名大小写

在声明函数和类的时候，其命名始终使用CamelCase。

我们遵循了微软提供的[框架设计指南](https://docs.microsoft.com/zh-cn/dotnet/standard/design-guidelines/index)来确保 API 的一致性和易用性。

**错误的例子**

```csharp
public class fileSystem
{
}
```

**正确的例子**

```csharp
public class FileSystem
{
}
```

## `(B)`以类名作为服务名，而不是以接口名

我们强烈建议以类名作为服务名，而不是以接口名作为服务名。接口名应该以别名的形式附加到服务名上。这样做我们能够让绑定关系更加明确。

**错误的例子**

```csharp
App.Bind<IFileSystem>(()=> new FileSystem());
```

**正确的例子**

```csharp
App.Bind<FileSystem>(()=> new FileSystem())
    .Alias<IFileSystem>();
```

## `(B)`服务内的命名规范一致

对于一个服务中的命名规范强烈建议一致，例如：要么变量是`_`开头要么始终是`m_`开头（或者其他统一的规范，如：无标示符开头）。而不能交叉混用。

交叉混用会导致团队一致性下降，从而提高理解成本。单个组件一般由2-3人协同开发。我们强烈建议以服务为最小单元统一代码命名规范。

**错误的例子**

- `Providers/CatLib.FileSystem/FileSystem.cs`

```csharp
public class FileSystem
{
    private string defaultDiskName;
    private IDictionary<string, IDisk> m_disks;
}
```

- `Providers/CatLib.FileSystem/Disk.cs`

```csharp
public class Disk
{
    private string _diskName;
}
```

**正确的例子**

- `Providers/CatLib.FileSystem/FileSystem.cs`

```csharp
public class FileSystem
{
    private string defaultDiskName;
    private IDictionary<string, IDisk> disks;
}
```

- `Providers/CatLib.FileSystem/Disk.cs`

```csharp
public class Disk
{
    private string diskName;
}
```

## `(C)`使用上下文关系来处理不同实例相同接口的服务

我们建议使用上下文关系来处理不同实例相同接口的服务，而不是使用在具体实现构造函数中通过获取指定服务。如果在实际的实现中获取服务实例，将会和框架耦合，并产生公共耦合。

而将这些关系定义在服务提供者中将会避免这些问题。

**错误的例子**

```csharp
public class GameVideo : IGameVideo
{
    public GameVideo(IFileSystem fileSystem)
    {
        var disk = fileSystem.Get("oss");
    }
}
```

**正确的例子**

```csharp
public class GameVideo : IGameVideo
{
    public GameVideo(IDisk disk)
    {
    }
}
```

```csharp
App.Singleton<GameVideo>().Alias<IGameVideo>()
    .Needs<IDisk>()
    .Given(()=> App.Make<IFileSystem>().Get("oss"));
```


## `(C)`总是在服务提供者中来注册服务

我们建议服务在服务提供者中进行绑定，而不是在`Register`以外的其他地方。

**错误的例子**

- 在`Register`之外的其他地方

```csharp
protected override void OnStartCompleted()
{
    App.Singleton<FileSystem>();
}
```

**正确的例子**

```csharp
public class ProviderFileSystem : ServiceProvider
{
    public override void Register()
    {
        App.Singleton<FileSystem>();
    }
}
```

## `(C)`字符串常量的值，可以映射到实际类型

我们建议字符串常量，可以映射到实际有效的类型或者该常量本身。这样在未来需要通过常量值来分析时可以快速定位具体的类。

**错误的例子**

```csharp
public class ApplicationEvents 
{
    public const string Bootstrapping = "bootstrapping";
}
```

**正确的例子**

```csharp
public class ApplicationEvents 
{
    public const string Bootstrapping = "ApplicationEvents.Bootstrapping";
}
```

## `(C)`一个服务只提供一个服务提供者

一个服务只提供一个服务提供者。如果一个服务提供了多个服务提供者将会导致沟通成本的上升，使用者无法了解不同服务提供者之间的区别或不知道该如何使用。

我们建议使用变量控制的方式来处理这个问题。

**错误的例子**

- `Providers/CatLib.FileSystem/ProviderFileSystemClean.cs`

```csharp
public class ProviderFileSystemClean : ServiceProvider
{
    public override void Register()
    {
        App.Singleton<FileSystem>();
    }
}
```

- `Providers/CatLib.FileSystem/ProviderFileSystem.cs`

```csharp
public class ProviderFileSystem : ServiceProvider
{
    public override void Register()
    {
        App.Singleton<FileSystem>()
            .OnResolving((instance) =>
            {
                var fileSystem = (FileSystem)instance;
                fileSystem.Extend(()=> new Disk(new AdapterLocal()), "local");
                fileSystem.Extend(()=> new Disk(new AdapterHttp()), "http");
            });
    }
}
```

**正确的例子**

- `Providers/CatLib.FileSystem/ProviderFileSystem.cs`

```csharp
public class ProviderFileSystem : ServiceProvider
{
    public bool ExtendDefaultAdapter { get; set; } = false;
    public override void Register()
    {
        var binder = App.Singleton<FileSystem>();
        if(!ExtendDefaultAdapter)
        {
            return;
        }
        binder.OnResolving((instance) =>
        {
            var fileSystem = (FileSystem)instance;
            fileSystem.Extend(()=> new Disk(new AdapterLocal()), "local");
            fileSystem.Extend(()=> new Disk(new AdapterHttp()), "http");
        });
    }
}
```

## `(C)`可拆分单词的命名

有些时候我们可能纠结于可拆分单词如何进行命名，如：`username`可以被拆分为`user`和`name`。虽然这些单词可以被拆分但是我们需要注意`username`为英文中的一个整体单词。所以我们应该视作为一个单词。

**错误的例子**

```csharp
public class LoginUI
{
    private string userName; // UserName, _userName 都是错误的例子
}
```

**正确的例子**

```csharp
public class LoginUI
{
    private string username; // Username, _username 都是正确的例子
}
```

## `(C)`门面应该放置在Facades命名空间下

属于[门面](architecture/facade.html)的代码应该被放置在`项目名.Facades`的命名空间下。

**错误的例子**

- `Providers/CatLib.FileSystem/Facades/FileSystem.cs`

```csharp
namespace CatLib.FileSystem
{
    public class FileSystem : Facade<IFileSystem>
    {
    }
}
```

**正确的例子**

- `Providers/CatLib.FileSystem/Facades/FileSystem.cs`

```csharp
namespace CatLib.Facades
{
    public class FileSystem : Facade<IFileSystem>
    {
    }
}
```

> 门面作为一个特殊存在，所以我们允许其命名空间例外于其他规范。

## `(D)`在循环中生成Lambda表达式，并尝试访问迭代器变量。

在循环中生成Lambda表达式，并尝试访问迭代器变量时，会导致迭代器变量不是预期值的问题。

```csharp
foreach (var index in new int[1, 2, 3, 4, 5])
{
    closure(()=> index); // index 为预期外的值
}
```

迭代器会导致index发生变化，从而使lambda表达式不能返回正确的index值。