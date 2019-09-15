---
date: 2018-01-01
title: 场景预览
description: 您可以在场景预览中看到的内容
redirect_from:
  - /documentation/preview-scene/
categories:
  - getting-started
type: Document
set: getting-started
set_order: 5
tag: introduction
---

一旦你[构建了一个新场景](https://docs.decentraland.org/#create-your-first-scene) 或下载了[示例场景]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %}) ，你就可以在本地预览。

## 准备

请确保已运行以下命令安装了 CLI 工具：

```bash
npm install -g decentraland
```

更多详细信息和特定说明，请参阅[安装指南]({{ site.baseurl }}{% post_url /getting-started/2018-01-01-installation-guide %})。


## 预览场景

在场景主目录运行以下命令来预览场景：

```bash
dcl start
```

此命令会自动安装所有的末安装的依赖项，然后 CLI 会自动在新的浏览器选项卡中打开场景。它会在系统中创建一个本地 Web 服务器，并将 Web 浏览器选项卡地址指向本地服务器。

每次对场景进行更改时，都会自动重新加载和更新预览，因此无需再次运行该命令。

## 预览远程场景

远程场景依赖外部服务器，存储场景的所有用户共享的状态，从而可以进行多人交互。

在本地预览远程场景时，您通常还必须在另一个端口上本地运行服务器。 查看场景的 readme 文件，了解有关如何启动服务器以及场景的说明。

您可以将其他浏览器指向同一本地地址，这会将新用户添加到场景中。 如果用户的操作影响服务器上托管的场景状态，则所有用户都应该看到此效果。

## 上传场景到 decentraland

一旦您对您的场景感到满意，您可以上传并将其发布到 Decentraland，具体操作请参阅[发布]({{ site.baseurl }}{% post_url /deploy/2018-01-07-publishing %}) 。

## 预览命令的参数

您可以将以下标志添加到 `dlc preview` 命令以更改其行为：

- `--no-browser` 阻止预览打开新浏览器选项卡。
- `--port` 分配一个特定的端口来运行场景。否则，它将使用任何可用的端口。
- `--w` or `--no-watch` to not open watch for filesystem changes
- `--c` or `--ci` To run the parcel previewer on a remote unix server

> 要预览使用早期版本 SDK 构建的场景，必须在项目的 `package.json` 文件中设置相应的 `decentraland-ecs` 版本。

## 场景预览的基本用法

预览提供了一些有用的调试信息和工具，可帮助您了解场景的呈现方式。预览模式提供了显示地块边界和场景方向的指示器。

查看预览时，可以按 Esc 键来释放鼠标，以正常使用。

如果您的场景将消息输出到控制台（使用 `log()`），您可以通过打开浏览器的 JavaScript 控制台来查看这些消息。例如，使用 Chrome 时，您可以通过“视图 > 开发者 > JavaScript控制台”来查看。

预览的左上角会通知您以下内容：

- _FPS_ ：每秒帧数
- _MS_  ：网络 ping 延迟，以毫秒为单位
- _MB_  ：以兆字节为单位的内存使用量

单击图表可以切换指标。

## 场景大小预览

预览中显示的场景大小基于场景的配置，您在使用 CLI 构建场景时的设置。默认情况下，场景占用一个地块（16 x 16 米）。

如果您构建的场景上传所用地块比预览的要多，则可以编辑 _scene.json_ 文件，在“parcels”字段中列出所需的多个地块。

```json
 "scene": {
    "parcels": [
      "0,0",
      "0,1",
      "1,0",
      "1,1"
    ],
    "base": "0,0"
  },
```

> 提示：在运行预览时，土地坐标不需要与场景真正使用的坐标相匹配，只要它们相邻并排列成相同的形状即可。 当您[部署场景](#upload-a-scene-to-decentraland)时，将不得不用实际坐标来替换。

## 场景调试

运行预览提供了一些有用的调试信息和工具，可帮助您了解场景的渲染方式。 预览模式提供显示土地边界和场景方向的指示器。

如果无法编译场景，您将只看到地面上的网格，其上没有任何内容。

如果发生这种情况，您可以在几个地方查找错误消息，以帮助您了解出现了什么问题：

1. 检查代码编辑器以确保它没有标记任何语法或逻辑错误。
2. 检查运行 `dcl start` 的命令行的输出
3. 检查浏览器中的 JavaScript 控制台是否有任何其他错误消息。例如，使用 Chrome 时，您可以通过“视图>开发者> JavaScript 控制台”访问。
4. 如果您正在与本地服务器一起运行多人游戏场景的预览，请检查运行本地服务器的命令行窗口的输出。

如果您的场景超出任何[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})，例如，如果其中包含太多三角形，场景不会被渲染。如果发生这种情况，将在场景所在的空间中显示警告标志。

如果实体位于或超出场景的限制，实体将红色闪烁以指示此种情况。场景中的任何内容都不能超出场景限制。场景在本地还可以渲染，但部署到 Decentraland 时会被阻止。

### 使用 console 控制台

如果您的场景将消息输出到控制台（使用 `log()`），您可以通过打开浏览器的 JavaScript 控制台来查看这些消息。 例如，使用 Chrome 时，您可以通过 `视图` > `开发者` > `JavaScript 控制台` 访问。

您还可以添加 `debugger` 命令或使用开发人员工具菜单中的 `sources` 选项卡来添加断点暂停执行。

### 查看场景统计信息

预览的左上角会显示以下内容：

- _FPS_ : 每秒帧数
- _MS_ : 网络 ping 延迟，以毫秒为单位
- _MB_ : 内存使用量，以兆字节为单位

单击图表可切换这些指标。

单击 _P_ 键可以打开面板。 此面板显示有关场景的以下信息，并在变化时实时更新：

- Processed Messages 处理过的消息
- Pending on Queue 消息队列
- Scene location (preview vs deployed)
- Poly Count
- Textures count
- Materials count
- Entities count
- Meshes count
- Bodies count
- Components count

处理过的消息和消息队列是指场景代码发送给引擎的消息。 对了解场景是否正在试图运行超过引擎支持的更多操作。 如果许多消息排队，那通常是一个不好的迹象。

面板中的其他数字是指[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})中资源的使用情况。请记住，这些值的最大允许数量与场景中土地的数量成正比。 如果场景尝试渲染超过这些限值的实体，例如，如果它有太多三角形，则不会渲染它。

