---
date: 2018-01-01
title: SDK 概述
redirect_from:
  - /docs/sdk-overview
  - /docs/command-line-interface
  - /docs/sdk-quick-start-guide
description: Decentraland 软件开发套件
categories:
  - sdk-reference
type: Document
set: sdk-reference
set_order: 1
---

## 介绍

**Decentraland 软件开发工具包**包括 alpha 版本的 API、CLI 等工具，以及在 Decentraland 中创建、测试、管理和部署 3D 内容所需的所有支持文档和示例。SDK 允许内容创建者和开发者创建可在 Decentraland 平台上运行的静态或动态场景。

#### 系统要求

请注意，开发场景并不需要 LAND。 开发和测试场景可以完全脱机完成，无需将场景部署到以太坊网络（Decentraland 以此来确认 LAND 的所有权）或 IPFS（我们用于分发和传送内容的 P2P 网络）。

此 SDK 旨在使开发人员或用户能轻松使用代码和终端。我们的大多数工具都是基于TypeScript 和 node.js 生态系统构建的。这就是为什么你需要安装 **npm**（node 包管理系统）来开发场景，即便是对于使用标记而不是代码构建的基本静态场景也是如此。

#### 功能

在顶层，SDK 允许您执行以下操作：

- 生成包含你的第一个场景的默认_项目_，包括了渲染和运行场景所需要的所有的资源。
- 构建、测试和在 Web 浏览器中预览场景 - 完全脱机，无需进行任何以太坊交易或拥有 LAND。
- 使用 API 编写 TypeScript 脚本，为场景添加交互式、动态功能。
- 将场景内容上传到[IPFS](https://ipfs.io)。
- 将您的 LAND 与您上传的内容的 IPFS URL 相关联。

#### 什么是场景?

Decentraland 中的**场景**是在一个或多个 LAND 地块上渲染的 3D 对象、纹理和音频的集合。场景可以是：

- _本地场景_：在用户的客户端内运行。您的场景在 WebWorker 内运行，意味着您不需要任何服务器。
- _远程场景_：有助于创建更丰富的体验，适用于需要集中协调场景的状态，或者由于安全原因不能在每个用户的计算机中执行的情况。这些场景在 Node.js 服务器中运行，并通过 WebSockets 接口与客户端连接。
- _静态场景_：仅包含 3D 对象和音频，没有交互的场景。

场景由 CLI 在“项目”中创建。

##### 场景放哪？

场景会被部署到**地块**上，即 10 米乘 10 米的虚拟 LAND，LAND 是由以太智能合约维护的稀缺和不可替代的资产。使用 SDK 创建的交互内容最终会上传到土地的虚拟空间上。

##### 用户怎样才能看到这些场景并与之互动？

我们正在开发 Decentraland 的 Web 客户端。 您上传到 LAND 的所有内容都能通过此客户端进行显示和浏览。SDK 中还包含了预览工具，您可以同时预览、测试您的内容并与之交互。

**Decentraland 客户端仍处于积极开发之中。预计将在 2018 年底前公开发布。请继续关注更新！**

有关其他术语、定义和解释，请参阅我们的[完整词汇表]({{ site.baseurl }}{% post_url /general/2018-01-03-glossary %})。

## SDK 安装

SDK 中包含许多不同的部件和组件。有关如何下载和安装 SDK 的详细分步说明，请参阅 [SDK 快速入门指南]({{ site.baseurl }}{% post_url getting-started/2018-01-01-installation-guide %})。

#### CLI

Decentraland 命令行界面（CLI）能让您在不使用区块钱链和 IPFS（星际文件系统）的开发环境中创建，部署和管理场景。

在您自己的机器上本地生成新的 Decentraland 场景后，您就可以使用您喜欢的文本编辑器编辑场景。在本地测试场景后，您可以使用 CLI 将内容上传到 IPFS。

有关安装 CLI 的更多分步说明，请阅读我们的 [SDK 快速入门指南]({{ site.baseurl }}{% post_url getting-started/2018-01-01-installation-guide %})或 [CLI 教程](https://docs.decentraland.org/v1.0/docs/command-line-interface)。

#### API

`decentraland-api`（以前被称为 `metaverse-api`，通常称为 API）是一个 TypeScript 包，其中包含辅助函数库，能让您创建交互式单人游戏和多人游戏体验。除了有助于促进用户或其他应用程序之间的内部交易的方法之外，API 还包括允许您在 LAND 上创建和操作对象的方法。

## API 元素

API 包含 `decentraland-api` 包中的所有内容。包含了编写 TypeScript 场景时所需的所有类和辅助方法，无论场景是静态的，动态的还是远程的。

我们以实体组件系统进行场景开发来设计 Decentraland API，其中**实体**包括用户将能够在其Web 浏览器中与之交互的所有资产（如音频文件，视频文件和 3D 对象），**组件**充当客户端的底层函数与您编写的脚本之间的桥梁，以控制场景中的实体。

#### 什么是实体？

实体是您场景中包含的所有资产，用户可以通过其 Web 浏览器进行渲染、查看和交互，包括音频文件和 3D 对象。

在内部，您呈现的场景是通过包含所有实体的树生成的。

#### 什么是组件？

有关 API 的完整参考，以及每个组件和方法的说明，请参阅 [API 参考](https://decentraland.github.io/cli/)。
