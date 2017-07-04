---
title: 版本说明
type: guide
order: 2
---

## 版本说明

> CatLib的版本标准是采用：[Semver语义化版本标准](http://semver.org/lang/zh-CN/)

### V0.8.3 Beta

- 修复了 `UnitySettingLocator` 无法正确存储及访问的问题
- 修复了 `Application.Version` 不一致的问题

### V0.8.2 Beta

- 修复了由于 `FileSystemManager` 门面模型的配置错误引发的bug

### V0.8.1 Beta

- 修复了版本名不一致的问题
- 修复了遗漏注册的文件服务提供者
- 修复了一个bug，这个bug会导致在路径`/`和`\`混用时无法正确定位
- 修复了一个bug，这个bug导致在没有给定`AssetPath`时不能返回默认的路径

### V0.8.0 Beta

> 本版本为 CatLib 预发行Beta版

- 新增CatLib核心
- 新增标准库 - 容器
- 新增标准库 - 有序集
- 新增标准库 - 快速列表
- 新增标准库 - 最少使用缓存
- 新增标准库 - 过滤器链
- 新增路由组件
- 新增文件系统
- 新增配置组件
- 新增时间组件
- 新增计时器组件
