---
title: 加解密
type: guide
order: 232
enable: true
---

## 加解密

CatLib加解密组件使用`AES-256`和`AES-128`加密，强烈建议您使用由CatLib加解密组件所提供的加密服务，而不要使用自己的加解密算法。

所有加密信息在底层都会进行`HMac`签名，以保证加密值不能被更改。

### 初始配置

在使用CatLib加解密组件之前您必须对组件进行配置，您可以通过初始配置来配置：

| 配置名                            | 是否必须 | 配置描述(可以点击查看详细)                 |
| -------------------------------- |:------:|:--------------------------------------:|
| `EncryptionProvider.Key`     | 是      | 密钥（AES-128需要16位长度, AES-256需要32位长度）  |
| `EncryptionProvider.Cipher`  | 否      | 加密类型（`AES-128-CBC` 和 `AES-256-CBC`）  |

### 使用加解密组件

**加密一个值**

您可以通过`Encrypt`方法来对值进行加密，所有的值一经加密则不能修改。

```csharp
var encrypter = App.Make<IEncrypter>();
byte[] data;
string encryptStr = encrypter.Encrypt(data);
```

**解密被加密的内容**

```csharp
var encrypter = App.Make<IEncrypter>();
byte[] str = encrypter.Decrypt(encryptStr);
```

如果解密失败，例如：HMac验证失败，那么将会抛出一个`EncryptionException`异常。

## 交换密钥算法

```csharp
var manager = App.Make<IEncryptionManager>();
var key = manager.ExchangeSecret((publicKey)=>{
    //todo: 将客户端的publicKey提交到服务器并换回服务器的serverPublicKey
    return serverPublicKey;
});
```

## 扩展加密组件

您可以通过`Extend`方法对加密函数进行拓展。

```csharp
var manager = App.Make<IEncryptionManager>();
manager.Extend(()=>{
    return new AesEncrypter();
},"extend-encrypter");

var encrypter = manager["extend-encrypter"];
```