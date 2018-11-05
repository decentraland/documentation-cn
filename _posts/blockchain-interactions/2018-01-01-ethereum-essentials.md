---
date: 2018-01-01
title: 以太坊概要
description: Decentraland 如何使用以太坊区块链
categories:
  - blockchain-interactions
type: Document
set: blockchain-interactions
set_order: 1
---

区块链本质上是一个去中心化的数据库，它分布在网络中的各个计算机中。 所有交易按“块”进行分组，并按顺序处理形成一条_链_。

以太坊是最受欢迎的区块链之一。 它与其他（如比特币）的区别在于它使用区块链存储数据而不仅仅是保存货币交易的记录。 以太坊可以存储更复杂的信息用以区分不同类型的通证，甚至可以处理具有特定特征的独特通证。 以太坊区块链还可以运行智能合约，可以处理更复杂的基于事先商定事件的交易。

Decentraland 使用以太坊记录数字资产的所有权，包括其它可由 3D 场景读取和响应的其他可交易物品。

区块链并不存储场景状态，以及用户位置或当用户与场景交互时需要实时更改的所有内容，所有这些都存储在每个用户的本地计算机上，或者存储在场景所有者的私有服务器上。由场景的开发者决定哪些信息要存储在区块链中，哪些需要存储在私有服务器中。

## 钱包

需要使用钱包来处理以太坊通证的交易。以太坊钱包可以使用各种通证，包括以太、MANA、LAND 或其他可能在 Decentraland 游戏或体验中使用的通证。

有许多钱包提供商现在可以处理 Decentraland 通证。但是在虚拟市场交易或进入 Decentraland，则需要集成到 Web 浏览器的钱包，因此我们建议您使用：

