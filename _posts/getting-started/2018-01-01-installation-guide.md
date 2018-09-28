---
date: 2018-01-01
title: 安装指南
description: 安装 SDK 的分步指南
redirect_from:
  - /documentation/installation-guide/
categories:
  - getting-started
type: Document
set: getting-started
set_order: 2
---

在 Decentraland 上构建开发场景，您首先需要安装命令行接口（CLI）。

CLI 允许您在“链下”的开发环境中编译和预览场景。场景在本地测试后，您可以使用 CLI 将内容上传到 IPFS，并将其链接到自己的土地（LAND）上。

**请注意：** 目前，Decentraland SDK（捆绑在 CLI 安装之中）仅支持 TypeScript。

Decentraland CLI 通过 [npm](https://www.npmjs.com/get-npm?utm_source=house&utm_medium=homepage&utm_campaign=free%20orgs&utm_term=Install%20npm) 分发。

## 安装前的准备

在安装 CLI 之前，请安装以下的依赖项目：

* [Node.js](https://github.com/decentraland/cli#nodejs-installation) (version 8)
* [IPFS](https://dist.ipfs.io/#go-ipfs)
* [Python 2.7.x](https://www.python.org/downloads/)

## 在 Mac OS 上安装 CLI

在Mac OS上，只需运行以下命令：

```bash
npm install -g decentraland
```

安装完成后，即可全局使用 `dcl` 命令。

## 在 Linux 上安装 CLI


如果要在基于 Linux 的操作系统上安装 CLI，请运行

```bash
npm i -g --unsafe-perm decentraland
```

安装完成后，就可全局使用 `dcl` 命令。

## 在 Windows 上安装 CLI

1. 找到命令提示符应用并选择 **以管理员身份运行**

2. 运行以下命令安装 windows-build-tools：
`npm install --global --production windows-build-tools`
...等 Visual Studio Build Tools 和 Python 安装程序都显示 “Successfully installed xxxx”。成功安装后，您将返回到命令提示符。

3. 运行以下命令安装 CLI ：
`npm install -g decentraland`

安装完成后，就可全局使用 `dcl` 命令。

#### 可选：安装 Git

由于 Windows 电脑不使用 bash，我们建议您安装 git 及其包含的 git bash。您可以在 Windows 命令提示符下运行 CLI 命令。

1. 下载 [git](https://git-scm.com/download/win)（您可能需要 64 位 Windows 版本）：
2. 提示选择组件时选择安装 **git bash**
3. 当提示选择 default text editor 时，选择 **Use the Nano editor by default**
4. 在 adjust your path environment 界面，选择 **Use Git from the Windows Command Prompt**
5. 当提示选择 the SSH executable 时，选择 **Use OpenSSH**
6.  在 choose the HTTPS transport backend 选择页面中，请选择 **Use the OpenSSL library**
7.  当提示 configure the line ending conversions 时，选择 **Checkout Windows-style, commit Unix-style line endings**
8.  当提示 configure the terminal emulator to use with Git Bash 时，选择 **Use MinTTY**
9.  在最后安装设置中，请选择以下选项
    * **Enable file system caching**
    * **Enable Git Credential Manager**
    * **Enable symbolic links**


## 在任意操作系统上更新 CLI

要将 CLI 更新到最新支持的版本，请运行以下命令：

```bash
npm update -g decentraland
```

## 更新场景的 SDK 版本

通过更新 CLI，您用它新创建的场景将使用最新版本的 SDK。在更新 CLI 之前创建的项目将继续使用创建它们时所用的 SDK 版本。更升级，唯一方法是手动更新场景中的 SDK 版本。

更新 Decentraland SDK 版本：

1. 在场景文件夹中打开文件 _package-lock.josn_。
2. 查找正在使用的 _metaverse_api_ 版本，并手动将其更改为最新版本的 SDK。

> 提示：如果您不确定最新版本是哪个，请查看[发行说明]{{ site.baseurl }}{% post_url /releases/sdk/2018-01-01-4.0.0 %})。

>注意：使用 `npm` 查看已安装的 SDK 版本有可能跟预览场景时使用的 SDK 版本不一致。要了解所使用的版本的唯一方法是查看 _package-lock.josn_ 文件。