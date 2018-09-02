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

在 Decentraland，场景是地产或 LAND 的内容的表现。 所有场景都由[实体]({{ site.baseurl }}{% post_url /sdk-reference/2018-06-21-entity-interfaces %})组成，它们代表场景中的所有元素并构成树形结构，非常类似于 Web 开发中 DOM 树中的元素。

## CLI 安装

确保首先安装 CLI 工具。在 Mac OS 中，您可以通过运行以下命令来执行此操作：

```bash
npm install -g decentraland
```

针对 Windows 和 Linux 系统的更多详细信息和特定说明，请参阅[安装指南]({{ site.baseurl }}{% post_url /getting-started/2018-01-01-installation-guide %})。

## 创建文件结构

可以使用我们的 CLI 工具来自动创建初始的场景模板。为此，请在空文件目录中运行 `dcl init`。有关如何安装和使用 CLI 的详细信息，请参阅[SDK概述]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-01-SDK-Overview %})。

`dcl init` 命令在当前工作目录中创建包含一个 **场景** 的 Decentraland **项目**。
它提示你回答一系列可选问题，如关于场景所有权的以及Decentraland 最终上传的位置，然后它要求你选择一个场景模板。根据您选择的选项，CLI 构建具有不同默认内容的不同文件结构。

有四种不同的场景模板可供使用：

- **基本场景**：在一个简单的 TypeScript 文件中定义的，用来显示单一的 glTF 模型。
 - **交互式场景**：在 TypeScript 文件中定义，该示例以一个可以打开的门为例。这是开始学习如何使用 SDK 的最佳模板。场景具有基本状态并处理单击事件。场景状态被本地存储在用户的浏览器中，因此用户的行为不会影响其他用户的场景呈现。
- **远程场景**：在 TypeScript 文件中定义，具有与本地场景基本相同的示例，但不同之处在于场景状态存储在与 WebSockets 通信的远程服务器中。因此，所有用户都看到相同的渲染场景。要测试场景，可以同时在本地运行服务器和客户端。 
- **静态场景**：单独一个 glTF 模型定义在 **XML** 文件中。不能将任何动态或交互式内容添加到这类场景中，它只能显示静态实体。

有关在场景中创建的默认文件的概述，请参阅[场景内容]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-11-scene-files %})。

## 预览你的场景

要在本地预览渲染的场景，请在场景的主文件夹上运行以下命令：

```bash
dcl start
```

每次对场景进行更改时，预览都会自动重新加载和更新，因此无需再次运行该命令。

有关在场景预览中可以看到的内容以及如何运行远程场景预览的说明，请参阅[场景预览]({{ site.baseurl }}{% post_url /getting-started/2018-01-04-preview-scene %})。

## 编辑你的场景

要编辑场景，我们建议使用源代码编辑器，如 [Visual Studio Code](https://code.visualstudio.com/) 或 [Atom](https://atom.io/)。像这样的编辑器可以帮助您更快地创建场景并减少错误，因为它会标记出语法错误，输入时自动完成，甚至向您提示依赖于上下文的智能建议。使用 Visual Studio Code，您甚至可以通过单击对象以查看其类的完整定义。

- 在 _基本_ 和 _交互_ 场景中，您可以通过编辑 _Scene.tsx_ 文件来创建场景的逻辑。
- 在 _远程_ 场景中，您可以通过编辑 _RemoteScene.tsx_ 文件和 _State.tsx_ 来创建场景的逻辑。
- 在 _静态_ 场景中，您可以在 _scene.xml_ 文件中创建场景的内容。

有关向场景添加内容的简单说明，请参阅[场景内容指南]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %}) 。


## 发布场景

完成场景创建后，想要将其上传到 LAND，请参阅[发布]({{ site.baseurl }}{% post_url /getting-started/2018-01-07-publishing %})。
