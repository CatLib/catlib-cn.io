---
title: 字符串
---

# 字符串

字符串方法库允许您通过简单的方式访问和操作字符串。

## Method

提取字符串所表达的合法的函数名。

```csharp
Str.Method("Function");                       // Function
Str.Method("ClassName.Function");             // Function
Str.Method("ClassName.Function@NewFunction"); // NewFunction
Str.Method("ClassName.Function@#$%^^&*");     // Function
```

---
##### 函数原型

```csharp
string Method(string pattern);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `pattern`            | 需要提取函数名的字符串      |

## Is

判断规定字符串是否符合匹配表达式。匹配表达式只允许使用星号`*`通配(即删减正则表达式中除了星号外的所有功能)

```csharp
var result = Str.Is("he*d" , "helloworld"); // true
```

---
##### 函数原型

```csharp
string Is(string pattern, string value);
```

```csharp
bool Is<T>(string[] patterns, T source);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `pattern`            | 匹配表达式      |
| `value`              | 进行匹配的值      |

## AsteriskWildcard

将规定字符串翻译为星号匹配表达式。(即删减正则表达式中除了星号外的所有功能)

```csharp
var result = Str.AsteriskWildcard("[hello]w*d");
// result : "\[hello\]w*d"
```

---
##### 函数原型

```csharp
string AsteriskWildcard(string pattern);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `pattern`            | 需要处理的表达式      |

## Split

根据长度将字符串分割到数组中,最后一个元素可能小于需求长度。

```csharp
var result = Str.Split("helloworld1" , 2);
// result : ["he", "ll", "ow", "or", "ld", "1"]
```

---
##### 函数原型

```csharp
string[] Split(string str, int length = 1);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `str`            | 需要进行拆分的字符串      |
| `length`         | 拆分后每个元素的长度      |

## Repeat

将字符串重复指定的次数。

```csharp
var result = Str.Repeat("hello" , 3);
// result : hellohellohello
```

---
##### 函数原型

