---
date: 2018-01-01
title: 创建 DApp 应用
description: 创建自己的去中心化应用程序参考
categories:
  - blockchain-integration
type: Document
redirect_from:
  - /blockchain-interactions/create-a-dapp
set: blockchain-integration
set_order: 20
---

您可以创建自己的去中心化应用程序（dApps），来与 Decentraland 智能合约交互，从而使得功能实现更为精巧并更加用户友好。

## 什么是 dApp

去中心化应用程序或 dApp 是在分布式对等网络上运行而不是从中央服务器运行的应用程序。

在区块链界，dApp 使用智能合约，可能还有 P2P 网络，而不是 Web API 服务。 dApp 还可以有前端界面和临时缓存来自区块链的信息，但其输出最终反映在链上。

有关 dApp 的更完整概述，请参阅[此站点](https://blockchainhub.net/decentralized-applications-dapps/)。

## Decentraland 智能合约

Decentraland 编写并维护了许多与 LAND 和 MANA 通证互动的智能合约。

LAND 和 MANA 通证本身分别由 _LANDregistry_ 和 _MANAtoken_ 合约定义。清单中还包括其它更具体的合约，如由几个地块创建连块土地或抵押土地。

- [Decentraland 智能合约](https://contracts.decentraland.org/addresses.json):
  您可以找到我们每个合约及其地址的完整列表。

注意，每个合约都在 _主网_ 中有一个生产版本，在 _ropsten_ 中有一个测试版本，每个都有不同的地址。

您可以阅读每个合约的完整代码，因为它是区块链的公共信息。例如，您可以在[Etherscan](https://etherscan.io/contractsVerified) 上按名称查找合约以阅读其内容。

## dApp 样板代码

 - [Boilerplate 代码](https://github.com/decentraland/dapp-boilerplate)：这个 Boilerplate 代码可以成为构建自己的dApp 的一个很好的开始。


## 辅助库

在构建我们自己的 dApp 时，这里有一些您可能觉得有用的辅助库：

- [Decentraland-eth](https://github.com/decentraland/decentraland-eth)：这是一个低级库，具有使用太坊区块链的一些实用函数。

- [Decentraland-dapps](https://github.com/decentraland/decentraland-dapps)：这是一个更高级别的库，具有 dApp 开发需要的一些通用模块。该库中的模块使用 `Decentraland-eth` 构建。

- [Decentraland UI](https://ui.decentraland.org/)：此库包含 Decentraland 项目中包含的一系列可重用 UI 元素。

## dApps 示例

下面是我们围绕 Decentraland 构建的几个 dApp 的完整代码的链接，可能有助于您构建自己的应用：

- [Canilla](https://github.com/decentraland/canilla)：这个基本的 dApp 提供免费的 Ropsten MANA。

- [Gate](https://github.com/decentraland/gate)：这个基本的 dApp 使用 Decentraland 私有 alpha 版本为白名单用户创建邀请 NFT。

- [Marketplace](https://github.com/decentraland/marketplace)：这是运行 Decentraland [虚拟市场](https://market.decentraland.org/) 的完整应用程序。为了运行速度，它需要一个数据库和连接到以太坊网络的后端服务器，来创建包含有关 LAND 信息的索引。

## dApp 测试框架

在将 dApp 投入使用之前，我们建议先进行测试。

- [dAppeteer](https://github.com/decentraland/dappeteer)：我们将这个框架放在一起，以帮助您在 dApp 上运行测试。
