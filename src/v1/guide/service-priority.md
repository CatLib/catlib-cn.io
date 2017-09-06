---
title: 初始化优先级
type: guide
order: 3001
---

## 初始化优先级

下表中列出了CatLib提供的核心服务初始化优先级。服务初始化将会严格按照优先级顺序执行。

> 优先级规则中0为最优先，值越大越落后，优先级相同则根据注册顺序触发。

| 服务名                     | 优先级      |
| ------------------------- |:------------:|
| `ConfigProvider`          | 1           |
| `FileSystemProvider`      | 10          |
| `UnityFileSystemProvider` | 10          |
| `MonoDriverProvider`      | 10          |
| `DebuggerProvider`        | 20          |
| `UnityDebuggerProvider`   | 20          |
| `ConvertersProvider`      | 2147483647  |
| `CompressProvider`        | 2147483647  |
| `EncryptionProvider`      | 2147483647  |
| `EventsProvider`          | 2147483647  |
| `HashingProvider`         | 2147483647  |
| `JsonProvider`            | 2147483647  |
| `RandomProvider`          | 2147483647  |
| `UnityRandomProvider`     | 2147483647  |
| `RoutingProvider`         | 2147483647  |
| `UnityRoutingProvider`    | 2147483647  |
| `TimeProvider`            | 2147483647  |
| `UnityTimeProvider`       | 2147483647  |
| `TimerProvider`           | 2147483647  |
| `TranslationProvider`     | 2147483647  |