---
title: 守卫
---

# 守卫

Guard允许您使用简单优雅的代码写出断言代码。守卫是可拓展的。

## 实用函数

#### That

通过**That**可以获取守卫实例，这样您可以使用扩展函数为守卫进行扩展。

```csharp
var guard = Guard.That;
```

#### Requires

验证条件并在条件失败时抛出异常。

```csharp
Guard.Requres<ArgumentNullException>(arg != null, $"Argument {nameof(arg)} cannot be null.");
```

#### ParameterNotNull

验证指定的参数不为空

```csharp
Guard.ParameterNotNull(arg, nameof(arg));
```

## 扩展异常

在进行守卫时，您可以扩展异常的构建方法，这样可以生成一些复杂的异常关系。

```csharp
Guard.Extend<ArgumentNullException>((message, innerException, state) =>
{
    return new ArgumentNullException(message, innerException);
})
```

> 如果扩展返回`null`则会通过反射构建。