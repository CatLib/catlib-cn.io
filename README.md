# CatLib文档 [![Build Status](https://www.travis-ci.org/CatLib/catlib-zh.io.svg?branch=1.3)](https://www.travis-ci.org/CatLib/catlib-zh.io)

这是CatLib的文档网站，文档是使用[hexo](http://hexo.io/)构建的。 文档内容在`source`文件夹中，使用Markdown格式编写。

文档地址: [catlib.io](https://catlib.io)

## 环境需求

- [Nodejs(8.0.2+)](https://nodejs.org/en/)
- [Git(2.13.1+)](https://nodejs.org/en/)

## 快速开始

- clone 文档项目

```shell
$ git clone https://github.com/CatLib/catlib-zh.io.git
```

- 安装项目依赖包

```shell
$ cd catlib-zh.io && npm install
```

- 启动开发者服务器

```shell
$ hexo s
```

如果您打开浏览器并访问`http://localhost:4000`,您应该看到文档网站已启动并正在运行。

## 搜索支持(可选)

**环境需求: node-gyp**

- 安装windows-build-tools(仅限windows)

```shell
npm install -g -production windows-build-tools
```

- 安装node-gyp

```shell
npm install -g node-gyp
```

- 重启您的命令行

**安装依赖包**

- 安装js-yaml

```shell
$ npm install js-yaml --save-dev
```

- 安装nodejieba

```shell
$ npm install nodejieba --save-dev
```

> 当完成依赖包安装后，搜索功能会自动生效

## 欢迎贡献

我们欢迎以任何形式的贡献，您的贡献将会被列入贡献者名单。如果您想要贡献，请`fork`本库，并以`pull request`的形式提交贡献。
