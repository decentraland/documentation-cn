---
date: 2018-01-07
title: 场景区块链操作
description: SDK 以太坊区块链操作功能
redirect_from:
  - /sdk-reference/blockchain-operations/
categories:
  - blockchain-interactions
type: Document
set: blockchain-interactions
set_order: 5
---

Decentraland 场景可以与以太坊区块链进行交互。 可用于获取有关用户钱包及其通证的相关数据，或触发任何以太坊通证（所有可替代或不可替代通证 NFT）交易。有多种方式来使用它们，例如销售通证，作为游戏机制的一部分奖励通证，当用户如果拥有某些通证时改变场景的交互方式等。

目前可以使用以下工具，所有这些工具均由 Decentraland 提供：

<!--
- The `Identity` library: used to obtain general user data.
-->

- `Ethereum controller`：一个功能有限简单的基本库。
- `eth-connect` 库：一个与以太坊合约交互并调用其功能的低级库，例如触发事务或检查余额。

请注意，场景触发的所有交易都需要用户批准并支付交易费用。

所有有关区块链操作需作为[异步函数]({{ site.baseurl }}{% post_url /development-guide/2018-02-25-async-functions %})调用，因为所需时间取决于外部事件。

<!--

## User identity

#### Import the identity library

The identity library is ported with the Decentraland ECS. Simply import it into a scene, no additional steps are needed.

```ts
import { getUserPublicKey, getUserData } from '@decentraland/Identity'
```

#### Get a public key

You can obtain a user's public Ethereum key by using the `getUserPublicKey()` function.


```ts
import { getUserPublicKey } from '@decentraland/Identity'

executeTask(async () => {
  let key = await getUserPublicKey()
  log(key)
})

```
The user's public key is necessary to send payments or other transactions that involve the user. The public key can also be used as a persisting ID to recognize a user over multiple sessions.

> Note: The user must be logged into their Metamask account on their browser for this method to work.

#### Get other user data

You can obtain other user data via the `getUserData()` function. This function returns the following data:

- displayName: the user's name Decentraland
- publicKey: the user's Ethereum public key

```ts
import { getUserPublicKey } from '@decentraland/Identity'

executeTask(async () => {
  let userData = await getUserData()
  log(userData)
})
```

Users can change their display name at any time while in Decentraland. For this reason, the public key is generally a more recommendable way to keep track of users over time.

> Note: The user must be logged into their Metamask account on their browser for this method to work.

-->

## 高级操作

在以太坊区块链上执行操作的最简单方法是通过 _ethereum controller_ 库。 SDK 集成了该控制器，因此无需进行安装。

将 Ethereum controller 导入场景文件：

```ts
import * as EthereumController from "@decentraland/EthereumController"
```

下面介绍一些使用此控制器执行的操作。

#### 消息签名

用户可以使用他们的以太坊公钥对消息进行签名。 此签名是一种安全的方式来表示同意或注册区块链操作事件。

签署消息不是交易，因此并不意味要在以太坊网络上支付交易费用，但它会打开一个弹出窗口，要求用户同意。

可以签名的消息需要遵循特定格式以符合安全要求。 他们必须在顶部包含“Decentraland 签名标题”，这可以防止用户钱包管理不善的可能性。

可签名消息应遵循以下格式：

```
# DCL Signed message
<key 1>: <value 1>
<key 2>: <value 2>
<key n>: <value n>
Timestamp: <time stamp>
```

例如，游戏的可签名消息可能如下所示：

```ts
# DCL Signed message
Attacker: 10
Defender: 123
Timestamp: 1512345678
```

在用户签署消息之前，必须首先使用 `convertMessageToObject()` 函数将其从字符串转换为对象，然后可以使用 `signMessage()` 函数进行签名。

```ts
import * as EthereumController from "@decentraland/EthereumController"

const messageToSign = `# DCL Signed message
Attacker: 10
Defender: 123
Timestamp: 1512345678`

let eth = EthereumController

executeTask(async () => {  
  const convertedMessage = await eth.convertMessageToObject(messageToSign)
  const { message, signature } = await eth.signMessage(convertedMessage)
  log({ message, signature })
})
```


#### 检查消息是否正确

要验证用户签名的消息实际上就是您要发送的消息，可以使用 `eth-connect` 库中的 `toHex()` 函数进行转换并轻松进行比较。 有关如何导入 `eth-connect` 库的说明，请参见下文。


