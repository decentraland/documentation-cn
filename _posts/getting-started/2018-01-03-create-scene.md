---
date: 2018-01-01
title: 创建您的第一个场景
description: 了解 Decentraland 场景的基础知识
redirect_from:
  - /documentation/create-scene/
categories:
  - getting-started
type: Document
set: getting-started
set_order: 1
tag: introduction
---

在 Decentraland，场景是地产或土地的内容的表现。所有场景都由[实体和组件]({{ site.baseurl }}{% post_url /development-guide/2018-02-1-entities-components %})组成，它们代表场景中的所有元素并构成树形结构，非常类似于 Web 开发中 DOM 树中的元素。

## CLI 安装

确保首先安装 CLI 工具。在 Mac OS 中，您可以通过运行以下命令来执行此操作：

```bash
npm install -g decentraland
```

针对 Windows 和 Linux 系统的更多详细信息和特定说明，请参阅[安装指南]({{ site.baseurl }}{% post_url /getting-started/2018-01-01-installation-guide %})。

## 创建默认场景

可以使用我们的 CLI 工具来自动创建初始的场景模板：

1. 新建一个目录
2. 在新建的目录里，打开终端（Mac)或命令行（Windows），运行以下命令：

   ```bash
   dcl init
   ```

`dcl init` 命令在当前工作目录中创建包含一个 **场景** 的 Decentraland **项目**。

> 提示：要创建[XML静态场景]({{ site.baseurl }}{% post_url /development-guide/2018-01-13-xml-static-scenes %})，请运行 `dcl init --boilerplatestatic`。

默认场景在 TypeScript 文件中定义，其中包含一个可以打开的门的示例。 场景具有基本状态并能处理点击事件。

有关在场景中创建的默认文件的概述，请参阅[场景文件]({{ site.baseurl }}{% post_url /development-guide/2018-01-11-scene-files %})。

## 克隆示例场景

您也可以克隆现有的[示例场景]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %})而不创建新场景。

可以这样操作：

1. 转到[示例场景]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %})
2. 找到您喜欢的示例并点击其 **代码** 链接转到它的 GitHub 库。
3. 从那里你可以：
   1. Fork 这个库并在自己的版本上工作。
   2. 单击 **Clone or Download**，然后选择下载 _.zip_ 文件。

## 预览你的场景

要在本地预览渲染的场景，请在场景的主文件夹上运行以下命令：

```bash
dcl start
```

每次对场景进行更改时，预览都会自动重新加载和更新，因此无需再次运行该命令。

有关在场景预览中可以看到的内容以及如何运行远程场景预览的说明，请参阅[场景预览]({{ site.baseurl }}{% post_url /getting-started/2018-01-04-preview-scene %})。

您也可以通过使用 CLI 运行命令将场景部署到第三方服务器。这也是与他人共享场景的好方法，而无需安装任何本地构建场景所需的开发工具。 请参阅[Now 部署]({{ site.baseurl }}{% post_url /deploy/2018-01-01-deploy-to-now %})。

## 编辑你的场景

要编辑场景，我们建议使用源代码编辑器，如 [Visual Studio Code](https://code.visualstudio.com/) 或 [Atom](https://atom.io/)。像这样的编辑器可以帮助您更快地创建场景并减少错误，因为它会标记出语法错误，输入时自动完成，甚至向您提示依赖于上下文的智能建议。使用 Visual Studio Code，您甚至可以通过单击对象以查看其类的完整定义。

- 通过编辑 _game.ts_ 文件创建场景逻辑。
- 您可以在 _scene.json_ 文件中定义场景属性，例如它所包含的领地或所有者信息。

有关向场景添加内容的简单说明，请参阅 _开发指南_。

## 发布场景

完成场景创建后，想要将其上传到 LAND，请参阅[发布]({{ site.baseurl }}{% post_url /deploy/2018-01-07-publishing %})。
