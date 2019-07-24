---
date: 2018-01-06
title: 发布场景
description: 如何发布项目?
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

- 您的场景符合所有[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})。 每次运行场景预览时，大多数限制都会一一验证。

- 您有一个 [Metamask](https://metamask.io/) 帐户，并为其分配了 LAND 地块。该帐户还必须持有最低金额以支付交易费。

- 您拥有必要数量的相邻 LAND 地块。 否则，您可以在[虚拟市场]({{ site.baseurl }}{% post_url /blockchain-interactions/2018-01-01-marketplace %})中购买 LAND。

> 注意：多块土地的场景只能部署到相邻地块中。

<!--
- 如果要将单个场景部署到多个相邻地块，则必须先将它们合并到 _连块土地_ 中，然后才能部署到它们。有关如何创建连块土地的说明，请参阅[虚拟市场]({{ site.baseurl }}{% post_url /blockchain-interactions/2018-01-01-marketplace %})。

-->

## 检查场景数据

部署时，CLI 会从 _scene.json_ 读取场景部署位置等信息。

打开场景的 _scene.json_ 文件并验证以下内容：

- **Owner**：需要与您的以太坊钱包地址相匹配。这个地址要求有 LAND 通证，或者已被所有者授予上传场景的权限。

- **Parcels**：场景占用的地块坐标

- **Base**：场景中视为[0,0]坐标的土地坐标。

> 注意：有关如何设置这些参数的更多详细信息，请参阅[场景元数据]({{ site.baseurl }}{% post_url /development-guide/2018-02-26-scene-metadata %})。

## 发布场景：

1. 确保更改的场景已经在本地完成构建。如果还没有，请运行 `npm run build` 。
2. 使用跟您的 Decentraland 土地相关联的地址来登录 Metamask 帐户。
3. 在场景的文件夹中运行 `dcl deploy`。
4. 命令行列出了将要上传的文件。按 _Y_ 确认。
   > 提示: 如果项目中有不想部署的文件，可以在 _.dclignore_ 文件中列出。
   如果只想部署自上次部署以来已更改的文件，可以在 deploy 命令中添加 `--p` 标志。
5. 会自动打开浏览器，显示要部署到的地块。请单击 **Sign and Deploy** 。
6. Metamask 会打开提示，告知需要您的签名。请单击 **Sign** 确认此操作。

<!--
Currently, as a measure to improve performance and your visitor's experience, your content will be pinned to Decentraland’s main server to ensure that the data needed to render your parcel is always readily available.
-->

> 注意: 虽然这个命令将您的场景部署到您的土地中，但是用户目前还不能使用 Decentraland，所以您的内容还不能在“这个世界”显示。

> 提示: 如果使用持续集成，需要自动部署对场景的更改，那么您可以在运行 deploy 命令时使用 `--y` 标志跳过手动确认。

## 使用 Ledger 硬件钱包发布

将 LAND 存储在插入计算机的 [Ledger](https://www.ledger.com/) 硬件钱包中比存储在 Metamask 帐户更安全。

如果您使用了这些钱包，则上传的过程略有不同。

1. 确保在修改场景后已经在本地构建过场景，请运行 `npm run build`。
2. 将您的 Ledger 设备插入。您在 Decentraland 的地块应该与这个钱包相关联。
3. 从场景的文件夹中运行`dcl deploy --https`。
4. 命令行列出了将要上传的文件。按 _Y_ 确认。
   
   > 提示:如果项目中有不想部署的文件，可以在 _.dclignore_ 文件中列出。

5. 将打开浏览器，显示要部署到的地块。单击 **Sign and Deploy** 。

   > 注意:目前证书是自签名的，因此浏览器可能会在启动页面之前发出警告。警告只是因为证书是由您的机器自签名而显示的，请忽略它并继续。

6. Ledger 然后会要求你确认，通过按设备的按钮来进行确认。

> 提示: 如果使用持续集成，需要自动部署对场景的更改，那么您可以在运行 deploy 命令时使用 `--y` 标志跳过手动确认。

## 场景覆盖

当部署一个新场景时，它会覆盖它所使用的土地上存在的旧内容。

如果一个场景使用了多块土地，而另一个场景只覆盖了其中的一部分，那么剩下的土地就无法渲染。场景只能全部渲染。

假设您将场景 _A_ 部署在两块土地上 _[100,100]_ 和 _[100,101]_。然后您将地块 _[100,101]_ 出售给拥有相邻土地的用户，该用户将一个大场景(_B_)部署到多个地块，其中包括 _[100,101]_。

您的场景 _A_ 不能在一块地中部分渲染，因此 _[100,100]_ 不会显示任何内容。您必须构建一个新版本的只使用一块土地的场景 _A_，并将其部署到土地 _[100,100]_。

## 什么是内容服务器

内容服务器使用内容寻址的文件系统，这意味着文件都由它的内容来标识，而不是通过其文件名。

内容服务器以类似于 BitTorrent 的方式托管和分发所有场景内容，保持 Decentraland 网络的分布。

1. 内容服务器存储和分发显示场景所需的所有文件。
2. `dcl deploy` 命令将这些文件链接到 **scene.json** 文件指定的土地中。每当您重新部署场景时，如果需要，CLI 会更新您的 LAND 智能合约，使之指向内容服务器上的最新内容。

任何愿意托管内容服务器副本的人都可以自由地复制这些内容。每个服务器副本上的信息都是可验证的，因为每个场景都经过土地所有者的签名。意味着托管服务器副本的人也无法篡改内容以显示不合法的内容。