```csharp
string Repeat(string str, int num);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `str`            | 规定字符串      |
| `num`            | 规定字符串重复的次数      |

## Shuffle

随机打乱字符串中的所有字符

```csharp
var result = Str.Shuffle("helloworld");
// result 为被打乱的值，如：lwloeodlrh, 每次调用值都不一样
```

---
##### 函数原型

```csharp
string Shuffle(string str, int? seed = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `str`            | 规定字符串      |
| `seed`           | 随机种子，相同的种子，相同的规定字符串可以随机出相同的值        |

## SubstringCount

计算子串在字符串中出现的次数（该函数不计数重叠的子串）

```csharp
var result = Str.SubstringCount("abcabcab","abcab");
// result : 1
```

---
##### 函数原型

```csharp
int SubstringCount(string str, string subStr, int start = 0, int? length = null, StringComparison comparison = StringComparison.CurrentCultureIgnoreCase);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `str`            | 规定字符串      |
| `subStr`         | 查找的子字符串        |
| `start`          | 起始位置，如果为负数则从后向前计算起始位置        |
| `length`         | 从起始位置开始查询的长度，如果为负数则从后向前计算终止位置        |
| `comparison`     | 扫描规则        |

## Reverse

反转规定字符串

```csharp 
var result = Str.Reverse("helloworld");
// result : dlrowolleh
```

---
##### 函数原型

```csharp
string Reverse(string str);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `str`            | 规定字符串      |

## Reverse

反转规定字符串

```csharp 
var result = Str.Reverse("helloworld");
// result : dlrowolleh
```

---
##### 函数原型

```csharp
string Reverse(string str);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `str`            | 规定字符串      |

## Pad

把规定字符串填充为新的长度。

- 如果填充长度小于字符串的原始长度，则不进行任何操作。
- 填充字符串的两侧。如果不是偶数，则右侧获得额外的填充。

```csharp
var result = Str.Pad("hello", 10, "wor", PadTypes.Both);
// result : wohellowor
```

---
##### 函数原型

```csharp
string Pad(string str, int length, string padStr = null, PadTypes type = PadTypes.Right);
// 该函数原型即将在v2.0版本中弃用
```

```csharp
string Pad(int length, string str, string padStr = null, PadTypes type = PadTypes.Right);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `length`         | 规定字符串新的字符串长度，如果该值小于规定字符串的原始长度，则不进行任何操作。      |
| `str`            | 规定字符串      |
| `padStr`         | 规定供填充使用的字符串。默认是空白。注释：空白不是空字符串,而是` `(空白) |
| `type`           | 填充的方向`Both`,`Left`,`Right` |

## After

在规定字符串中查找在规定搜索值，并在规定搜索值之后返回规定字符串的剩余部分。(如果没有找到则返回规定字符串本身)

```csharp
var result = Str.After("helloworld","llo");
// result : world
```

---
##### 函数原型

```csharp
string After(string str, string search);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `str`            | 规定字符串      |
| `search`         | 搜索的值      |

## Contains

判断规定字符串是否包含规定子字符串。

```csharp
var result = Str.Contains("helloworld","llo");
// result : true
```

---
##### 函数原型

```csharp
bool Contains(string str, params string[] needles);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `str`            | 规定字符串      |
| `needles`        | 规定子字符串      |

## Replace

在规定字符串中替换匹配项。

```csharp
var result = Str.Replace(new []{"hel","wor"},"***","helloworld");
// result : ***lo***ld
```

---
##### 函数原型

```csharp
string Replace(string[] matches, string replace, string str);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `matches`            | 匹配的值列表      |
| `replace`            | 替换的值      |
| `str`                | 规定子字符串      |

## ReplaceFirst

替换规定字符串中第一次遇到的匹配项

```csharp
var result = Str.ReplaceFirst("hel","***","helloworldhelloworld");
// result : ***loworldhelloworld
```

---
##### 函数原型

```csharp
string ReplaceFirst(string match, string replace, string str);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `match`              | 匹配的值      |
| `replace`            | 替换的值      |
| `str`                | 规定子字符串      |

## ReplaceLast

替换规定字符串中从后往前第一次遇到的匹配项

```csharp
var result = Str.ReplaceLast("hel","***","helloworldhelloworld");
// result : helloworld***loworld
```

---
##### 函数原型

```csharp
string ReplaceLast(string match, string replace, string str);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `match`              | 匹配的值      |
| `replace`            | 替换的值      |
| `str`                | 规定子字符串      |

### Random

生成一个随机字母（含大小写），数字的字符串。

```csharp
var result = Str.Random(8);
// result : 一个随机值，如：u8Kn1YUz
```

---
##### 函数原型

```csharp
string Random(int length = 16, int? seed = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `length`              | 生成的随机字符串长度      |
| `seed`                | 随机种子，种子一致则随机顺序结果一致      |

## Trancate

如果长度超过给定的最大字符串长度，则截断字符串。 截断的字符串的最后一个字符将替换为默认为`...`的省略字符串。

```csharp
var result = Str.Trancate("helloworld",6);
// result: hel...
```

```csharp
var result = Str.Trancate("hello world",10 , " ");
// result: hello...
```

---
##### 函数原型

```csharp
string Truncate(string str, int length, object separator = null, string mission = null);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `str`                 | 规定字符串      |
| `length`              | 截断长度(含缺省字符长度)      |
| `separator`           | 临近的分隔符，如果设定则，截断长度为距离截断长度最近的分隔符位置,如果传入的是一个正则表达式那么则使用正则进行匹配。 |
| `mission`             | 缺省字符，默认是`...` |

## JoinList

返回给定数组的所有顺序组合。

```csharp
v[0] = "hello"
v[1] = "world"
var result = Str.JoinList(v, "/"); 
result[0] == "hello";
result[1] == "hello/world";
```

---
##### 函数原型

```csharp
string[] JoinList(string[] source, string separator = null);
```

```csharp
string[] JoinList(string[] source, char separator);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `source`              | 源数组      |
| `separator`                | 分割字符（串）      |

## Levenshtein

计算两个字符串之间的相似度。

> [https://en.wikipedia.org/wiki/Levenshtein_distance](https://en.wikipedia.org/wiki/Levenshtein_distance)

```csharp
Str.Levenshtein("hello", "world"); // 4
Str.Levenshtein("hello", "catlib"); // 5
Str.Levenshtein("hello", "catlib-world"); // 10
```

---
##### 函数原型

```csharp
int Levenshtein(string str1, string str2);
```

| 参数                            | 描述                 |
| -------------------------------- |:----------------------------:|
| `str1`              | 字符串1      |
| `str2`                | 字符串2      |

## Space

表示了一个带有空格的空字符串。

```csharp
var space = Str.Space; // " "
```

---
##### 常量原型

```csharp
const string Space = " ";;
```