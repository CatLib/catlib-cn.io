---
title: 哈希
type: guide
order: 231
enable: true
---

## 哈希

哈希库为您提供了一些常用的哈希函数。您可以通过这些函数安全的加密用户的密码，验证文件的有效性等。

### 初始配置

在使用哈希组件之前您必须对组件进行配置（注意大小写敏感），您可以通过初始配置来配置：

| 配置名                            | 是否必须 | 配置描述(可以点击查看详细)                 |
| -------------------------------- |:------:|:--------------------------------------:|
| `HashingProvider.DefaultChecksum`     | 否      | 默认的校验算法(`Crc32`,`Adler32`) |
| `HashingProvider.DefaultHash`  | 否      | 默认的哈希算法（`MurmurHash`,`Djb`）  |

### 支持的哈希

- 非加密性哈希
-- `Adler32`
-- `Crc32`
-- `Murmur Hash 3`
-- `Djb Hash`

- 加密性哈希
-- `BCrypt`

### 非加密性哈希

**计算哈希**

您可以通过`Checksum`方法来进行一致性检查，方法接收2个参数，一个参数为输入值，第二个参数为使用的哈希算法。

``` csharp
var hashing = App.Make<IHashing>();
byte[] data; // 假定需要加密的数据
long hashCode = hashing.Checksum(data , Checksums.Crc32);
```

### 使用加密哈希

通过`HashPassword`方法您可以调用加密哈希，加密哈希接受2个参数，第一个参数为输入值，第二个参数为加密因子，加密因子默认为10，值越大则运行越慢，但越安全。加密Hash可以有效的保护您的密码。

> 使用的慢哈希算法为：`BCrypt`

``` csharp
var hashing = App.Make<IHashing>();
string data; // 假定需要加密的数据
long hashCode = hashing.HashPassword(data , 10);
```

**验证加密哈希和输入值是否匹配**

您可以通过`CheckPassword`来验证加密哈希和输入值是否匹配。

``` csharp
var hashing = App.Make<IHashing>();
bool isMatch = hashing.HashPassword("输入值" , "加密的哈希");
```
