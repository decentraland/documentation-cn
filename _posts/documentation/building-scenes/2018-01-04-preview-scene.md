---
date: 2018-01-01
title: 场景预览
description: 您可以在场景预览中看到的内容
categories:
  - documentation
type: Document
set: building-scenes
set_order: 4
tag: introduction
---

Once you have [built a new scene]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-03-create-scene %}) ) or downloaded a [sample scene]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %}) ) you can preview it locally.

一旦你[构建了一个新场景]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-03-create-scene %}) 或下载了[示例场景]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %}) ，你就可以在本地预览。

## Before you begin
## 开始之前

Please make sure you first install the CLI tools. In Mac OS, you do this by running the following command:

请确保首先安装了 CLI 工具。 在 Mac OS 中，您可以运行以下命令安装：

```bash
npm install -g decentraland
```

See the [Installation Guide]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-01-installation-guide %}) for more details and specific instructions for Windows and Linux systems.

有关 Windows 和 Linux 系统的更多详细信息和特定说明，请参阅[安装指南]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-01-installation-guide %})。


## Preview a local scene
## 预览本地场景


To preview a local scene run the following command on the scene's main folder:

在场景主目录运行以下命令来预览本地场景：

```bash
dcl start
```

Any dependencies that are missing are installed and then the CLI opens the scene in a new browser tab automatically. It creates a local web server in your system and points the web browser tab to this local address.

缺少的所有的依赖项将自动安装，然后 CLI 将自动在新的浏览器选项卡中打开场景。它在您的系统中创建一个本地 Web 服务器，并将 Web 浏览器选项卡指向此本地地址。

Every time you make changes to the scene, the preview reloads and updates automatically, so there's no need to run the command again.

每次对场景进行更改时，都会自动重新加载和更新预览，因此无需再次运行该命令。

## Preview a remote scene
## 远程场景预览

To preview a remote scene you must first start a WebSockets server in your local machine. To do this:

要预览远程场景，必须首先在本地计算机中启动 WebSockets 服务器。

From the scene directory run the following bash commands:

从场景目录运行以下 bash 命令：

```bash
cd server

npm install
# npm will find your dependencies

npm run build
# npm will build the socket server

npm start
# now the port is running
```

Note that when the server is running, the command informs you what port that the server is running on, take note of this. It will probably be 8087.

请注意，当服务器运行时，该命令会通知您运行服务器的端口，请注意这一点。 它可能是 8087。

Check that the `scene.json` file in your scene has the same websocket port set up, otherwise change the file so that it matches the local server you're running.

检查场景中的 `scene.json` 文件是否设置了相同的 websocket 端口，否则请更改文件以使其与您正在运行的本地服务器相匹配。

Once the websocket server is up and the scene is properly pointing at it, fire up the preview as you would with a local scene.

一旦 websocket 服务器启动并且场景正确指向它，就可以像启动本地场景一样启动预览。

From the scene directory run the following bash command:

从场景目录运行以下 bash 命令：

```bash
dcl start
```

Any missing dependencies are installed and then the CLI opens the scene in a new browser tab automatically.

缺少的所有的依赖项将自动安装，然后 CLI 将自动在新的浏览器选项卡中打开场景。

You can point other browser tabs to the same local address and this will add new users to the scene. User's aren't able to see other avatars in the preview, but they can see a marker representing other users. If the actions of a user affect the scene state, which is hosted on the server, all users should see the effects of this.

您可以在其他浏览器选项卡中输入同一本地地址，这会将新用户添加到场景中。 用户无法在预览中看到其他用户化身，但他们可以看到代表其他用户的标记。如果用户的操作影响了服务器上托管的场景状态，则所有用户都能看到此效果。

> Note: Currently, the preview of remote scenes doesn't automatically update changes to the scene as local scenes do. Each time you make changes, you must run the build and start commands for the server again. It's advisable to start building a scene as local and only move its code to a remote scene once it's mostly complete.

> 注意：目前，远程场景的预览不会像本地场景那样自动更新对场景的更改。每次进行更改时，都必须再次运行服务器的构建和启动命令。建议将场景构建为本地场景，并且只有在大部分完成后才将其代码移动到远程场景。

## Upload a scene to decentraland
## 上传场景到 decentraland

Once you're happy with your scene, you can upload it and publish it to Decentraland, see [publishing]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-07-publishing %}) ) for instructions on how to do that.

一旦您对您的场景感到满意，您可以上传并将其发布到 Decentraland，具体操作请参阅[发布]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-07-publishing %}) 。

## Parameters of the preview command
## 预览命令的参数

You can add the following flags to the `dcl prevew` command to change its behavior:

您可以将以下标志添加到 `dlc preview` 命令以更改其行为：

- `--no-browser` to prevent the preview from opening a new browser tab.
- `--port` to assign a specific port to run the scene. Otherwise it will use whatever port is available.
- `--skip` to skip the confirmation prompt.

- `--no-browser` 阻止预览打开新浏览器选项卡。
- `--port` 分配一个特定的端口来运行场景。否则，它将使用任何可用的端口。
- `--skip` 跳过确认提示。

> To preview old scenes that were built for older versions of the SDK, you must install the latest versions of the `metaverse-api` and `metaverse-rpc` packages in your project. Check the CLI version via the command `dcl -v`

> 要预览为早期版本的 SDK 构建的旧场景，必须在项目中安装最新版本的`metaverse-api` 和 `metaverse-rpc` 包。 通过命令 `dcl -v` 检查 CLI 版本

## Basic usage of the scene preview
## 场景预览的基本用法

