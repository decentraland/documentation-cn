---
date: 2018-01-01
title: 部署到 Now
description: 上传场景到 ZEIT Now 以供链下分享。
categories:
  - deploy
type: Document
set: deploy
set_order: 7
---

Decentraland scene previews are compatible with [Now](https://zeit.co/now), a very handy service that lets you upload content to a server and run it for as long as you like, for free. You can easily upload a Decentraland scene preview to one of these servers.

This is a fantastic way to share interactive previews of your content with others! It's also useful to test your scenes in other platforms, such as mobile.

Decentraland 场景预览与 [Now](https://zeit.co/now) 兼容，[Now](https://zeit.co/now)提供了非常方便的服务，可以让您将内容上传到服务器并随意免费运行。可以轻松地将 Decentraland 场景预览上传到服务器上。

这是将您的内容与他人分享和互动预览的绝佳方式！对在其他平台（如移动设备）中测试场景也很有用。

Once uploaded to Now, the only thing that others have to do in order to explore your scene is open a link. They don’t need to install the CLI, Node, NPM, or any of the other tools that would be required to run the preview on their local machine.

For example here are some scenes that are currently running in Now:

一旦上传到 Now，其他人查看你的场景只需要打开一个链接，并不需要安装 CLI，Node，NPM 或在本地计算机上运行预览所需的任何其他工具。

下面一些当前正在运行的场景：

- [Block Dog](https://blockdog-wtciaozdbo.now.sh)
- [Humming birds](https://hummingbirds-ujovmbtmui.now.sh)

Note that it's not necessary to own LAND to upload a scene preview to Now. The uploaded content isn't linked to the blockchain in any way. When running the preview, other adjacent parcels appear as empty.

请注意，不需要拥有 LAND 就可以将场景预览上传到 Now。上传的内容不会以任何方式链接到区块链。运行预览时，其他相邻地块显示为空。

## Get your scene ready
## 准备你的场景

To run your scene in Now, make sure you’re using the latest version of the SDK. Check the **Release notes** section if you're not sure.

Remember that the SDK version is specified in the scene and is determined when you first create it with the CLI. So, if you have a scene you created with an older version of the CLI, and then update the CLI, you will need to manually update the scene so it references the latest version of the SDK.

要在 Now 中运行场景，请确保使用的是最新版本的 SDK。如果您不确定，请查看**发行说明**部分。

切记，SDK 版本是在场景中指定的，在您首次使用 CLI 创建时确定。因此，如果您使用较旧版本的 CLI 创建了场景，然后更新了 CLI，则需要手动更新场景，让它使用最新版本的 SDK。

## Deploy to Now
## 部署到 Now


要将场景部署到 Now：

To deploy a scene to now:

1. Run the following command form the scene folder:

1. 在场景目录运行以下命令：

   ```
   npm run deploy:now
   ```

   The console will show you the output of the server as it installs the necessary dependencies to run your scene. This takes a few minutes.

   控制台会显示服务器正在安装运行场景所必要的依赖项，因此需要几分钟。


2. When done, the URL to the server should automatically be added to your clipboard, ready to paste in a browser!

2. 完成后，服务器的 URL 会自动添加到剪贴板中，然后粘贴到浏览器中！

   You can otherwise get the link in the console's output, it will resemble something like `https://myscene-gnezxvoayw.now.sh`.

   也可以在控制台的输出中获得链接，类似于 `https://myscene-gnezxvoayw.now.sh`。

3. Share the link with anyone you want! _Now_ will keep hosting your scene at that link indeterminably.

3. 可以把这个链接分享给任何人！ _Now_ 会继续在该链接上托管您的场景。

Keep in mind that the free version of Now enforces a maximum file size of 50 MB.

If your scene exceeds this limit, try deleting the node_modules folder and any other files in the scene folder that don’t need to be uploaded or that can be automatically installed by the server based on what's stated in `package.json`.

切记，Now 的免费版本限制文件最大为 50 MB。

如果场景超出此限制，请尝试删除 node_modules 文件夹以及场景文件夹中不需要上载的任何其他文件，或者服务器可以根据 `package.json` 中的说明自动安装的文件。

## Multiplayer considerations
## 多人游戏考虑因素

Note that scenes deployed to _Now_ behave as single player experiences, even though you're able to see other users moving around.

请注意，部署到 _Now_ 的场景只能进行单人游戏体验，即便您能够看到其他用户在四处走动。


User positions are shared, but each user runs the scene locally in their browser. If the scene can be modified by a user's interaction, each user will see the scene in a different state.

用户位置是共享的，但每个用户在浏览器中看到的是本地运行场景，不是同步场景。如果可以通过用户的交互来改变场景，则每个用户将看到处于不同状态的场景。

To synchronize the scene's state amongst users, it's currently necessary to use a remote server to sync the state amongst the various users.

为了使用户之间能看到同步的场景状态，当前需要使用远程服务器来设置各个用户之间的同步状态。


## Upload multiplayer scenes
## 上传多人游戏场景

If you created your scene based on the remote scene examples, then you need to make two separate deployments to Now, one for the server and another for the scene client.

如果您基于远程场景示例创建场景，则需要用 Now 进行两次单独部署，一次针对服务器，另一次针对场景客户端。


1. Change directory to the `/server` folder and run the following command to deploy just the server:
1. 更改目录为 `/server` 文件夹并运行以下命令以部署服务器：

   
   ```
   npm run deploy:now
   ```

2. Copy the URL from the deployed project.
2. 从已部署的项目中复制 URL。


3) In the `/scene` folder, modify the code so that it points to the link of the server you deployed to Now (the URL you just copied).

3. 在`/scene`文件夹中，修改代码，使其指向您部署到 Now 的服务器的链接（就是刚刚复制的 URL）。

   If your scene uses a websocket connection, you must modify the URL address manually so that it begins with _wss_ (web socket secure) instead of _https_.

   如果场景使用 websocket 连接，则必须手动修改 URL 地址，使用 _wss_ 而不是 _https_ 开头。


   For example, if the server’s address is `https://dcl-project-fsutefbepd.now.sh/`, you should modify it to `wss://dcl-project-fsutefbepd.now.sh/`.

   例如，如果服务器的地址是 `https://dcl-project-fsutefbepd.now.sh/`，则应将其修改为 `wss://dcl-project-fsutefbepd.now.sh/`。


4) Deploy the full scene folder, running
4. 部署完整的场景文件，运行

   ```
   npm run deploy:now
   ```

5) Once the upload is completed, the URL to the scene preview should be in your clipboard, ready to share.
5. 上传完成后，剪贴板中将会有场景预览 URL，可用于分享。
