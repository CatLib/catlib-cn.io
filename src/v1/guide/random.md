---
title: 随机数
type: guide
order: 204
enable: true
---

## 随机数

随机数组件为您提供了一些常用的随机算法。

### 初始配置

在使用随机数组件之前您必须对组件进行配置，您可以通过初始配置来配置：

| 配置名                              | 是否必须 | 配置描述(可以点击查看详细)                 |
| ---------------------------------- |:------:|:--------------------------------------:|
| `RandomProvider.DefaultRandomType` | 否      | 默认的随机数发生器，允许输入以下字符串（大小写敏感）：`MersenneTwister`,`WH2006`,`Xorshift`,`Mrg32k3a`  |

### 提供的随机算法

- `MersenneTwister` 马特赛特旋转演算法是一个伪随机数发生算法,可以快速产生高质量的伪随机数。该算法随机性好，占用内存较少。
- `WH2006` Wichman-Hill算法通过线性合并不同短周期随机数发生器的输出来产生长周期的随机数序列。Wichmann-Hill、Mersenne Twister算法性能差距不大。
- `Xorshift` 实现的是George Marsaglia，通过线性反馈移位寄存器生成。
- `Mrg32k3a` 使用的是素数模乘同余法，素数模乘同余发生器是提出的一个统计性质较好和周期较大的发生器,目前是使用最广的一种均匀随机数发生器，但产生速率较慢。

### 获取随机数发生器

**通过工厂生成**

您可以通过`IRandomFactory`接口来获取随机数工厂，该工厂通过参数可以生成不同算法的随机数发生器。

```csharp
var factory = App.Make<IRandomFactory>();
var random = factory.Make(RandomTypes.WH2006); // 参数允许传入一个RandomTypes或者字符串
//factory.Make("WH2006"); // 以上2句话是等价的
```

**获取默认定义的随机数发生器**

您可以通过`IRandom`接口来获取默认定义的随机数发生器。您可以通过配置来配置这个定义。

```csharp
var random = App.Make<IRandom>();
```

### 生成一个随机数

通过随机数发生器的`Next`方法可以进行随机数生成。

```csharp
var value = random.Next();
value = random.Next(100); //最大值为100（不包含）
value = random.Next(0,100);//最小值为0（包含），最大值为100（不包含）
```

### 生成浮点随机数

```csharp
var value = random.NextDouble(); // 0(包含)到1(不包含)之间的值
```