```ts
import * as EthConnect from '../node_modules/eth-connect/esm'
import * as EthereumController from "@decentraland/EthereumController"


const messageToSign = `# DCL Signed message
Attacker: 10
Defender: 123
Timestamp: 1512345678`

let eth = EthereumController

function signMessage(msg: string){
  executeTask(async () => {  
    const convertedMessage = await eth.convertMessageToObject(msg)
    const { message, signature } = await eth.signMessage(convertedMessage)
    log({ message, signature })

    const originalMessageHex = await  EthConnect.toHex(msg)
    const sentMessageHex = await  EthConnect.toHex(message)
    const isEqual = sentMessageHex === originalMessageHex
    log("Is the message correct?", isEqual)
  })
}

signMessage(messageToSign)
```

#### 请求付款

`requirePayment()` 函数提示用户接受向您选择的以太坊钱包支付一笔款项。

用户必须始终手动确认付款，永远不会因为用户在场景中的操作而直接付款。

```ts
eth.requirePayment(receivingAddress, amount, currency)
```

该功能要求您指定以太坊钱包地址以接收付款，交易金额和要使用的特定货币（例如，MANA 或 ETH）。

如果用户接受，则该函数返回交易的哈希值。

> 警告：此功能通知您已请求交易，但未通知确认。 如果 gas 价格太低，或因任何原因末被矿工确认交易，则交易将无法完成。


```ts
const myWallet = ‘0x0123456789...’
const enterPrice = 10

function payment(){
  executeTask(async () => {
    try {
      await eth.requirePayment(myWallet, entrancePrice, ‘MANA’)
      openDoor()
    } catch {
      log("failed process payment")
    }
  })
}

const button = new Entity()
button.addComponent(new BoxShape())
button.addComponent(new OnClick( e => {
    payment()
  }))
engine.addEntity(button)
```

上面的示例侦听 _button_ 实体的点击。 单击时，系统会提示用户使用给定金额 MANA 支付给特定钱包。 一旦用户接受此付款，就可以执行该功能的其余部分。 如果用户不接受付款，则不会执行该功能的其余部分。

![](/images/media/metamask_confirm.png)

> 提示：我们建议在 _.ts_ 文件的开头定义钱包地址和支付金额作为全局常量。您将来可能需要更改这些值，将它们设置为常量可以更轻松地更新代码。

#### 等待交易确认

在 Ethereum controller 中可以检查特定交易是否已经确认。 它会查找特定交易的哈希值，并验证它是否已由矿工确认添加到区块链中。

