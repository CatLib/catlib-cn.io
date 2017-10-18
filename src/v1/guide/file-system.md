---
title: 文件系统
type: guide
order: 230
enable: true
---

## 文件系统

CatLib提供的文件系统抽象允许使用一个API来对多个本地(云存储)的文件存储系统进行操作。

CatLib文件系统已经集成了处理本地文件系统的简单驱动。

### 初始配置

在使用CatLib文件组件之前您必须对组件进行配置，您可以通过初始配置来配置：

| 配置名                            | 是否必须 | 配置描述(可以点击查看详细)                 |
| -------------------------------- |:------:|:--------------------------------------:|
| `FileSystemProvider.DefaultPath`     | 是      | 默认驱动定位的路径  |
| `FileSystemProvider.DefaultDevice`  | 否      | 默认驱动的名字  |

### 上下文环境

如果没有特殊的说明，本文中的示例项目都是基于下述上下文环境，默认使用本地文件驱动：

``` csharp
var fileSystemManager = App.Make<IFileSystemManager>();
```

您可以通过`IFileSystemManager`的`Disk()`方法来获取文件系统的磁盘实例。

``` csharp
var disk = fileSystemManager.Disk();
```

### 文件系统API

#### **Read**

通过`Read()`方法可以读取文件的字节流。

``` csharp
byte[] data = disk.Read("helloworld.txt");
```

#### **Exists**

通过`Exists()`可以判断文件是否存在。

``` csharp
bool isExists = disk.Exists("helloworld.txt");
```

#### **Delete**

通过`Delete()`可以删除文件。

``` csharp
disk.Delete("helloworld.txt");
```

#### **Write**

通过`Write()`可以写入一个文件，如果文件已经存在那么覆盖数据。

``` csharp
disk.Wrire("helloworld.txt" , Encoding.Default.GetBytes("hello world"));
```

#### **Move**

移动`文件`或`文件夹`到指定路径中。

`Move()`可以被用于重命名。

``` csharp
disk.Move("helloworld.txt" , "NewDir");
```

#### **Copy**

复制`文件`或`文件夹`到指定路径。

``` csharp
disk.Copy("helloworld.txt" , "new-helloworld.txt");
```

#### **MakeDir**

新建一个`文件夹`。

``` csharp
disk.MakeDir("NewDir");
```

#### **GetAttributes**

获取`文件`或`文件夹`的属性。

``` csharp
disk.GetAttributes("helloworld.txt");
```

#### **GetSize**

获取`文件`或`文件夹`的大小（如果是文件夹则是总大小）。

``` csharp
disk.GetSize("helloworld.txt");
```

#### **GetHandler**

获取`文件`或`文件夹`的句柄（可以通过句柄直接操作这个文件或文件夹）

``` csharp
var fileHandler = disk.GetHandler<IFile>("helloworld.txt");
```

``` csharp
var dirHandler = disk.GetHandler<IDirectory>("NewDir");
```

#### **GetList**

通过`GetList()`可以获取指定路径下的所有文件夹和文件的句柄(注意不会迭代子文件夹)。

``` csharp
var list = disk.GetList();
```

### 自定义文件系统

CatLib文件系统允许您自定义您的文件系统驱动，您只需要使用`Extend()`就可以自定义拓展。

``` csharp
fileSystemManager.Extend(()=>
{
    return new FileSystem(new Local("/user/data"));
},"MyDisk");
```

扩展后您可以通过`Disk()`来获取扩展实例。

``` csharp
var newDisk = fileSystemManager.Disk("MyDisk");
```