- [Metamask](https://metamask.io/)
- [Trezor](https://trezor.io/)/[Ledger](https://www.ledger.com/) 硬件钱包

每个钱包都具有公钥和私钥。公钥的哈希值就是钱包的唯一地址，用于处理交易和识别用户。钱包使用您的私钥对您发送到网络的每笔交易进行签名，来证明它是由您真正发送的。为了防止您忘记密码，私钥还可用于恢复钱包，因此请将私钥保存在安全的地方，并不要给其它任何人。

在 Decentraland 中，由于钱包公钥是唯一的，用户身份可以基于钱包。因此场景一直可以用它来识别 Decentraland 用户。钱包中还可以有为用户提供独特的头像、可穿戴物品、进入限制访问的场景的权限、在游戏中使用的特殊武器等通证。

## 交易

交易会更改存储在区块链中的信息。 典型的交易涉及更改通证的所有者，例如用户 A 将他的 LAND 通证给用户 B ，并换取一定数量的 MANA 通证。然而，在以太坊网络中，交易还可以意味着在不改变其所有者的情况下改变存储的关于通证的信息。 例如，更改领地的描述，或将多个地块合并到一个连块土地中。

以太坊主链中发生的所有交易都需要用以太通证支付的费用。此费用称为 “gas” 费用，用以支付给网络上打包交易的矿工。

当进行交易时，可以设置为打包交易您愿意支付的 gas 费用。费用越高交易打包的速度越快，因为矿工会优先考虑这些交易。打包交易的市场价格定期动荡，当网络使用率较高时，价格便更高。 请确保您支付的价格不低于市场价格，否则您的交易可能会无限期地保留在未经处理的交易池中。

所有交易必须使用以太地址对应的私钥签名，以证明交易是由该地址发出的。

#### 验证交易

Blockchain transactions aren’t immediate, they require time to be “mined” by one of the nodes in the network, and then to be propagated throughout the rest of the machines. The more transactions that are being requested by the network, the more time they take to be validated.

In brief terms, this is how a transaction is validated:

区块链交易并不是即时的，需要时间由网络中的节点们“打包”，然后传播到其余的机器中。 网络请求的交易越多，验证交易所需的时间就越长。

简单而言，验证交易的方式如下：

1. 所有新的交易，首先会进入一个未验证交易的交易池。
2. 当网络中有计算机成功解决了算法问题，会生成一个新“块”，并把池中的一小部分交易放入块内，然后把这个新块附加到链的末尾。
3. 该块会与网络的其他机器共享。每台机器会验证块中的每个交易是否有效，并检查块的哈希以确保它是合法的，然后将其添加到其自己的链中。
4. 新块会在整个网络中传播。这使得这项交易达成共识。

#### Decentraland 的侧链

Decentraland is working on creating a _sidechain_ (a special kind of blockchain) that will be able to handle transactions faster and cheaper than the main Ethereum network. This sidechain will be ideal for in-game transactions, as changes can occur closer to real time and at a very low cost. For transactions that involve valuable items, we’ll still recommend the main Ethereum chain, as it will be more secure.

Each developer working on a scene will be able to choose whether to use the mainchain, the sidechain or a combination of both for different transactions.

The sidechain will be kept interoperable with the Ethereum’s mainchain. You’ll be able to load tokens from the main chain into the side chain and vice versa. Transactions that take place in the sidechain are eventually reflected in the mainchain when the tokens “exit” back into the mainchain.

Decentraland 正致力于创建一种 _侧链_（一种特殊的区块链），它能够比主以太坊网络更快、更便宜地处理交易。这种侧链将是游戏内交易的理想选择，它更接近实时且成本非常低。对于涉及贵重物品的交易，我们仍然会推荐使用主以太坊链，因为它更安全。

每个场景开发人员可以决定是否将主链、侧链或两者的组合用于不同的交易。

侧链会与以太坊的主链保持互操作。您将能够将主链中的通证加载到侧链中，反之亦然。当通证“退出”回到主链时，发生在侧链中的交易最终会反映在主链中。

#### 从场景中触发交易

Your scene’s code can trigger transactions, both on the Ethereum mainchain and on Decentraland’s sidechain. You could have a store in your scene that sells tokens (like NFTs), or have a game that rewards game items to players that achieve certain goals.
The user must always approve these transactions explicitly on their Ethereum client. For example, when using Metamask, Metamask prompts the user to accept each transaction before it’s processed.
Read [game design doc] for more ideas about how to integrate a scene to the blockchain. See [blockchain operations] for instructions on how to implement these integrations.

您的场景代码可以触发交易，不管是以太坊主链还是 Decentraland 的侧链。 你可以在你的场景中设计一个商店来出售通证（如NFT），或者开发一个游戏来奖励游戏物品给达到某个目标的玩家。
用户必须始终在其以太坊客户端上明确批准这些交易。 例如，使用 Metamask 时，Metamask 会在处理这个交易这前提示用户确认。
阅读[游戏设计文档]，了解有关如何将场景集成到区块链的更多想法。 有关如何实现这些集成的说明，请参见[区块链操作]。

## 通证类型

Different types of tokens can be handled in the Ethereum network. A few standards have emerged that group tokens that share the same characteristics.

In Decentraland, you can use tokens to represent items that relate to your game or experience, such as a weapon or a trophy. As tokens are held in a user’s wallet, they accompany a user from scene to scene, so each scene can choose if and how they want to react to every existing kind of token.

可以在以太坊网络中处理不同类型的令牌。 出现了一些标准，即具有相同特征的组标记。

在Decentraland中，您可以使用代币来表示与您的游戏或体验相关的项目，例如武器或奖杯。 由于令牌被保存在用户的钱包中，它们在场景之间陪伴用户，因此每个场景可以选择他们是否以及如何对每种现有的令牌作出反应。

Read [What are NFTs](https://decentraland.org/blog/technology/what-are-nfts) on our blog for a more in-depth look at the emergence and evolution of non-fungible tokens.

请阅读我们博客上的[什么是NFT]（https://decentraland.org/blog/technology/what-are-nfts），以便更深入地了解不可替代的令牌的出现和发展。

#### Fungible tokens 虚拟代币

If an item is fungible, then it can be substituted or exchanged for any similar item. Fiat currencies, like the US dollar, are fungible. One dollar bill can be exchanged for any other dollar bill.

Cryptocurrency tokens like Bitcoin, Ethereum, and MANA are all fungible because one token unit can be exchanged for any other token unit.

如果物品是可替换的，那么它可以替换或交换任何类似的物品。 菲亚特货币，如美元，是可以替代的。 一美元的账单可以换成任何其他美元账单。

像比特币，以太坊和MANA这样的加密货币令牌都是可互换的，因为一个令牌单元可以换成任何其他令牌单元。

You could also create custom fungible tokens to use in Decentraland scenes and use them to depict items that are all equal and have no distinctive or customizable properties between them. You could, for example, create a game that revolves around collecting a large quantity of identical items, and represent these through a fungible token . You could also use a fungible token to represent a golden ticket that gives users who hold it access to a specific region or service.

_ERC20_ is the most accepted standard for non-fungible tokens in the Ethereum Network. MANA is built upon this standard.

您还可以创建自定义可替换标记以在Decentraland场景中使用，并使用它们来描绘完全相同且在它们之间没有独特或可自定义属性的项目。 例如，您可以创建一个围绕收集大量相同项目的游戏，并通过可互换的令牌表示这些游戏。 您还可以使用可互换令牌来表示黄金票证，该票证可以让持有它的用户访问特定区域或服务。

_ERC20_是以太坊网络中不可替代的令牌最常被接受的标准。 MANA建立在这个标准之上。

#### Non-Fungible tokens 非虚假令牌

Non-fungible tokens (or NFTs) have characteristics that make each unit objectively different from others. Parcels of LAND in Decentraland are NFTs, as the location of each parcel is unique. The adjacency to other parcels, roads, or districts make these locations relevant to token owners.

In Decentraland, you can use NFTs to represent in-game items such as avatars, wearables, weapons, and other inventory items. You could, for example, use a single type of NFT to represent all weapons in your game, and differentiate them by setting different properties in these NFT.

不可替代的令牌（或NFT）具有使每个单元客观上与其他单元不同的特征。 Decentraland的LAND地块是NFT，因为每个地块的位置都是独一无二的。 与其他地块，道路或地区的邻接使这些地点与令牌所有者相关。

在Decentraland中，您可以使用NFT来表示游戏中的项目，例如头像，可穿戴设备，武器和其他库存物品。 例如，您可以使用单一类型的NFT来表示游戏中的所有武器，并通过在这些NFT中设置不同的属性来区分它们。

NFTs can be used to provide provably scarce digital goods. Because of the legitimate scarcity made possible by the blockchain, buyers can rest assured that the art they purchase is, in fact, rare. This gives digital art real value that we’ve never seen before.

Game items will have a history that’s stored in the blockchain. This history could deem an item more valuable, for example if it was used to accomplish great achievements or used by someone who’s admired.

NFT可用于提供可证明稀缺的数字商品。 由于区块链可以实现合法的稀缺性，买家可以放心，他们购买的艺术品实际上很少见。 这给了我们以前从未见过的数字艺术真正的价值。

游戏物品将具有存储在区块链中的历史记录。 这段历史可以认为一件物品更有价值，例如，如果它被用来完成伟大的成就或者被一个受人钦佩的人使用。

Depending on the contract describing the token, each NFT could either be immutable, or you could allow users to customize and change certain characteristics about them if they choose to.

_ERC721_ is the most accepted standard for non-fungible tokens in the Ethereum Network. LAND tokens follow the ERC721 standard.

根据描述令牌的合同，每个NFT可以是不可变的，或者您可以允许用户根据他们的选择自定义和更改有关它们的某些特征。

_ERC721_是以太坊网络中不可替代的令牌最受欢迎的标准。 LAND令牌符合ERC721标准。

## Smart Contracts 智能合约

A contract consists of a both code (its methods) and data (its state) that resides at a specific address on the Ethereum blockchain.

The methods in a contract are always called via a transaction that has the _to_ field set to the contract’s address. The code that’s executed by the contract’s method can include calls to other contracts, these trigger more transactions that have the _from_ field set to the contract’s address.

契约由代码（其方法）和数据（其状态）组成，它们位于以太坊区块链的特定地址。

合同中的方法总是通过将_to_字段设置为合同地址的事务来调用。 由合同方法执行的代码可以包括对其他合同的调用，这些代码会触发将_from_字段设置为合同地址的更多事务。

A contract can’t trigger any actions on its own or based on a time event. All actions performed by a smart contract always arise from a transaction that calls one of the contract’s functions.
You can use smart contracts to condition transactions based on custom conditions. For example, users could stake a bet on the outcome of a game, and the corresponding payments would occur as soon as the outcome of the game is informed to the contract.
The entire code for a smart contract is public to whoever wants to read it. This allows developers to create publicly verifiable rules.
All Tokens are defined by a smart contract that specifies its characteristics and what can be done with it. Decentraland has written and maintains a number of smart contracts. LAND and MANA tokens themselves are defined by the _LANDregistry_ and _MANAtoken_ contracts respectively.

合同不能单独触发任何操作或基于时间事件触发任何操作。 智能合约执行的所有操作总是来自调用合约函数之一的事务。
您可以使用智能合约根据自定义条件调整事务。 例如，用户可以对游戏的结果下注，并且一旦将游戏的结果通知给合同，就会发生相应的支付。
智能合约的整个代码对于想要阅读它的人是公开的。 这允许开发人员创建可公开验证的规则。
所有令牌都由智能合约定义，该合约指定其特征以及可以使用它完成的任务。 Decentraland已经编写并维护了许多智能合约。 LAND和MANA令牌本身分别由_LANDregistry_和_MANAtoken_契约定义。

You can find the address of every contract created by Decentraland in [Decentraland smart contracts](https://contracts.decentraland.org/addresses.json).

You can read the full code of each of those contracts, as it's public information on the blockchain. You can find the contract by name on [Etherscan](https://etherscan.io/contractsVerified) and read its content there.

您可以在[Decentraland智能合约]（https://contracts.decentraland.org/addresses.json）中找到Decentraland创建的每份合约的地址。

您可以阅读每个合同的完整代码，因为它是区块链的公共信息。 您可以在[Etherscan]（https://etherscan.io/contractsVerified）上按名称查找合同，并在那里阅读其内容。

## dApps

_dApps_ (decentralized applications) are applications that are built upon smart contracts and the blockchain.

A dApp can be as simple as something that validates that your wallet holds a certain token and lets you use a service. Or it can be a fully fledged application with its own UI, such as the Decentraland Marketplace.

If you want to build your own dApp around Decentrlanad, see [Create a dApp]({{ site.baseurl }}{% post_url /blockchain-interactions/2018-01-09-create-a-dapp %}).

_dApps_（分散应用程序）是基于智能合约和区块链构建的应用程序。

dApp可以像验证您的钱包持有某个令牌并让您使用服务的东西一样简单。 或者它可以是具有自己的UI的完全成熟的应用程序，例如Decentraland Marketplace。

如果您想围绕Decentrlanad构建自己的dApp，请参阅[创建dApp]（{{site.baseurl}} {％post_url / blockchain-interactions / 2018-01-09-create-a-dapp％}）。

## Ropsten test network

Before you deploy a smart contract, create a new type of token, or a Decentraland scene that relies on transactions on the Ethereum network, you need to make sure that it has no bugs or gaps that malicious users could exploit.

The Ropsten test network is an alternative version of Ethereum that’s specifically made for running tests.

在部署智能合约，创建新类型的令牌或依赖于以太坊网络上的事务的Decentraland场景之前，您需要确保它没有恶意用户可以利用的漏洞或漏洞。

Ropsten测试网络是以太坊的替代版本，专门用于运行测试。

Tokens in the Ropsten network have no real value, so you can afford to make mistakes without running any real risk. You can replenish any lost tokens for free by using a faucet:

Ropsten网络中的令牌没有实际价值，因此您可以承担错误而不会冒任何实际风险。 您可以使用水龙头免费补充任何丢失的代币：

- Ropsten Ether faucet (https://faucet.ropsten.be/)
- Ropsten MANA faucet (https://faucet.decentraland.today/)

If you’re developing a scene that triggers transactions, testing these transactions in the Ropsten network is free, as the tokens you send don’t have a value. In mainnet you would otherwise have to pay at the very least a real gas fee in Ether for each test transaction you carry out.

Once you’re confident that your code works as expected and can’t be exploited, you can deploy to the Ethereum mainnet.

- Ropsten Ether水龙头（https://faucet.ropsten.be/）
- Ropsten MANA水龙头（https://faucet.decentraland.today/）

如果您正在开发一个触发事务的场景，那么在Ropsten网络中测试这些事务是免费的，因为您发送的令牌没有值。 在主网中，否则您必须为您执行的每项测试交易至少支付以太币的实际燃气费。

一旦您确信您的代码按预期工作且无法利用，您就可以部署到以太坊主网。

## Blockchain reorgs 区块链重组

Occasionally, multiple machines will create alternative new blocks at roughly the same time. This is a problem, because this forks the chain into two diverging versions that could potentially contradict each other. When a fork occurs, Ethereum solves this by always giving priority to the longest chain and discarding any shorter chains. Even though it’s possible for two rivaling chains to exist at the same time, soon one of the two chains will add another block and outgrow the other. Due to the time it takes to solve the mining algorithms, it becomes increasingly difficult for rivaling chains to keep growing in perfect sync with each other. Sooner or later one will prevail over the other.

有时，多台机器会在大致相同的时间创建替代新块。 这是一个问题，因为这会将链分成两个可能相互矛盾的分歧版本。 当叉子出现时，以太坊通过始终优先考虑最长的链并丢弃任何较短的链来解决这个问题。 尽管两个可以同时存在两个竞争链但很快就会有两个链中的一个会增加另一个块并且会增加另一个块。 由于解决挖掘算法所花费的时间，竞争链变得越来越难以保持彼此完美同步的增长。 迟早会胜过另一个。

When one chain outgrows the other and the dispute is resolved, machines that had adopted the shorter chain need to make adjustments. This is what’s known as a “reorg”. They need to roll back on all of the transactions included in the blocks from the branch they’re in until they reach the point at which the fork occurred. Then they need to add the new blocks from the longer branch that’s considered legitimate.

Rolled back transactions may return to the pool of pending transactions until they’re picked up again by a miner (or are discarded). Any gas fees paid for these transactions are also rolled back.

当一条链条超过另一条链条并且争议得到解决时，采用较短链条的机器需要进行调整。 这就是所谓的“重组”。 他们需要从他们所在的分支回滚块中包含的所有事务，直到它们到达fork出现的点。 然后他们需要从被认为合法的较长分支添加新块。

回滚交易可能会返回待处理交易池，直到被矿工再次取回（或被丢弃）。 为这些交易支付的任何燃气费也会回滚。

Blocks that were just added to the end of a chain have a substantial chance of being rolled back because of the mechanisms explained above. As subsequent blocks are added to the end of the chain, it becomes less and less likely that the blocks that are further back in the blockchain could be rolled back, because that would require a larger reorg. Due to this, each new block that’s added to the end of the chain after a transaction is called a confirmation for that transaction.

When creating applications (or scenes) that use information from off the blockchain, you should be aware of the occurrence of reorgs. You might want to only consider transactions as verified when a certain number of confirmations have occurred, and the transaction is no longer at the very end of the chain.

由于上面解释的机制，刚刚添加到链末尾的块很有可能被回滚。 随着后续块被添加到链的末端，进一步返回区块链中的块变得越来越不可能被回滚，因为这将需要更大的重组。 因此，在事务之后添加到链末尾的每个新块称为该事务的确认。

在创建使用区块链之外的信息的应用程序（或场景）时，您应该知道重新排序的发生。 您可能希望仅在发生一定数量的确认时将事务视为已验证，并且事务不再位于链的最后。

Using several confirmations will make the information very stable, but transactions will take a long time to be reflected.
Using few confirmations, changes will be reflected faster, but there will sometimes be hiccups that appear to undo transactions when reorgs occur. If these transactions have off-chain consequences in your scene, then you might need to somehow reverse these consequences as well.

使用多个确认将使信息非常稳定，但交易将需要很长时间才能反映出来。
使用很少的确认，更改将反映得更快，但有时会出现在发生重组时撤消事务的打嗝。 如果这些交易在您的场景中产生了链外后果，那么您可能还需要以某种方式扭转这些后果。