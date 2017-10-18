---
title: 国际化(I18N)
type: guide
order: 250
enable: true
---

## 国际化(I18N)

CatLib国际化功能提供方便的方法来获取多语言的字符串，让你的游戏可以简单的支持多语言。

CatLib的语言代码遵循ISO 639, ISO 639-1, ISO 639-2, ISO 639-3

### 初始配置

初始配置必须在框架初始化前完成配置，下面是国际化组件需要的初始配置：

| 配置名                     | 是否必须 | 配置描述    |
| -------------------------- |:------:|:-----------:|
| `TranslationProvider.DefaultLanguage`      | 否      | 默认的语言  |
| `TranslationProvider.FallbackLanguage`     | 否      | 备选的语言  |

### 设定翻译资源

您可以通过 `SetResources` 接口来设定翻译资源，国际化工具本身不提供如何获取翻译字符串。您需要给定翻译资源国际化才能对其进行选择。

``` csharp
var translator = App.Make<ITranslator>();
translator.SetResources(/*you resources*/);
```

### 设定当前语言

您可以通过`SetLocale`来设定当前语言。设定的语言需要遵循`ISO639`语言规范。我们已经为您提供了`Languages`类来辅助您定义语言。

``` csharp
var translator = App.Make<ITranslator>();
translator.SetLocale(Languages.Chinese);
```

### 设定备选语言

备选语言则主语言无法获取时会使用备选语言（如果备选语言也不存在那么会将返回一个空字符串）

``` csharp
var translator = App.Make<ITranslator>();
translator.SetFallback(Languages.Chinese);
```

### 获取翻译语句

您可以通过`Get`方法来获取翻译语句。

``` csharp
var translator = App.Make<ITranslator>();
translator.Get("msg.title");
```

#### **翻译语句中的参数替换**

如果需要，你也可以在翻译语句中定义占位符。所有的占位符都使用的 `:` 开头。
 
``` json
{ "msg.title" : "你好:name" }
```

你可以在 `Get` 方法中传递一个数组作为第二个参数，它会将数组的值替换到语言内容的占位符中：

数组的第0个元素是键，第1个元素是对应的值，以此类推。

``` csharp
translator.Get("msg.title" ,"name" ,"catlib"); //output: 你好catlib
```

除此之外您还可以使用另外一种格式:

``` csharp
translator.Get("msg.title" ,"name:catlib"); //output: 你好catlib
```

如果你的占位符中包含了`首字母大写`或者`全体大写`，翻译过来的内容也会相应的做相应的处理：

``` json
{ "msg.title" : "你好:Name" } //output: 你好Catlib
{ "msg.title" : "你好:NAME" } //output: 你好CATLIB
```

### 复数形式

复数是个复杂的问题，不同语言对于复数有不同的规则，对此我们采用了成熟的复数形式方案，CatLib使用的复数形式方案来自于：[ZendFramework](https://framework.zend.com/)

#### **简单的区分**

使用管道符 `|` ，可以区分单复数字符串格式：

``` json
{ "msg.apple" : "这里只有一个苹果|这里有好多苹果" }
```

#### **复杂的区分**

你可以创建使用更复杂的复数规则，例如根据数量的范围不同来指定不同翻译语句：

``` json
{ "msg.apple" : "这里只有一个苹果|[1,19]这里有好多苹果|[20,*]天那成堆的苹果" }
```

当你定义完复数语句条件的时候，你可以使用 Get 方法来设置 `总数` 以获取符合对应条件的复数翻译语句。例如，在这个例子中，设置 `总数` 为 10 ，符合数量范围 1 至 19，所以会得到 `这里有好多苹果` 这条复数语句。