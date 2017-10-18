---
title: 文件系统扩展
type: guide
order: 530
enable: true
---

## 文件系统扩展(Unity增强)

在Unity项目下您可以直接通过可视化配置对文件系统进行配置。

### PathType

内嵌的路径类型。

- `DataPath` : Unity项目数据路径;
- `StreamingAssets` : Unity项目数据路径下的SteamingAssets路径;
- `PersistentDataPath` : 可读可写路径(Application.persistentDataPath)
- `Auto` : 自动识别（推荐）

### DefaultDevice

默认的驱动设备名字，在框架启动时会自动载入一个本地驱动器，本地驱动器使用`PathType`提供的路径进行定位，驱动器的名字则为`DefaultDevice`设定的值。