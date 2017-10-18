---
title: 字符串(Str)
type: detail
order: 102
enable: true
---

## 字符串(Str)

字符串方法库允许您通过简单的方式访问和操作字符串。

### 可用方法

- [Is](#Is)
- [AsteriskWildcard](#AsteriskWildcard)
- [Split](#Split)
- [Repeat](#Repeat)
- [Shuffle](#Shuffle)
- [SubstringCount](#SubstringCount)
- [Reverse](#Reverse)
- [Pad](#Pad)
- [After](#After)
- [Contains](#Contains)
- [Replace](#Replace)
- [ReplaceFirst](#ReplaceFirst)
- [ReplaceLast](#ReplaceLast)
- [Random](#Random)

### Is

判断规定字符串是否符合匹配表达式。（匹配表达式允许使用星号(*)通配）

```csharp
var result = Str.Is("/hello/w*d" , "helloworld");
// result : true
```

### AsteriskWildcard

将规定字符串翻译为星号匹配表达式。（即删减正则表达式中除了星号外的所有功能）

```csharp
var result = Str.AsteriskWildcard("[hello]w*d");
// result : "\[hello\]w*d"
```

### Split

根据长度将字符串分割到数组中。

```csharp
var result = Str.Split("helloworld" , 2);
// result = ["he" , "ll" , "ow" , "or" , "ld"]
```

### Repeat

将字符串重复指定的次数。

```csharp
var result = Str.Repeat("hello" , 3);
// result : hellohellohello
```

### Shuffle

随机打乱字符串中的所有字符

```csharp
var result = Str.Shuffle("helloworld");
// result 为被打乱的值，如：lwloeodlrh, 每次调用值都不一样
```

### SubstringCount

计算子串在字符串中出现的次数（该函数不计数重叠的子串）

```csharp
var result = Str.SubstringCount("abcabcab","abcab");
// result : 1
```

### Reverse

反转规定字符串

```csharp 
var result = Str.Reverse("helloworld");
// result : dlrowolleh
```

### Pad

把规定字符串填充为新的长度。

- 如果填充长度小于字符串的原始长度，则不进行任何操作。
- 填充字符串的两侧。如果不是偶数，则右侧获得额外的填充。

```csharp
var result = Str.Pad("hello", 10, "wor", PadTypes.Both);
// result : wohellowor
```

### After

在规定字符串中查找在规定搜索值，并在规定搜索值之后返回规定字符串的剩余部分。(如果没有找到则返回规定字符串本身)

```csharp
var result = Str.After("helloworld","llo");
// result : world
```

### Contains

判断规定字符串是否包含规定子字符串。

```csharp
var result = Str.Contains("helloworld","llo");
// result : true
```

### Replace

在规定字符串中替换匹配项。

```csharp
var result = Str.Replace(new []{"hel","wor"},"***","helloworld");
// result : ***lo***ld
```

### ReplaceFirst

替换规定字符串中第一次遇到的匹配项

```csharp
var result = Str.ReplaceFirst("hel","***","helloworldhelloworld");
// result : ***loworldhelloworld
```

### ReplaceLast

替换规定字符串中从后往前第一次遇到的匹配项

```csharp
var result = Str.ReplaceLast("hel","***","helloworldhelloworld");
// result : helloworld***loworld
```

### Random

生成一个随机字母（含大小写），数字的字符串。

```csharp
var result = Str.Random(8);
// result : 一个随机值，如：u8Kn1YUz
```