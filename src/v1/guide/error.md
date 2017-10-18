---
title: 常见错误
type: guide
order: 3
enable: true
---

## 常见错误

本文档描述了开发者经常会遇到的错误，以帮助开发者快速解决问题。

### 依赖注入

- 特性依赖注入总是在构造函数依赖注入之后执行的，请不要在构造函数中使用特性依赖注入的对象。
- 当为一个类注入时出现：`System.MissingMethodException: 没有为该对象定义无参数的构造函数` 请检查构造函数是否是`public`可访问的，只有当构造函数为`public`时才会进行注入。

### 路由

- 路由系统`Route()`中定义的uri的host匹配区块不允许使用变量匹配。即：`ui://{myname}/hello/world/1/`(在host中不能使用{myname}做变量匹配，这是一个错误的例子)