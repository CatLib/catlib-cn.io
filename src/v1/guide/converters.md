---
title: 转换器
type: guide
order: 203
---

## 转换器

转换器提供了类型转换统一定义的空间，您可以通过转换器安全的转换类型。

### 默认支持的转换

CatLib默认的配置容器都默认支持如下转换类型:

| 支持类型              | 转换处理类                |
| -------------------- |:-----------------------:|
| `Bool`               | BoolStringConverter     |
| `Byte`               | ByteStringConverter     |
| `Char`               | CharStringConverter     |
| `DateTime`           | DateTimeStringConverter |
| `Decimal`            | DecimalStringConverter  |
| `Double`             | DoubleStringConverter   |
| `Enum`               | EnumStringConverter     |
| `Int16`              | Int16StringConverter    |
| `Int32`              | Int32StringConverter    |
| `Int64`              | Int64StringConverter    |
| `SByte`              | SByteStringConverter    |
| `Single`             | SingleStringConverter   |
| `String`             | StringStringConverter   |
| `UInt16`             | UInt16StringConverter   |
| `UInt32`             | UInt32StringConverter   |
| `UInt64`             | UInt64StringConverter   |

### 实现一个转换处理器

实现一个自定义的转换处理器需要实现`ITypeConverter`接口。

### 增加转换到转换器

通过 `AddConverter` 方法您可以增加一个转换处理器

### 转换目标类型

通过 `Convert` 和 `TryConvert` 方法您可以将一个类型通过转换器转化为另一个类型