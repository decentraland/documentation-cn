---
date: 2018-01-01
title: 上传预览
description: 将场景预览上传到服务器并在链下分享。
categories:
  - deploy
type: Document
set: deploy
set_order: 7
---

Decentraland 场景的预览可以独立运行，就像可以在机器本地运行预览一样。 这是将您的内容与他人分享和互动预览的绝佳方式！ 同时对在其他平台（如移动设备）中测试场景也很有用。

Decentraland 场景预览与以下托管平台兼容：

- [Zeit Now](https://zeit.co/now) _(free)_
- [GitHub pages](https://pages.github.com/) _(free)_
- [Amazon S3](https://aws.amazon.com/s3/)

一旦上传到其中一个平台，其他人唯一要做的事就是打开链接。 不需要安装 CLI，Node，NPM 或在本地计算机上运行预览所需的任何其他工具。

例如，以下是当前在 Zeit Now 中运行的一些场景：

- [Block Dog](https://blockdog-wtciaozdbo.now.sh)
- [Humming birds](https://hummingbirds-ujovmbtmui.now.sh)

请注意，将场景预览上载到服务器并不需要拥有 LAND。 上传的内容不会以任何方式链接到区块链。 运行预览时，其他相邻领地显示为空白。

## 准备你的场景

要将预览上载到服务器，请确保使用的是最新版本的 SDK。如果您不确定，请查看**发行说明**部分。

切记，SDK 版本是在场景中指定的，在您首次使用 CLI 创建时确定。因此，如果您使用较旧版本的 CLI 创建了场景，然后更新了 CLI，则需要手动更新场景，让它使用最新版本的 SDK。

场景必须编译，因为上传的是 `/bin` 文件夹中的文件而不是 `/src` 文件夹中的文件。

如果没有使用最新的编译器编译场景，可以运行 `npm install` 来完成编译。 同时每次使用 `dcl start` 运行预览时，场景也会自动编译。

> 注意：如果要部署的场景是使用较旧版本的 CLI（2.2.6 或更早版本）构建的，请更新项目中的 `dclignore` 文件以包含以下条目：

    - node_modules
    - dist
    - export


## 场景部署准备

要将场景部署到 Zeit Now，GitHub 页面或Am azon S3 等托管服务，必须先将场景代码导出为静态网页的格式。 为此，请运行以下命令：

```
dcl export
```

这将在项目目录中创建一个名为 `export` 的新文件夹。 此文件夹包含以大多数托管服务兼容的格式运行场景所需的所有内容。

仅将 `export` 文件夹的内容上传到托管服务器。

> 注意：`/export` 文件夹是根据 `/bin` 文件夹的内容构建的。 在运行 `dcl export` 命令之前，请确保已编译场景源代码的最新版本。 您可以通过运行`npm install` 或 `dcl start` 来编译场景。

## 部署到 Zeit Now

要将场景部署到 Now：

1. 确保您安装了最新版本的 _Zeit Now CLI_ 和 _Decentraland CLI_。

    ```
    npm i -g decentraland now
    ```

2. 从场景的 `export` 文件夹（使用 `dcl export` 命令创建）运行以下命令：

   ```
   now
   ```

   控制台将显示服务器的输出，因为它会安装必要的依赖项来运行场景。这需要几分钟。

2. 完成后，服务器的 URL 会自动添加到剪贴板中，然后粘贴到浏览器中！ 

   也可以在控制台的输出中获得链接，类似于 `https://myscene-gnezxvoayw.now.sh`.

   > 提示：URL 将把文件夹名称作为路径的一部分。 您可以将 `export` 文件夹重命名。 这不会影响场景，但会改变 URL。

3. 可以把这个链接分享给任何人！ _Now_ 会继续在该链接上托管您的场景。

> 注意: 切记，Now 的免费版本限制文件最大为 50 MB。

可选的，可以使用 now.sh 的域名，或者在 Now 注册的其它域名。 以下示例使用 decentraland.now.sh域：

```
now alias {deploymentId} decentraland.now.sh
```

> 注意：对于使用 6.0 或更早版本构建的场景，您必须对项目进行一些手动调整。有关详细信息，请参阅[此博客文章](https://decentraland.org/blog/announcements/decentraland-on-now/)。

## 多人游戏考虑因素

请注意，部署到服务器的场景只能进行单人游戏体验，即便您能够看到其他用户在四处走动。

用户位置是共享的，但每个用户在浏览器中看到的是本地运行场景，不是同步场景。如果可以通过用户的交互来改变场景，则每个用户将看到处于不同状态的场景。

要在用户之间同步场景状态，有两种方法（参见[多人游戏注意事项]({{ site.baseurl }}{% post_url /development-guide/2018-01-10-remote-scene-considerations %}) )如果您使用远程服务器在各个用户之间同步状态，然后您还应该将服务器部署在与场景分开的某个位置。 您还可以按照以下步骤将服务器部署到`now`。

#### 上传多人游戏场景

If you created your scene based on one of the remote scene examples, then you need to make two separate deployments, one for the server and another for the scene client.

如果您基于远程场景示例创建场景，则需要进行两次单独部署，一次针对服务器，另一次针对场景客户端。

例如，要将服务器和场景客户端部署到 Zeit now，请按照下列步骤操作：

1. 更改目录为 `/server` 文件夹并运行以下命令以部署服务器：
   
   ```
   now
   ```

2) 从已部署的项目中复制 URL。

3) 在`/scene`文件夹中，修改代码，使其指向您部署到 Now 的服务器的链接（就是刚刚复制的 URL）。

   如果场景使用 websocket 连接，则必须手动修改 URL 地址，使用 _wss_ 而不是 _https_ 开头。

   例如，如果服务器的地址是 `https://dcl-project-fsutefbepd.now.sh/`，则应将其修改为 `wss://dcl-project-fsutefbepd.now.sh/`。


4) 部署完整的场景文件，运行

   ```
   now
   ```

5) 上传完成后，剪贴板中将会有场景预览 URL，可用于分享。
