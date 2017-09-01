---
title: 服务表
type: guide
order: 3000
---

## 服务表

本表中列出了CatLib框架所有内置组件可以通过`Make`方法构建的服务，`接口名`和`别名`均可用于构建服务。

- 所有的`(默认)`等价与使用`管理器`调用`Default`方法得到的内容。

| 描述                | 接口名               | 别名        | 所属组件                              |
| -----------------  |:--------------------:|:-----------:|:------------------------------------:|
| 压缩解压缩管理器     | `ICompressManager`   | `无`        | [压缩解压缩](compress.html)            |
| 压缩解压缩(默认)     | `ICompress`          | `无`        | [压缩解压缩](compress.html)            |
| 配置管理器          | `IConfigManager`      | `无`       | [配置](config.html)                   |
| 转换器管理器        | `IConvertersManager`  | `无`       | [转换器管理器](converters.html)        |
| 转换器(默认)        | `IConverters`         | `无`       | [转换器管理器](converters.html)        |
| 日志工具            | `ILogger`             | `无`       | [控制台](console.html)                |
| 数据监控            | `IMonitor`            | `无`       | [控制台](console.html)                |
| 加解密工具          | `IEncrypter`          | `无`       | [加解密](encryption.html)            |
| 全局事件            | `IDispatcher`         | `无`       | [事件](events.html)                  |
| 文件系统管理器       | `IFileSystemManager` | `无`        | [文件系统](file-system.html)          |
| 文件系统(默认)       | `IFileSystem`        | `无`        | [文件系统](file-system.html)          |
| 哈希库              | `IHashing`            | `无`       | [哈希](hashing.html)                 |
| Json库              | `IJson`, `IJsonAware` | `无`       | [Json](json.html)                   |
| 路由系统            | `IRouter`             | `无`       | [路由系统](routing.html)             |
| 国际化(I18N)        | `ITranslator`         | `无`       | [国际化(I18N)](translation.html)     |