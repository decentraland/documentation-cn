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

一旦你[构建了一个新场景]({{ site.baseurl }}{% post_url /getting-started/2018-01-03-create-scene %}) 或下载了[示例场景]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %}) ，你就可以在本地预览。

## 开始前

请确保首先安装了 CLI 工具。 在 Mac OS 中，您可以运行以下命令安装：

```bash
npm install -g decentraland
```

有关在 Windows 和 Linux 系统上安装的更多详细信息和特定说明，请参阅[安装指南]({{ site.baseurl }}{% post_url /getting-started/2018-01-01-installation-guide %})。


## 预览场景

在场景主目录运行以下命令来预览场景：

```bash
dcl start
```

此命令会自动安装所有的末安装的依赖项，然后 CLI 会自动在新的浏览器选项卡中打开场景。它会在系统中创建一个本地 Web 服务器，并将 Web 浏览器选项卡地址指向本地服务器。

每次对场景进行更改时，都会自动重新加载和更新预览，因此无需再次运行该命令。

## 预览远程场景

远程场景依赖外部服务器，存储场景的所有用户共享的状态，从而可以进行多人交互。

预览远程场景的最简单方法是在本地运行服务器。

在场景目录下运行以下 bash 命令：

```bash
cd server

npm install
# npm will find your dependencies

npm run build
# npm will build the server

npm start
# now the port is running
```

请注意，当服务器运行时，该命令会提示您服务器运行的端口，请注意一下。 一般可能是 8087。

检查场景中的 `scene.json` 文件是否设置了相同的 websocket 端口，否则请更改文件以使其与您正在运行的本地服务器端口相匹配。

一旦服务器启动并且场景正确指向它，就可以像启动本地场景一样启动预览。

回到场景目录运行以下 bash 命令：

```bash
dcl start
```

将自动安装所有的末安装的依赖项，然后 CLI 将自动在新的浏览器选项卡中打开场景。

您可以在其他浏览器选项卡中输入同样的本地地址，这会将新用户添加到场景中。 用户无法在预览中看到其他用户的形象，但他们可以看到代表其他用户的标记。如果用户的操作影响了服务器上托管的场景状态，则所有用户都能看到此效果。

> 注意：目前，远程场景的预览不会像本地场景那样对场景的更改自动更新。每次进行更改时，都必须再次运行服务器的构建和启动命令。建议将场景构建为本地场景，并且只有在大部分完成后才将其代码移动到远程场景。

## 上传场景到 decentraland

一旦您对您的场景感到满意，您可以上传并将其发布到 Decentraland，具体操作请参阅[发布]({{ site.baseurl }}{% post_url /deploy/2018-01-07-publishing %}) 。

## 预览命令的参数

您可以将以下标志添加到 `dlc preview` 命令以更改其行为：

- `--no-browser` 阻止预览打开新浏览器选项卡。
- `--port` 分配一个特定的端口来运行场景。否则，它将使用任何可用的端口。
- `--w` or `--no-watch` to not open watch for filesystem changes
- `--c` or `--ci` To run the parcel previewer on a remote unix server

> 要预览为早期版本 SDK 构建的旧场景，必须在项目中安装最新版本的`decentraland-ecs`。 可以使用命令 `dcl -v` 检查 CLI 版本。

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

预览中显示的场景大小基于场景的配置，您在使用 CLI 构建场景时的设置。默认情况下，场景占用一个地块（10 x 10 米）。

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

## 场景调试

如果无法编译场景，则浏览器中的场景将显示为占据场景空间的大白框。

如果发生这种情况，您可以在几个地方查找错误消息，以帮助您了解出现了什么问题：

1. 检查代码编辑器以确保它没有标记任何语法或逻辑错误。
2. 检查运行 `dcl start` 的命令行的输出
3. 检查浏览器中的 JavaScript 控制台是否有任何其他错误消息。例如，使用 Chrome 时，您可以通过“视图>开发者> JavaScript 控制台”访问。
4. 如果您正在运行远程场景的预览，请检查另一个运行 `npm start` 以启动服务器的命令行窗口的输出

如果您的场景超出任何[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})，例如，如果其中包含太多三角形，场景不会被渲染。如果发生这种情况，将在场景所在的空间中显示警告标志。

如果实体位于或超出场景的限制，实体将红色闪烁以指示此种情况。场景中的任何内容都不能超出场景限制。场景在本地还可以渲染，但部署到 Decentraland 时会被阻止。

您还可以在代码中添加 `log()` 命令，以便打印信息到 JavaScript 控制台。

You can also add `debugger` commands or use the `sources` tab in the developer tools menu to add breakpoints and pause execution while you interact with the scene in real time.

<!---
以下视频介绍了调试场景的不同方法：

{%  include youtube.html video_id='UIJ_dGOFjKM'  %}
-->

## 查看 collision 碰撞网格

在查看预览时，您可以按 `c` 查看场景的 glTF 模型中加载的碰撞网格。这些通常是不可见的，用来确定玩家哪些地方可以通过，哪些不能通过。

![](/images/media/collision-meshes.png)

碰撞网格可以添加到外部 3D 建模工具（如 Blender）中的任何模型中。像房屋这样的大型模型通常包括碰撞网格，它们通常比原始形状更为简单，因为这样可以减少计算要求。楼梯通常使用简化的碰撞网格，如斜坡，便于攀爬。更多细节，请查看 [外部 3D 模型]({{ site.baseurl }}{% post_url /development-guide/2018-01-09-external-3d-models %}) 。

## 查看边界框

在查看预览时，您可以按 `b` 来查看实体的边界框。边界框显示实体占用的空间，这在处理不可见实体、陷藏在其他实体中或地下的实体时特别有用。

## 使用以太坊测试网络

如果您的场景使用了以太坊网络上的交易，例如，提示需要支付一笔 MANA 才能打开一扇门，你可以在预览场景时避免使用真实货币。

为此，您必须使用_以太 Ropsten 测试网络_来发送伪 MANA。 要使用测试网络，您必须将 Metamask Chrome 扩展程序设置为使用 _Ropsten test network_ 而不是 _Main network_。 并且在 Ropsten 区块链中拥有 MANA，你可以从 Decentraland 免费获得。

要使用测试网络预览场景，请将 `DEBUG` 属性添加到浏览器上访问场景预览的 URL 上。 如，如果您通过 `http://127.0.0.1:8000/?position=0%2C-1` 访问场景，则应将 URL 设置为 `http://127.0.0.1:8000/?DEBUG&position=0%2C-1`。

在此模式下，查看场景时进行的任何交易都只会出现在测试网络中，而不会影响真实钱包中的 MANA 余额。
