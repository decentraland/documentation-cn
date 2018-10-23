---
date: 2018-01-01
title: 创建 dApp 应用
description: 创建自己的去中心化应用程序参考
categories:
  - blockchain-interactions
type: Document
set: blockchain-interactions
set_order: 9
---

You can create your own decentralized apps (dApps) to interface with Decentraland's smart contracts and expose their functionality in more elaborate and friendlier ways.

您可以创建自己的去中心化应用程序（dApps），来与 Decentraland 智能合约交互，从而使得功能实现更为精巧并更加用户友好。

## What is a dApp

A decentralized application, or dApp, is one that runs on a distributed peer to peer network rather than from a central server.

In the context of blockchain, a dApp uses smart contracts and possibly a P2P network, instead of a Web API service. A dApp may also expose a front end and cache information from the blockchain temporarily, but its output is ultimately reflected on-chain.

See [this site](https://blockchainhub.net/decentralized-applications-dapps/) for a more complete overview about dApps.

## 什么是 dApp

去中心化应用程序或 dApp 是在分布式对等网络上运行而不是从中央服务器运行的应用程序。

在区块链界，dApp 使用智能合约，可能还有 P2P 网络，而不是 Web API 服务。 dApp 还可以有前端界面和临时缓存来自区块链的信息，但其输出最终反映在链上。

有关 dApp 的更完整概述，请参阅[此站点](https://blockchainhub.net/decentralized-applications-dapps/)。

## Decentraland smart contracts

Decentraland has written and maintains a number of smart contracts that interact with LAND and MANA tokens.

LAND and MANA tokens themselves are defined by the _LANDregistry_ and _MANAtoken_ contracts respectively. The list also includes more specific contracts like creating an estate out of several parcels or mortgaging parcels.

## Decentraland 智能合约

Decentraland 编写并维护了许多与 LAND 和 MANA 通证互动的智能合约。

LAND 和 MANA 通证本身分别由 _LANDregistry_ 和 _MANAtoken_ 合约定义。清单中还包括其它更具体的合约，如由几个地块创建连块土地或抵押土地。

- [Decentraland smart contracts](https://contracts.decentraland.org/addresses.json):
  You can find a full list of each of our contracts and their addresses.

Note that each contract has a production version in _mainnet_ and a test version in _ropsten_ and that each has a different address.

You can read the full code of each contract, as it's public information on the blockchain. For example, you can find the contract by name on [Etherscan](https://etherscan.io/contractsVerified) to read its contents.

- [Decentraland 智能合约](https://contracts.decentraland.org/addresses.json):
  您可以找到我们每个合约及其地址的完整列表。

注意，每个合约都在 _主网_ 中有一个生产版本，在 _ropsten_ 中有一个测试版本，每个都有不同的地址。

您可以阅读每个合约的完整代码，因为它是区块链的公共信息。例如，您可以在[Etherscan](https://etherscan.io/contractsVerified) 上按名称查找合约以阅读其内容。

## dApp boilerplate code

- [Boilerplate code](https://github.com/decentraland/dapp-boilerplate): This Boilerplate code can be a great starting point for building your own dApp.

## dApp 样板代码

 - [Boilerplate 代码](https://github.com/decentraland/dapp-boilerplate)：这个 Boilerplate 代码可以成为构建自己的dApp 的一个很好的开始。

## Helper libraries

While building our own dApps internally, we put together some helper libraries that you might also find useful.

- [Decentraland-eth](https://github.com/decentraland/decentraland-eth): This is a low level library with utility functions to work with the Ethereum blockchain.

- [Decentraland-dapps](https://github.com/decentraland/decentraland-dapps): This is a higher level library with common modules for dApps. The modules in this library are built using `Decentraland-eth`.

- [Decentraland UI](https://ui.decentraland.org/): This library contains a selection of reusable UI elements that are included in Decentraland's projects.

## 辅助库

在构建我们自己的 dApp 时，这里有一些您可能觉得有用的辅助库：

- [Decentraland-eth](https://github.com/decentraland/decentraland-eth)：这是一个低级库，具有使用太坊区块链的一些实用函数。

- [Decentraland-dapps](https://github.com/decentraland/decentraland-dapps)：这是一个更高级别的库，具有 dApp 开发需要的一些通用模块。该库中的模块使用 `Decentraland-eth` 构建。

- [Decentraland UI](https://ui.decentraland.org/)：此库包含 Decentraland 项目中包含的一系列可重用 UI 元素。

## Sample dApps

Below are links to the full code of several dApps that we built around Decentraland, these might help you build your own:

- [Canilla](https://github.com/decentraland/canilla): This basic dApp provides free Ropsten MANA.

- [Gate](https://github.com/decentraland/gate): This basic dApp creates an invitation NFT to whitelist users using the Decentraland private alpha version.

- [Marketplace](https://github.com/decentraland/marketplace): This is the full application that runs the Decentraland [Marketplace](https://market.decentraland.org/). To make it run fast, it requires a database and a backend server connected to the Ethereum network to create indexes with information about LAND.

## dApps 示例

下面是我们围绕 Decentraland 构建的几个 dApp 的完整代码的链接，可能有助于您构建自己的应用：

- [Canilla](https://github.com/decentraland/canilla)：这个基本的 dApp 提供免费的 Ropsten MANA。

- [Gate](https://github.com/decentraland/gate)：这个基本的 dApp 使用 Decentraland 私有 alpha 版本为白名单用户创建邀请 NFT。

- [Marketplace](https://github.com/decentraland/marketplace)：这是运行 Decentraland [虚拟市场](https://market.decentraland.org/) 的完整应用程序。为了运行速度，它需要一个数据库和连接到以太坊网络的后端服务器，来创建包含有关 LAND 信息的索引。

## dApp testing framework

Before launching your dApp into production, we recommend testing it first.

- [dAppeteer](https://github.com/decentraland/dappeteer): We put this framework together to help you run tests on your dApp.


## dApp 测试框架

在将 dApp 投入使用之前，我们建议先进行测试。

- [dAppeteer](https://github.com/decentraland/dappeteer)：我们将这个框架放在一起，以帮助您在 dApp 上运行测试。
