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

CLI 允许您在“链下”的开发环境中编译和预览场景。场景在本地测试后，您可以使用 CLI 将内容上传到内容服务器，并将其链接到自己的土地（LAND）上。

**请注意：** 目前，Decentraland SDK（捆绑在 CLI 安装之中）仅支持 TypeScript。

Decentraland CLI 通过 [npm](https://www.npmjs.com/get-npm?utm_source=house&utm_medium=homepage&utm_campaign=free%20orgs&utm_term=Install%20npm) 分发。

## 安装前的准备

在安装 CLI 之前，请安装以下的依赖项目：

* [Node.js](https://github.com/decentraland/cli#nodejs-installation) (version 8)

## 安装 CLI

打开 _Terminal_ 应用并运行以下命令：

```bash
npm install -g decentraland
```

安装完成后，即可全局使用 `dcl` 命令。

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

如果你的 CLI 是最新的，使用用它新创建的场景将使用最新版本的 SDK。

更新 CLI，现有项目使用的 SDK 版本不会更改。您需要手动更新项目中的 SDK 版本。

> 注意：使用 `npm` 检查已安装的 SDK 版本并不会告诉您在预览特定场景时使用的 SDK 版本。

更新 Decentraland SDK 版本：

要更新项目使用的 Decentraland SDK的版本：

1. 在项目文件夹中打开文件 `package.json`。
2. 在此文件中，查找 `decentraland-ecs`：
   * 如果值为 `latest`，请保留。
   * 如果是数字或版本不是 SDK 的最新版本，请将其更改为`latest`。
   <!--
   * 如果您的项目是[静态XML场景]({{ site.baseurl }}{% post_url /development-guide/2018-01-13-xml-static-scenes %})，则没有此字段配置。请将字段`decentraland-api`设置为`latest`。
   -->
   * 如果你找不到字段 `decentraland-ecs`，但能找到 `decentraland-api`，那么你的项目是用 5.0 以前的旧版本编写的。 使用 CLI 创建一个新项目并[手动迁移](https://decentraland.org/blog/tutorials/sdk-migration)。

3. 从项目中删除文件 `package-lock.json` 和文件夹 `node-modules`。
4. 使用`dcl start`运行场景预览。根据 `package.json` 中指定的版本，应该会自动重新安装所有的依赖项。