Running a preview provides some useful debugging information and tools to help you understand how the scene is rendered. The preview mode provides indicators that show parcel boundaries and the orientation of the scene.

预览提供了一些有用的调试信息和工具，可帮助您了解场景的呈现方式。预览模式提供显示地块边界和场景方向的指示器。

When viewing a preview, you can press the Esc key to disengage the mouse and use it normally.

查看预览时，可以按 Esc 键取消鼠标以正常使用。

If an entity is located or extends beyond the limits of the scene, it will flash with red color to indicate this. Nothing in your scene can extend beyond the scene limits. If you're building a scene to be uploaded to an estate that occupies more parcels than the preview shows, you can edit the `scene.json` file to reflect this, listing multiple parcels in the "parcels" field.

如果实体位于或超出场景的限制，它将用红色闪烁给以提示。场景中的任何内容都不能超出场景限制。如果你正在创建一个场景，该场景将被上传到一个比预览显示的更多的地块中，您可以编辑 `scene.json` 文件以反映这一点，在 “parcels” 字段中列出多个地块。

If your scene excedes any of the [scene limitations]({{ site.baseurl }}{% post_url /documentation/sdk-reference/2018-01-06-scene-limitations %}) ), for example if there are too many triangles in it, the scene won't be rendered.

如果您的场景超出[场景限制]({{ site.baseurl }}{% post_url /documentation/sdk-reference/2018-01-06-scene-limitations %}) )，例如，如果有太多的三角形，场景将不会被渲染。

If your scene outputs messages to console (using `console.log()`) you can view these messages as they are generated by opening the JavaScript console of your browser. For example, when using Chrome you access this through `View > Developer > JavaScript console`.

如果您的场景将消息输出到控制台（使用`console.log()`），您可以通过打开浏览器的 JavaScript 控制台来查看这些消息。例如，使用 Chrome 时，您可以通过“视图>开发者> JavaScript控制台”访问它。


The preview informs you of the frames per second (FPS) of your scene. The performance depends in part on your local machine, but a bad number of FPS may be an indication that the scene is too complicated to render smoothly.

预览会通知您场景的每秒帧数（FPS）。 性能取决于您的本地机器，FPS 数值太小可能表明场景太复杂而无法平滑渲染。

## View collision meshes
## 查看碰撞网格

While viewing the preview, you can press `c` to view any collision meshes loaded in the glTF models of the scene. These are usually invisible, but determine where an avatar can move through, and where it can't.

在查看预览时，您可以按 `c` 查看场景的 glTF 模型中加载的任何“碰撞网格”。这些通常是不可见的，可以用赤确定玩家哪些位置可以通过，哪些不能通过。

![](/images/media/collision-meshes.png)

Collision meshes can be added to any model in an external 3D modeling tool like Blender. Large models like houses often include these, they are usually a lot simpler geometrically than the original shape, as this implies much less computational requirements. Stairs typically use a simplified collision mesh like a ramp to make it easier to climb, otherwise a character would have to jump up every step.

“碰撞网格”可以添加到外部 3D 建模工具（如Blender）中的任何模型中。像房屋这样的大型模型通常包括这些，它们通常比原始形状更简单，因为这意味着更少的计算要求。楼梯通常使用简化的碰撞网格，如斜坡，以便更容易攀爬，否则玩家必须在每一步都跳起来。

## View bounding boxes
## 查看边界框

While viewing the preview, you can press `b` to view any bounding boxes. Bounding boxes show the space occupied by an entity, it's especially useful to see these when dealing with invisible entities.

在查看预览时，您可以按“b”查看任何边界框。 边界框显示实体占用的空间，在处理不可见实体时查看这些空间特别有用。

## Using the Ethereum test network
## 使用以太坊测试网络

If your scene makes use of transactions over the Ethereum network, for example if it prompts you to pay a sum in MANA to open a door, you can avoid using real currency while previewing the scene.

如果您的场景使用以太坊网络上的交易，例如，如果它提示您在支付一笔 MANA 款项以打开一扇门，则可以在预览场景时避免使用真实货币。

For this, you must use the _Ethereum Ropsten test network_ and transfer fake MANA instead. To use the test network you must set your Metamask Chrome extension to use the _Ropsten test network_ instead of _Main network_. You must also own MANA in the Ropsten blockchain, which you can aquire for free from Decentraland.

为此，您必须使用_以太 Ropsten 测试网络_并转移伪造的 MANA 。 要使用测试网络，您必须将 Metamask Chrome 扩展程序设置为使用_Ropsten test network_而不是 _Main network_。 您还必须在 Ropsten 区块链中拥有 MANA，您可以从Decentraland 免费获得。

To preview your scene using the test network, add the `DEBUG` property to the URL you're using to access the scene preview on your browser. For example, if you're accessing the scene via `http://127.0.0.1:8000/?position=0%2C-1`, you should set the URL to `http://127.0.0.1:8000/?DEBUG&position=0%2C-1`.

要使用测试网络预览场景，请将 `DEBUG` 属性添加到您用于访问浏览器上的场景预览的 URL。 例如，如果您通过 `http://127.0.0.1:8000/?position=0%2C-1` 访问场景，则应将 URL 设置为 `http://127.0.0.1:8000/?DEBUG&position=0%2C-1`。

Any transactions that you accept while viewing the scene in this mode will only occur in the test network and not affect the MANA balance in your real wallet.

在此模式下查看场景时接受的任何交易都只会出现在测试网络中，而不会影响真实钱包中的 MANA 余额。