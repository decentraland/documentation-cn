---
date: 2018-02-07
title: 在场景中显示 NFT
description: 如何在场景中显示您拥有的通过认证的 NFT
categories:
  - blockchain-interactions
type: Document
set: blockchain-interactions
set_order: 4
---

可以在 Decentraland 场景中显示您拥有的 2D NFT（不可替换通证）。 在 NFT 四周会出现一个发光的相框，以证明您确实拥有它。

NTF 的图像数据通过通证合约和 id 从 API 中获取。

目前，只支持有限几种 NFT:

- CriptoKitties
- Editional
- Makersplace
- KnownOrigin
- Axie Infinity
- MyCryptoHeroes
- MLB Champions
- Chibi Fighters
- Blockchain Cuties
- HyperDragons
- Chainbreakers
- Chainguardians
- Cryptomorph
- Josie
- Blockcities

显示的相框大小会根据 NFT 图像的尺寸进行调整。如果图像是 512 X 512 像素，则相框会保持其原来的大小。如果图像大小不同，相框将调整自己尺寸来匹配。

> 提示：如果您想拉伸或调整默认生成的图像的大小，您可以更改实体的 `Transform` 组件中的 `scale` 属性。

> 注意：在未来的版本中，当使用 `NFTShape` 组件时，引擎将自动运行验证。拥有布署 LAND 通证的以太钱包必须拥有此通证，如果地址不匹配，图像将显示在场景中，但不会发光来标志这是个普通 NFT。

## 添加 NFT

将 `NFTShape` 组件添加到实体以便在场景中显示 2D 通证。

```ts
const entity = new Entity()
const shapeComponent = new NFTShape('ethereum://0x06012c8cf97BEaD5deAe237070F9587f8E7A266d/558536')
entity.addComponent(shapeComponent)
entity.addComponent(
  new Transform({
    position: new Vector3(4, 1.5, 4)
  })
)
engine.addEntity(entity)
```

`NFTShape` 组件有两个参数：

- 通证的_合约地址_（例如，CryptoKitties 合约）
- 您拥有的特定通证的 _id_

上面的示例使用合约地址 `0x06012c8cf97BEaD5deAe237070F9587f8E7A266d` 和特定的标识 `558536` 获取 NFT。 相应的资源可以在 OpenSea 的 [https://opensea.io/assets/0x06012c8cf97BEaD5deAe237070F9587f8E7A266d/558536](https://opensea.io/assets/0x06012c8cf97BEaD5deAe237070F9587f8E7A266d/558536) 中找到。

通证显示时，四周会有一个发光的相框。

可以通过更改 `NFTShape` 中的 `color` 来更改框架的背景颜色。 也会改变模型的背面，以及在 NFT 图像具有透明度的情况下图像的背景。

```ts
const shapeComponent = new NFTShape('ethereum://0x06012c8cf97BEaD5deAe237070F9587f8E7A266d/558536', Color3.Green())
```

 <img src="/images/media/nft-cat.png" alt="Move entity" width="300"/>


<!--
## 通证验证

使用 `NFTShape` 组件时，引擎会自动进行验证。部署场景的 LAND 通证的同一个以太坊地址也必须有该通证。

如果您不拥有此通证，则图像不会显示在场景中。

每次 `NFTShape` 组件的实体添加到引擎时，用户加载场景时都会进行验证。

在通证图像上方，会显示了一个用于验证真实性的徽章。这个闪烁的徽章构成了一个难以伪造的印章。

--->