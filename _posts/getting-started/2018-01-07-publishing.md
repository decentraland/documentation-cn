---
date: 2018-01-06
title: 发布场景
description: 如何发布我的项目?
redirect_from:
  - /documentation/publishing/
categories:
  - getting-started
type: Document
set: getting-started
set_order: 7
---

## 预备工作

确保以下内容：

- 场景的 `scene.json` 文件设置了正确的属性，包括 Metamask 地址，以及场景要上传的 LAND 地块。

> 注意：CLI 会在创建场景时提示您提供此信息，但您也可以随时手动修改该文件。

- 您的场景符合所有[场景限制](({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %}))。 每次运行场景预览时，大多数限制都会得到验证。

* 您已正确安装了 IPFS。 为此，请按照[这些步骤](https://ipfs.io/docs/install/)进行操作。

* 您有一个 [Metamask](https://metamask.io/) 帐户，并为其分配了 LAND 地块。该帐户还必须持有最低金额以支付交易费。

* 您拥有必要数量的相邻 LAND 地块。 否则，您可以在[虚拟市场](({{ site.baseurl }}{% post_url /marketplace/2018-01-01-marketplace %}))中购买 LAND。

## 发布您的场景：

1. 确保最近更改的场景已经在本地完成构建。如果还没有，请运行 `npm run build` 。
2. 使用跟您的 Decentraland 土地相关联的地址来登录 Metamask 帐户。地址必需配置在文件 _scene.json_ 中。
3. 按照[这里的说明](https://ipfs.io/docs/getting-started/)启动 IPFS 守护程序。
4. 最后，在场景的文件夹中运行 `dcl deploy`。

如果这是您第一次将此场景上传到选定的地块，在文件上传完成后 Metamask 将要求您批准支付矿工费的交易。您只有在首次部署内容时才需付款，因为只有在将内容链接到 IPNS（IPFS 的命名服务）时，才会更新 LAND 的智能合约。

`dcl deploy` 除了将您的内容上传到 IPFS 之外，还会更新您地块内容。

目前，作为提高性能和访问者体验的措施，您的内容将在 Decentraland 的主要 IPFS 服务器上保存，以确保渲染您的地块所需的数据始终可用。

> 注意：虽然此命令将您的场景部署到了您的地块，要注意的是目前用户还无法使用 Decentraland 浏览探索，因此您的内容还无法在虚拟世界里展示。

## Publish from a physical Ledger device

Instead of storing your LAND tokens in a Metamask account, you may find it more secure to store them in a [Ledger](https://www.ledger.com/) device that's phyisically plugged in to your computer.

If you're using one of these, the process of uploading content to your LAND is slightly different.

1.  To make sure the scene has been locally built with your latest changes, run `npm run build`.
2.  Plug your Ledger device in. Your parcels in Decentraland should be associated with that same wallet. The same public address should be listed in the scene's _scene.json_ file.
3.  Start up an IPFS daemon by following [these instructions](https://ipfs.io/docs/getting-started/).
4.  Run `dcl deploy --https` from the scene's folder. This will open a tab on your browser where you need to confirm this action.
    > Note: Currently, the certificate is self-signed, so your browser might give you a warning before launching the page. The warning is displayed only because the certificate is self-signed by your machine, please ignore it and carry on.
5.  The Ledger device will then ask you for a confirmation, which you must give by pushing the device's buttons.

## 使用 Ledger 硬件钱包发布

将 LAND 存储在插入计算机的 [Ledger](https://www.ledger.com/) 硬件钱包中比存储在 Metamask 帐户更安​​全。

如果您使用了其中的一种钱包，则上传的过程略有不同。

1. 要确保在修改场景后已经在本地构建过场景，请运行`npm run build`。
2. 将您的 Ledger 设备插入。您在 Decentraland 的地块应该与这个钱包相关联。并且与场景的 _scene.json_ 文件中配置的一样。
3. 按照[说明](https://ipfs.io/docs/getting-started/)启动IPFS守护程序。
4. 从场景的文件夹中运行`dcl deploy --https`。这将在您的浏览器上打开一个选项卡，您需要确认此操作。
   > 注意：目前，证书是自签名的，因此您的浏览器可能会在启动页面之前向您发出警告。仅显示警告，因为证书是由您的机器自行签名的，请忽略它并继续。
5. 然后，Ledger 设备将要求您进行确认，您必须通过按下设备的按钮进行确认。

## 什么是IPFS？

[IPFS](https://ipfs.io/)(星际文件系统的简称)是一个超媒体协议和一个用于分发文件的 P2P网络。文件系统是由内容来寻址的，这意味着文件是由其内容，而不是由随意的文件名称来识别。

我们使用 IPFS 以类似于 BitTorrent 的方式托管和分发所有场景内容，从而保持Decentraland 网络的分散。为了获得更好的性能，我们运行了一个“ IPFS 网关”，这意味着Decentraland 正在托管着区块链中引用的大部分内容（在使用某些过滤之后），以改善用户探索世界的体验。

要上传文件，您需要运行 IPFS 节点。在 “pinning”([使得 IPFS 保留本地备份](https://ipfs.io/ipfs/QmNZiPk974vDsPmQii3YbrMKfi12KTSNM7XMiYyiea4VYZ/example#/ipfs/QmRFTtbyEp3UaT67ByYW299Suw7HKKnWK6NJMdNFzDjYdX/pinning/readme.md)) 场景的内容（这意味着通知网络您的文件可用）之后，我们的 IPFS 节点将尝试从 IPFS 网络下载文件，最后到达您的计算机并复制文件。

要运行 IPFS 节点，请按照[这些说明](https://ipfs.io/docs/getting-started/)进行操作。

### IPFS 与我的 LAND 有什么关系？

IPFS 为 Decentraland 提供了两个主要功能。

1. 渲染场景所需的所有资源由 IPFS 存储和分发。

2. `dcl deploy` 命令将这些资源链接到 **scene.json** 文件中指定的 LAND 地块。每当您重新部署场景时，CLI 将根据需要更新您的 LAND 智能合约，使之指向 IPFS 上可用的最新内容。
