---
title: 介绍
type: guide
order: 0
---

## 介绍

<a href="https://github.com/yb199478/CatLib/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" title="license-mit" /></a> <a href="https://github.com/yb199478/catlib/"><img src="https://badge.fury.io/gh/catlib%2Fcatlib.svg" title="GitHub version" /></a> <a href="https://ci.appveyor.com/project/yb199478/catlib"><img src="https://ci.appveyor.com/api/projects/status/f12rb3x5hxvq6yr7?svg=true" title="Build status"/></a> <a href="https://codecov.io/gh/CatLib/CatLib"><img src="https://codecov.io/gh/CatLib/CatLib/branch/master/graph/badge.svg" alt="Codecov" /></a> <img src="https://img.shields.io/badge/unity-min%205.3-red.svg" alt="min unity" />

> CatLib要求Unity最低版本为5.3+

### CatLib是什么

CatLib 是一套`渐进式`的`服务提供者框架`。框架为客户端提供多个实现，并把他们从多个实现中解耦出来。服务提供者的改变对它们的客户端是透明的，这样提供了更好的可扩展性。她不仅易于上手，还便于与第三方库或既有项目整合。

### CatLib的优势

- CatLib是渐进式的框架，可以无缝和现有框架融合。无论您的项目处于哪个阶段您都可以轻易的接入CatLib。
- CatLib提供的依赖注入方案，可以极大程度的帮助项目解耦。
- CatLib提供了大量可靠，可持续的公共组件，帮助企业降低开发成本。
- 基于MIT协议，企业可以通过CatLib的组件化方案建立私有的公共组件库，积攒公共组件。
- 轻量级的框架，所有的组件都是可以被移除的，您可以只选择适合您的组件。
- 中文文档完善，极低的学习成本。
- 面向接口编程，底层组件无感知替换。

### 从Github上Clone

> CatLib:[https://github.com/catlib/catlib](https://github.com/catlib/catlib)

首先，先从Github上Clone出CatLib项目吧！

完成Clone后，您将会发现Clone的文件夹中存在：`CatLib.Unity`和`CatLib.VS` 2个文件夹，她们分别对应的是`CatLib Unity项目` 和 `CatLib编译项目`。

您的项目开发将会基于`CatLib.Unity`开始，而`CatLib.VS`则是用于将CatLib框架编译成`dll`文件。一般情况下CatLib的源码将会在`CatLib.VS`文件中。

使用Unity打开`CatLib.Unity`项目，首先先运行一次`单元测试`以确保您下载的框架项目没有问题。通过`Window` -> `Test Runner`来打开单元测试窗口，随后请执行`Run All`。

请稍等片刻，根据设备性能的不同，单元测试可能会进行1分钟左右。

### 第一次使用

CatLib是易于上手的。你只需要有良好的 C# 基础。你就可以非常快速地通过阅读这份 指南 投入开发。
