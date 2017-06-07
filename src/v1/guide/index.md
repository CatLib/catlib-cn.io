---
title: 介绍
type: guide
order: 0
---

## 介绍

> CatLib要求Unity最低版本为5.3+

### CatLib是什么

CatLib 是一套`渐进式`的`服务提供者框架`。框架为客户端提供多个实现，并把他们从多个实现中解耦出来。服务提供者的改变对它们的客户端是透明的，这样提供了更好的可扩展性。她不仅易于上手，还便于与第三方库或既有项目整合。

### 从Github上Clone

> CatLib:[https://github.com/catlib/catlib](https://github.com/catlib/catlib)

首先先从Github上Clone出CatLib项目吧！

完成Clone后您将发现的Clone文件夹中存在：`CatLib.Unity`和`CatLib.VS` 2个文件夹，她们分别对应的是CatLib Unity 项目 和 CatLib 编译项目。您的项目开发将会基于`CatLib.Unity`开始，而`CatLib.VS`则是用于将CatLib框架编译成`dll`文件

使用Unity打开`CatLib.Unity`项目，首先先运行一次`单元测试`以确保您下载的框架项目没有问题。通过`Window` -> `Test Runner`来打开单元测试窗口，随后请执行`Run All`。请稍等片刻，根据设备性能的不同，单元测试可能会进行1分钟左右。

### 第一次使用

> CatLib是易于上手的。你只需要有良好的 c# 基础。你就可以非常快速地通过阅读这份 指南 投入开发。

尝试学习CatLib的最简单的方法就是去查看框架自带的`Demo`和`服务接口`，您可以在1分钟以内了解基本服务使用方法。

您可以查看教程来了解组件细节使用方案，和自定义组件如何和CatLib产生联系。