> 重要提示：由于区块链的工作方式，确认的交易有可能会[回滚]({{ site.baseurl }}{% post_url /blockchain-interactions/2018-01-01-ethereum-essentials %}#blockchain-reorgs) 。只由一个节点确认一次的交易无法保证最终达到网络共识。 我们不建议依靠此功能来处理有价值的事情。

```ts
await this.eth.waitForMinedTx(currency, tx, receivingAddress)
```

该功能需要指定使用的货币（例如，MANA 或 ETH），交易哈希值和收款的以太坊钱包地址。

```ts
const myWallet = ‘0x0123456789...’
const enterPrice = 10

function payment(){
  executeTask(async () => {
    try {
      const tx = await eth.requirePayment(myWallet, entrancePrice, ‘MANA’)
      await eth.waitForMinedTx(‘MANA’, tx, myWallet)
      openDoor()
    } catch {
      log("failed process payment")
    }
  })
}

const button = new Entity()
button.addComponent(new BoxShape())
button.addComponent(new OnClick( e => {
    payment() 
  }))
engine.addEntity(button)
```

上面的例子首先要求用户确认交易，如果用户确认，那么 `requirePayment` 返回一个可用于跟踪交易并查看它是否被确认的哈希值。 一旦交易确认并作为区块链的一部分被接受，就会调用 `openDoor()` 函数。

#### 异步发送

使用函数 `sendAsync()` 通过[RPC 协议](https://en.wikipedia.org/wiki/Remote_procedure_call)发送消息。

```ts
import * as EthereumController from "@decentraland/EthereumController"

// send a message
await eth!.sendAsync(myMessage)
```

## 较低级别的操作

eth-connect 库由 Decentraland 制作和维护。 它基于流行的 [Web3.js](https://github.com/ethereum/web3.js/)库，但它完全用 TypeScript 编写，并进行了一些安全性改进。

该控制器运行于 _Ethereum controller_ （实际上 _Ethereum controller_ 是基于它构建的）的底层，因此使用起来更难，但更灵活。

它的主要用途是在合约中调用函数，它还为各种任务提供了许多辅助函数。 在[GitHub](https://github.com/decentraland/eth-connect)上查看。

> 注意：eth-connect 库目前缺少更深入的文档。 由于这个库主要基于 Web3.js 库，并且大多数函数名称都与 Web3.js 中的函数名称保持一致，因此通常可以参考[Web3的文档](https://web3js.readthedocs.io/en/1.0/)。

#### 下载并导入 eth-connect 库

使用 eth-connect 库，必须在场景文件夹中使用 `npm` 手动安装软件包。 为此，请在场景文件夹中运行以下命令：

```
npm install eth-connect
```

> 注意：Decentraland 场景不支持 eth-connect 库 4.0 以下的旧版本。

> 注意：目前，我们不允许通过 npm 安装非 Decentraland 创建的其他依赖项。 这是为了让场景在沙盒中运行以防止恶意代码。

安装完成后，必须将 `eth-connect` 导入到场景代码中：

```ts
import * as EthConnect from '../node_modules/eth-connect/esm'
```

#### 导入合约 ABI

ABI（应用程序二进制接口）描述了如何与以太坊合约交互，有哪些可用的函数，它们有哪些输入，以及输出什么。 每个以太坊合约都有自己的 ABI，您应该导入您希望在项目中使用的所有合同的 ABI。

例如，这是 MANA ABI 中一个函数的示例：

```ts
{
  anonymous: false,
  inputs: [
    {
      indexed: true,
      name: 'burner',
      type: 'address'
    },
    {
      indexed: false,
      name: 'value',
      type: 'uint256'
    }
  ],
  name: 'Burn',
  type: 'event'
}
```

ABI 定义可能非常冗长，因为它们通常包含很多函数，因此我们建议将 ABI 文件的 JSON 内容粘贴到单独的 `.ts` 文件中，然后将其导入到其他场景文件中。 我们还建议将所有 ABI 文件保存到命名为 `/contracts` 的单独文件夹中。

```ts
import {abi} from '../contracts/mana'
```

以下是一些有用的 ABI 定义的链接：

- [MANA ABI](https://etherscan.io/address/0x0f5d2fb29fb7d3cfee444a200298f468908cc942#code)

<!--
- fake MANA?:

- LAND?
-->

#### 合约实例

在导入 `eth-connect` 库和合同的 _abi_ 之后，您必须实例化多个对象，这些对象将允许您使用合约中的函数并在用户的浏览器中连接到 Metamask。

您还必须导入 web3 provider。 这是因为用户浏览器中的 Metamask 使用 web3，因此我们需要一种与之交互的方式。

```ts
import * as EthConnect from '../node_modules/eth-connect/esm'
import {abi} from '../contracts/mana'
import { getProvider } from '@decentraland/web3-provider'

executeTask(async () => {
  // create an instance of the web3 provider to interface with Metamask
  const provider = await getProvider()
  // Create the object that will handle the sending and receiving of RPC messages
  const requestManager = new EthConnect.RequestManager(provider)
  // Create a factory object based on the abi
  const factory = new EthConnect.ContractFactory(requestManager, abi)
  // Use the factory object to instance a `contract` object, referencing a specific contract
  const contract = (await factory.at('0x2a8fd99c19271f4f04b1b7b9c4f7cf264b626edb')) as any
})
```

请注意，必须使用 `await` 调用其中一些函数，因为它们依赖于外部数据获取，并且可能需要一些时间才能完成。

> 提示：对于遵循相同标准的合同，例如 ERC20 或 ERC721，可以导入单个通用 ABI。 然后，您使用该ABI 生成一个 `ContractFactory` 对象，并将相同 factory 来对实便每个合约的接口。

#### 调用合同中的方法

一旦创建了 `contract` 对象，就可以轻松调用其 ABI 中定义的函数，并将指定的输入参数传递给它。

```ts
import * as EthConnect from '../node_modules/eth-connect/esm'
import {abi} from '../contracts/mana'
import { getProvider } from '@decentraland/web3-provider'
import { getUserPublicKey } from '@decentraland/Identity'


executeTask(async () => {
  // Setup steps explained in the section above
  const provider = await getProvider()
  const requestManager = new EthConnect.RequestManager(provider)
  const factory = new EthConnect.ContractFactory(requestManager, abi)
  const contract = (await factory.at('0x2a8fd99c19271f4f04b1b7b9c4f7cf264b626edb')) as any

  // Perform a function from the contract
  const res = await contract.setBalance('0xaFA48Fad27C7cAB28dC6E970E4BFda7F7c8D60Fb', 100, {
    from: await getUserPublicKey()
  })
  // Log response
  log(res)
})

```

上面的示例使用 Ropsten MANA 合约 abi，并将 100 _测试用 MANA_ 转移到 Ropsten 测试网络中的帐户。

#### 其他功能

eth-connect 库包含许多您可以使用的其他帮助程序。 例如：

 - 估算预计 gas 价格
 - 获取给定地址的余额
 - 获取交易收据
 - 获取从指定地址发送的交易数量
 - 在各种格式之间转换，包括十六进制，二进制，utf8 等。

<!--

#### Obtain gas price


 * Generates and returns an estimate of how much gas is necessary to allow the transaction to complete.
     * The transaction will not be added to the blockchain. Note that the estimate may be significantly more
     * than the amount of gas actually used by the transaction, for a variety of reasons including EVM mechanics
     * and node performance.
     */
    eth_estimateGas: (data: Partial<TransactionCallOptions> & Partial<TransactionOptions>) => Promise<Quantity>;
    /** Returns information about a block by hash. */

  @exposeMethod
  async estimateGas(data: Partial<TransactionCallOptions> & Partial<TransactionOptions>): Promise<Quantity> {
    return requestManager.eth_estimateGas(data)
  }

#### Get Balance

 /** Returns the balance of the account of given address. */
    eth_getBalance: (address: Address, block: BlockIdentifier) => Promise<BigNumber>;


      @exposeMethod
  async getBalance(address: Address, block: Quantity | Tag): Promise<BigNumber> {
    return requestManager.eth_getBalance(address, block)
  }

## Get data from executed transactions

 /**
     * Returns the receipt of a transaction by transaction hash.
     * Note That the receipt is not available for pending transactions.
     */
    eth_getTransactionReceipt: (hash: TxHash) => Promise<TransactionReceipt>;

@exposeMethod
  async getTransactionReceipt(hash: TxHash): Promise<TransactionReceipt> {
    return requestManager.eth_getTransactionReceipt(hash)
  }


  /** Returns the number of transactions sent from an address. */
    eth_getTransactionCount: (address: Address, block: BlockIdentifier) => Promise<number>;

  @exposeMethod
  async getTransactionCount(address: Address, block: Quantity | Tag): Promise<number> {
    return requestManager.eth_getTransactionCount(address, block)
  }
-->

<!--
## Web3 API

We have only whitelisted the following methods from the API, all others are currently not supported:

- eth_sendTransaction
- eth_getTransactionReceipt
- eth_estimateGas
- eth_call
- eth_getBalance
- eth_getStorageAt
- eth_blockNumber
- eth_gasPrice
- eth_protocolVersion
- net_version
- eth_getTransactionCount
- eth_getBlockByNumber


Below is a sample that uses this API to get the contents of a block in the blockchain.


```ts
import { createElement, ScriptableScene } from "decentraland-api"
import Web3 = require("web3")

export default class EthereumProvider extends ScriptableScene {
  async sceneDidMount() {
    const provider = await this.getEthereumProvider()
    const web3 = new Web3(provider)

    web3.eth.getBlock(48, function(error: Error, result: any) {
      console.log("Eth block 48 (from scene)", result)
    })
  }

  async render() {
    return <scene />
  }
}
```


-->




<!--

const signature = await requestManager.personal_sign(message, account, 'test')
const signerAddress = await requestManager.personal_ecRecover(message, signature)


/**
     * The sign method calculates an Ethereum specific signature with:
     *   sign(keccack256("Ethereum Signed Message:" + len(message) + message))).
     *
     * By adding a prefix to the message makes the calculated signature recognisable as an Ethereum specific signature.
     * This prevents misuse where a malicious DApp can sign arbitrary data (e.g. transaction) and use the signature to
     * impersonate the victim.
     *
     * See ecRecover to verify the signature.
     */
    personal_sign: (data: Data, signerAddress: Address, passPhrase: Data) => Promise<Data>;
    /**
     * ecRecover returns the address associated with the private key that was used to calculate the signature in
     * personal_sign.
     */
    personal_ecRecover: (message: Data, signature: Data) => Promise<Address>;
    constructor(provider: any);

-->

## 使用以太坊测试网络

在测试场景时，为避免使用真正的 MANA 货币，您可以使用 _Ethereum Ropsten 测试网络_ 来交易 MANA。

要使用测试网络，您必须将 Metamask Chrome 扩展程序设置为使用 _Ropsten test network_ 而不是 _Main network_。

您还必须在 Ropsten 区块链中拥有 MANA。 要在测试网络中获得免费的 Ropsten MANA，请访问[MANA faucet](https://faucet.decentraland.today/)。

> 提示：要运行将 Ropsten MANA 转移到钱包的交易，您需要在 Ropsten Ether 支付 gas 费用。 如果您没有 Ropsten Ether，您可以[在这里](https://faucet.ropsten.be/)免费获得。

要使用测试网络预览场景，请将 `DEBUG` 属性添加到浏览器场景预览的 URL 上。 例如，如果您通过`http://127.0.0.1:8000/?position=0%2C-1` 访问场景，则应将 URL 设置为 `http://127.0.0.1:8000/?DEBUG&position=0%2C-1`。

在此模式下测试场景时接受的任何交易只会发生在测试网络中，而不会影响真正钱包中的 MANA 余额。