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
## 开始前

Please make sure you first install the CLI tools. In Mac OS, you do this by running the following command:

请确保首先安装了 CLI 工具。 在 Mac OS 中，您可以运行以下命令安装：

```bash
npm install -g decentraland
```

See the [Installation Guide]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-01-installation-guide %}) for more details and specific instructions for Windows and Linux systems.

有关在 Windows 和 Linux 系统上安装的更多详细信息和特定说明，请参阅[安装指南]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-01-installation-guide %})。


## Preview a local scene
## 预览本地场景


To preview a local scene run the following command on the scene's main folder:

在场景主目录运行以下命令来预览本地场景：

```bash
dcl start
```

Any dependencies that are missing are installed and then the CLI opens the scene in a new browser tab automatically. It creates a local web server in your system and points the web browser tab to this local address.

此命令会自动安装所有的末安装的依赖项，然后 CLI 会自动在新的浏览器选项卡中打开场景。它会在系统中创建一个本地 Web 服务器，并将 Web 浏览器选项卡地址指向本地服务器。

Every time you make changes to the scene, the preview reloads and updates automatically, so there's no need to run the command again.

每次对场景进行更改时，都会自动重新加载和更新预览，因此无需再次运行该命令。

## Preview a remote scene
## 远程场景预览

To preview a remote scene you must first start a WebSockets server in your local machine. To do this:

要预览远程场景，必须首先在本地计算机中启动 WebSockets 服务器。

From the scene directory run the following bash commands:

在场景目录下运行以下 bash 命令：

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

请注意，当服务器运行时，该命令会提示您服务器运行的端口，请注意一下。 一般可能是 8087。

Check that the `scene.json` file in your scene has the same websocket port set up, otherwise change the file so that it matches the local server you're running.

检查场景中的 `scene.json` 文件是否设置了相同的 websocket 端口，否则请更改文件以使其与您正在运行的本地服务器端口相匹配。

Once the websocket server is up and the scene is properly pointing at it, fire up the preview as you would with a local scene.

一旦 websocket 服务器启动并且场景正确指向它，就可以像启动本地场景一样启动预览。

From the scene directory run the following bash command:

从场景目录运行以下 bash 命令：

```bash
dcl start
```

Any missing dependencies are installed and then the CLI opens the scene in a new browser tab automatically.

将自动安装所有的末安装的依赖项，然后 CLI 将自动在新的浏览器选项卡中打开场景。

You can point other browser tabs to the same local address and this will add new users to the scene. User's aren't able to see other avatars in the preview, but they can see a marker representing other users. If the actions of a user affect the scene state, which is hosted on the server, all users should see the effects of this.

您可以在其他浏览器选项卡中输入同样的本地地址，这会将新用户添加到场景中。 用户无法在预览中看到其他用户的形象，但他们可以看到代表其他用户的标记。如果用户的操作影响了服务器上托管的场景状态，则所有用户都能看到此效果。

> Note: Currently, the preview of remote scenes doesn't automatically update changes to the scene as local scenes do. Each time you make changes, you must run the build and start commands for the server again. It's advisable to start building a scene as local and only move its code to a remote scene once it's mostly complete.

> 注意：目前，远程场景的预览不会像本地场景那样对场景的更改自动更新。每次进行更改时，都必须再次运行服务器的构建和启动命令。建议将场景构建为本地场景，并且只有在大部分完成后才将其代码移动到远程场景。

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

- `--no-browser` 阻止预览打开新浏览器选项卡。
- `--port` 分配一个特定的端口来运行场景。否则，它将使用任何可用的端口。

> To preview old scenes that were built for older versions of the SDK, you must install the latest versions of the `metaverse-api` and `metaverse-rpc` packages in your project. Check the CLI version via the command `dcl -v`

> 要预览为早期版本 SDK 构建的旧场景，必须在项目中安装最新版本的`metaverse-api` 和 `metaverse-rpc` 包。 可以使用命令 `dcl -v` 检查 CLI 版本。

## Basic usage of the scene preview
## 场景预览的基本用法

Running a preview provides some useful debugging information and tools to help you understand how the scene is rendered. The preview mode provides indicators that show parcel boundaries and the orientation of the scene.

预览提供了一些有用的调试信息和工具，可帮助您了解场景的呈现方式。预览模式提供了显示地块边界和场景方向的指示器。

When viewing a preview, you can press the Esc key to disengage the mouse and use it normally.

查看预览时，可以按 Esc 键来释放鼠标，以正常使用。

If your scene outputs messages to console (using `console.log()`) you can view these messages as they are generated by opening the JavaScript console of your browser. For example, when using Chrome you access this through `View > Developer > JavaScript console`.

如果您的场景将消息输出到控制台（使用`console.log()`），您可以通过打开浏览器的 JavaScript 控制台来查看这些消息。例如，使用 Chrome 时，您可以通过“视图 > 开发者 > JavaScript控制台”来查看。

The upper-left corner of the preview informs you of the following:

预览的左上角会通知您以下内容：

- _FPS_ : frames per second
- _MS_ : network ping delay in milliseconds
- _MB_ : memory usage in megabytes

- _FPS_ ：每秒帧数
- _MS_  ：网络 ping 延迟，以毫秒为单位
- _MB_  ：以兆字节为单位的内存使用量

Click on the graph to switch through these metrics.

单击图表可以切换指标。

## Preview scene size
## 场景大小预览

The scene size shown in the preview is based on the scene's configuration, you set this when building the scene using the CLI. By default, the scene occupies a single parcel (10 x 10 meters).