> 注意：保持此面板打开会对场景的帧速率和性能产生负面影响，因此我们建议在不使用时关闭它。

<!--
## 查看 collision 碰撞网格

在查看预览时，您可以按 `c` 查看场景的 glTF 模型中加载的碰撞网格。这些通常是不可见的，用来确定玩家哪些地方可以通过，哪些不能通过。

![](/images/media/collision-meshes.png)

碰撞网格可以添加到外部 3D 建模工具（如 Blender）中的任何模型中。像房屋这样的大型模型通常包括碰撞网格，它们通常比原始形状更为简单，因为这样可以减少计算要求。楼梯通常使用简化的碰撞网格，如斜坡，便于攀爬。更多细节，请查看 [colliders]({{ site.baseurl }}{% post_url /3d-modeling/2018-01-12-colliders %}) 。

## 查看边界框

在查看预览时，您可以按 `b` 来查看实体的边界框。边界框显示实体占用的空间，这在处理不可见实体、陷藏在其他实体中或地下的实体时特别有用。
-->

## 连接到以太坊网络

如果您的场景使用以太坊网络上的交易，例如，提示支付 MANA 以打开一扇门，您必须手动将另一个参数添加到预览 URL：`＆ENABLE_WEB3`

例如，如果场景使用以下 URL:
`http://127.0.0.1:8000/?SCENE_DEBUG_PANEL&position=0%2C0`

末尾添加参数，如下所示：
`http://127.0.0.1:8000/?SCENE_DEBUG_PANEL&position=0%2C0&ENABLE_WEB3`


## 使用以太坊测试网络

您可以在预览场景时避免使用真实货币。 为此，您必须使用 _Ethereum Ropsten test network_ 来发送伪 MANA。要使用测试网络，您必须将 Metamask Chrome 扩展程序设置为使用 _Ropsten test network_ 而不是 _Main network_。 并且在 Ropsten 区块链中拥有 MANA，你可以从 Decentraland 免费获得。

<!--
要使用测试网络预览场景，请将 `DEBUG` 属性添加到浏览器上访问场景预览的 URL 上。 如，如果您通过 `http://127.0.0.1:8000/?position=0%2C-1` 访问场景，则应将 URL 设置为 `http://127.0.0.1:8000/?DEBUG&position=0%2C-1`。
-->

在此模式下，查看场景时进行的任何交易都只会出现在测试网络中，而不会影响真实钱包中的 MANA 余额。
