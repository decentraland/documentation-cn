---
date: 2018-01-06
title: 发布场景
description: 如何发布我的项目?
redirect_from:
  - /documentation/publishing/
categories:
  - deploy
type: Document
set: deploy
set_order: 7
---

## 预备工作

确保以下内容：

- 您的场景符合所有[场景限制](({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %}))。 每次运行场景预览时，大多数限制都会得到验证。

- 您有一个 [Metamask](https://metamask.io/) 帐户，并为其分配了 LAND 地块。该帐户还必须持有最低金额以支付交易费。

- 您拥有必要数量的相邻 LAND 地块。 否则，您可以在[虚拟市场](({{ site.baseurl }}{% post_url /blockchain-interactions/2018-01-01-marketplace %}))中购买 LAND。

<!--
- 如果要将单个场景部署到多个相邻地块，则必须先将它们合并到 _连块土地_ 中，然后才能部署到它们。有关如何创建连块土地的说明，请参阅[虚拟市场]({{ site.baseurl }}{% post_url /blockchain-interactions/2018-01-01-marketplace %})。

-->

## 检查场景数据

部署时，CLI 会从 _scene.json_ 读取场景部署位置等信息。

打开场景的 _scene.json_ 文件并验证以下内容：

- **Owner**：需要与您的以太坊钱包地址相匹配。这个地址要求有 LAND 通证，或者已被所有者授予上传场景的权限。

- **Parcels**：场景占用的地块坐标

- **Base**：场景中视为[0,0]坐标的土地坐标。
<!--
- **Estate**：部署连块土地的 ID。如果要部署到单块土地，则不需要此字段。

  > 注意：要查看连块土地 ID，请在市场中打开该连块土地的详细信息页面。在 URL 中有包含 ID 编号。 例如，如果 URL 是 _market.decentraland.org/estates/84/detail_，则该连块土地的 ID 为 _84_。
-->
## 发布您的场景：

1. 确保最近更改的场景已经在本地完成构建。如果还没有，请运行 `npm run build` 。
2. 使用跟您的 Decentraland 土地相关联的地址来登录 Metamask 帐户。
3. 在场景的文件夹中运行 `dcl deploy`。
4.  The command line lists the files it will upload. Confirm with _Y_.

    > Tip: If there are files in your project folder that you don't want to deploy, list them in the _.dclignore_ file.
    If you only want to deploy the files that have changed since your last deploy, add the flag `--p` to the deploy command.

5.  A browser tab will open, showing what parcels you're deploying to. Click **Sign and Deploy**.
6.  Metamask opens, notifying you that your signature is requested. Click **Sign** to confirm this action.

<!--
Currently, as a measure to improve performance and your visitor's experience, your content will be pinned to Decentraland’s main server to ensure that the data needed to render your parcel is always readily available.
-->

> Note: Although this command deploys your scene to your parcels, remember that users can’t currently explore Decentraland, so your content won’t be discoverable “in-world” yet.

> Tip: If you're implementing a continuous integration flow, where changes to your scene are deployed automatically, then you can use the `--y` flag to skip the manual confirmations when running the deploy command.

## 使用 Ledger 硬件钱包发布

将 LAND 存储在插入计算机的 [Ledger](https://www.ledger.com/) 硬件钱包中比存储在 Metamask 帐户更安全。

如果您使用了其中的一种钱包，则上传的过程略有不同。

1. 要确保在修改场景后已经在本地构建过场景，请运行`npm run build`。
2. 将您的 Ledger 设备插入。您在 Decentraland 的地块应该与这个钱包相关联。
3. 从场景的文件夹中运行`dcl deploy --https`。
4. The command line lists the files it will upload. Confirm with _Y_.

    > Tip: If there are files in your project folder that you don't want to deploy, list them in the _.dclignore_ file.

5.  A browser tab will open, showing what parcels you're deploying to. Click **Sign and Deploy**.

    > Note: Currently, the certificate is self-signed, so your browser might give you a warning before launching the page. The warning is displayed only because the certificate is self-signed by your machine, please ignore it and carry on.

6.  The Ledger device will then ask you for a confirmation, which you must give by pushing the device's buttons.

> Tip: If you're implementing a continuous integration flow, where changes to your scene are deployed automatically, then you can use the `--y` flag to skip the manual confirmations when running the deploy command.

## What is the content server

The content server has a filesystem that's content-addressed, meaning that each file is identified by its contents, not an arbitrary file name.

We use the content server to host and distribute all scene content in a similar way to BitTorrent, keeping the Decentraland network distributed.

1.  The content server stores and distributes all of the assets required to render your scenes.
2.  The `dcl deploy` command links these assets to the LAND parcel specified in your **scene.json** file. Whenever you redeploy your scene, the CLI will update your LAND smart contract, if needed, to point to the most recent content available on the content server.

Anyone willing to host a copy of the content server will be free to replicate it. The information on each copy of the server will be verifiable, as each scene is signed by the LAND owner's hash. This means that someone hosting a copy of the server won't be able to tamper with the content to display something illegitimate.
