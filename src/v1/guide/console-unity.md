---
title: 控制台扩展
type: guide
order: 520
enable: true
---

## 调试控制台扩展(Unity增强)

Unity项目下您可以通过`Component` => `CatLib` => `Debugger` 来进行可视化的配置。

### 日志记录器

在Unity项目下允许关联Unity日志，您可以将可视化配置的`AssociationUnityLog`参数配置为`True`启动。

### 监控器

在Unity项目下您可以对下面数据进行监控：

| 支持监控                 | 描述                |
| ----------------------- |:-------------------:|
| `MonitorPerformance`    | Unity性能监控        |
| `MonitorScreen`         | 屏幕数据监控         |
| `MonitorScene`          | 场景监控             |
| `MonitorSystemInfo`     | 系统信息             |
| `MonitorPath`           | 路径信息             |
| `MonitorInput`          | 输入信息             |
| `MonitorInputLocation`  | 定位信息             |
| `MonitorInputGyroscope` | 陀螺仪信息           |
| `MonitorInputCompass`   | 罗盘信息             |
| `MonitorGraphics`       | 显卡信息             |