预览中显示的场景大小基于场景的配置，您在使用 CLI 构建场景时的设置。默认情况下，场景占用一个地块（10 x 10 米）。

If you're building a scene to be uploaded to an estate that occupies more parcels than the preview shows, you can edit the _scene.json_ file to reflect this, listing multiple parcels in the "parcels" field.

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

## Debug a scene

If the scene can't be compiled, the scene in your browser will be rendered as a large white box that occupies the entire space of the scene.

如果无法编译场景，则浏览器中的场景将显示为占据场景空间的大白框。

If this occurs, there are several places where you can look for error messages to help you understand what went wrong:

如果发生这种情况，您可以在几个地方查找错误消息，以帮助您了解出现了什么问题：

1.  Check your code editor to make sure that it didn't mark any syntax or logic errors.
2.  Check the output of the command line where you ran `dcl start`
3.  Check the JavaScript console in the browser for any other error messages. For example, when using Chrome you access this through `View > Developer > JavaScript console`.
4.  If you're running a preview of a remote scene, check the output of the other command line window, where you ran `npm start` to start the server.

1. 检查代码编辑器以确保它没有标记任何语法或逻辑错误。
2. 检查运行 `dcl start` 的命令行的输出
3. 检查浏览器中的 JavaScript 控制台是否有任何其他错误消息。例如，使用 Chrome 时，您可以通过“视图>开发者> JavaScript 控制台”访问。
4. 如果您正在运行远程场景的预览，请检查另一个运行 `npm start` 以启动服务器的命令行窗口的输出

If your scene excedes any of the [scene limitations]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-06-scene-limitations %}), for example if there are too many triangles in it, the scene won't be rendered. If this occurs, a warning sign will be rendered in the space where your scene should be, detailing the problem.

如果您的场景超出任何[场景限制]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-06-scene-limitations %})，例如，如果其中包含太多三角形，场景不会被渲染。如果发生这种情况，将在场景所在的空间中显示警告标志。

If an entity is located or extends beyond the limits of the scene, it will flash with red color to indicate this. Nothing in your scene can extend beyond the scene limits. This won't stop the scene from being rendered locally, but it will stop it from being deployed to Decentraland.

如果实体位于或超出场景的限制，实体将红色闪烁以指示此种情况。场景中的任何内容都不能超出场景限制。场景在本地还可以渲染，但部署到 Decentraland 时会被阻止。

## View collision meshes
## 查看 collision 碰撞网格

While viewing the preview, you can press `c` to view any collision meshes loaded in the glTF models of the scene. These are usually invisible, but determine where an avatar can move through, and where it can't.

在查看预览时，您可以按 `c` 查看场景的 glTF 模型中加载的碰撞网格。这些通常是不可见的，用来确定玩家哪些地方可以通过，哪些不能通过。

![](/images/media/collision-meshes.png)

Collision meshes can be added to any model in an external 3D modeling tool like Blender. Large models like houses often include these, they are usually a lot simpler geometrically than the original shape, as this implies much less computational requirements. Stairs typically use a simplified collision mesh like a ramp to make it easier to climb, otherwise a character would have to jump up every step.

碰撞网格可以添加到外部 3D 建模工具（如 Blender）中的任何模型中。像房屋这样的大型模型通常包括碰撞网格，它们通常比原始形状更为简单，因为这样可以减少计算要求。楼梯通常使用简化的碰撞网格，如斜坡，便于攀爬，要不玩家必须每一步都跳起来才能走上去。

## View bounding boxes
## 查看边界框

While viewing the preview, you can press `b` to view any bounding boxes. Bounding boxes show the space occupied by an entity, it's especially useful to see these when dealing with invisible entities.

在查看预览时，您可以按 `b` 来查看实体的边界框。边界框显示实体占用的空间，在处理不可见实体时查看这些空间特别有用。

## Using the Ethereum test network
## 使用以太坊测试网络

If your scene makes use of transactions over the Ethereum network, for example if it prompts you to pay a sum in MANA to open a door, you can avoid using real currency while previewing the scene.

如果您的场景使用了以太坊网络上的交易，例如，提示需要支付一笔 MANA 才能打开一扇门，你可以在预览场景时避免使用真实货币。

For this, you must use the _Ethereum Ropsten test network_ and transfer fake MANA instead. To use the test network you must set your Metamask Chrome extension to use the _Ropsten test network_ instead of _Main network_. You must also own MANA in the Ropsten blockchain, which you can aquire for free from Decentraland.

为此，您必须使用_以太 Ropsten 测试网络_来发送伪 MANA。 要使用测试网络，您必须将 Metamask Chrome 扩展程序设置为使用 _Ropsten test network_ 而不是 _Main network_。 并且在 Ropsten 区块链中拥有 MANA，你可以从Decentraland 免费获得。

To preview your scene using the test network, add the `DEBUG` property to the URL you're using to access the scene preview on your browser. For example, if you're accessing the scene via `http://127.0.0.1:8000/?position=0%2C-1`, you should set the URL to `http://127.0.0.1:8000/?DEBUG&position=0%2C-1`.

要使用测试网络预览场景，请将 `DEBUG` 属性添加到浏览器上访问场景预览的 URL 上。 如，如果您通过 `http://127.0.0.1:8000/?position=0%2C-1` 访问场景，则应将 URL 设置为 `http://127.0.0.1:8000/?DEBUG&position=0%2C-1`。

Any transactions that you accept while viewing the scene in this mode will only occur in the test network and not affect the MANA balance in your real wallet.

在此模式下，查看场景时进行的任何交易都只会出现在测试网络中，而不会影响真实钱包中的 MANA 余额。