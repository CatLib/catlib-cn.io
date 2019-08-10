---
title: 贡献指南
---

# 贡献指南

本文档描述了如何对框架提供贡献及框架，组件，文档，bug fixed的贡献情况。

## 缺陷报告

CatLib 强烈鼓励使用 GitHub 的 `pull requests` (以下简称PR)，来提供缺陷报告。“缺陷报告”也可以通过一个包含失败断言的 pull requests 的方式提交。 

如果你以文件的方式提交缺陷报告，你的问题应该包含一个标题和对该问题的明确说明，还要包含尽可能多的相关信息以及论证该问题的代码样板，缺陷报告的目的是为了让你自己和其他人更方便的重现缺陷并对其进行修复。

## 开发讨论

你可以在`issue`上提议新功能或者优化已有功能，如果是新功能的话请至少实现部分代码以便完成新功能开发。

## Pull Request

- 不要进行较大内容的PR ， 除非它是一个全新的组件
- 每个PR只做一件事情
- 确保PR的代码能够通过编译
- 提交PR时，请务必保证针对代码的所有测试都必须通过
- 提交PR时，请务必记录好PR的原因
- 提交的PR，请务必保证单元测试尽可能的覆盖（核心组件90%,常规组件75%）
- 如果提交的PR不能完成覆盖，请描述其原因。
- 所有的PR的单元测试都必须能在VS(MSTest)和Unity(NUnit)中运行
- 请按照代码[风格指南](style.html)正确规范的编写代码

## 提交到的分支

- 所有的 bug 修复应该被提交到最新的稳定分支，永远不要把 bug 修复提交到 master 分支，除非它们能够修复下个发行版本中才存在的特性。
- 当前版本中完全向后兼容的次要特性也可以提交到最新的稳定分支。
- 重要的新特性是要被提交到 master 分支的。
- 如果你不确定是重要特性还是次要特性，请在`calib.slack`或`QQ群`中咨询。

## 合并请求描述

合并请求描述需要附带下面表格，并完善表格中对应的信息：

| Q | A |
|----|----|
| Branch? |  \<version\>  |
| Bug fix? | \<yes \| no\> |
| New feature? | \<yes \| no\> |
| Deprecations? | \<yes \| no\> |
| Tests pass? | \<yes \| no \| n/a\> |
| License? | \<license-name \| n/a\> |
  
\<合并请求修改内容的描述\>

## 框架设计者

- [喵喵大人](https://github.com/yb199478)

## 组件贡献者

> 虚以待位

## 其他贡献者

- A:[AlianBlank](https://github.com/AlianBlank)
- D:[DawnKing](https://github.com/DawnKing)
- I:[iamthekk](https://github.com/iamthekk) [idle](https://github.com/views63)
- L:[liiir1985](https://github.com/liiir1985) [LrvingCong](https://github.com/LrvingCong)
- S:[silingfei](https://github.com/silingfei)
