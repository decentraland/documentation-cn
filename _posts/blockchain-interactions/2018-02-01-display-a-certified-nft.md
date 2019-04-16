---
date: 2018-02-07
title: 在场景中显示 NFT
description: 在场景中如何显示您拥有的通过认证的 NFT
categories:
  - blockchain-interactions
type: Document
set: blockchain-interactions
set_order: 4
---

You can display an NFT (Non-Fungible Token) that you own in your Decentraland scenes. A glowing badge will appear above the NFT to certify that you indeed own it.

您可以在 Decentraland 场景中显示您拥有的 NFT（不能替换通证）。在 NFT 上方会出现一个发光的徽章，用以证明您确实拥有此通证。

<img src="/images/media/verified-nft.png" alt="nested entities" width="300"/>

> Note: The NTF's image data is taken from the [OpenSea](https://opensea.io/) API, based on the token's contract and id. Note that most images from the OpenSea API are 2D, but categories like [Chainbreakers](https://opensea.io/assets/chainbreakerspresale) will return 3D assets.

> 注意：NTF 的图像数据取自[OpenSea](https://opensea.io/) API，基于通证的的合约地址和 ID。请注意，OpenSea API 中的大多数都是 2D 图像，但在[Chainbreakers](https://opensea.io/assets/chainbreakerspresale) 这样的类别中会返回 3D 的素材。


## Add an NFT
## 添加 NFT

Add an `NFTShape` component to an entity to display a 2D token in your scene.

将 `NFTShape` 组件添加到实体以便在场景中显示 2D 通证。


```ts
// create entity
const nft = new Entity()

// position entity
nft.add(new Transform({
  position: new Vector3(1, 1.2, 1)
  }))

// add an NFTShape, instanced with a token contract and token id
nft.add(new NFTShape("0x06012c8cf97bead5deae237070f9587f8e7a266d", "475577"))

// add entity to engine
engine.addEntity(nft)
```

The `NFTShape` component must be instanced with two parameters:

`NFTShape` 组件有两个参数：


- The _contract_ of the token (for example, the CryptoKitties contract)
- The _id_ of the specific token you own

- 通证的_合约_（例如，CryptoKitties 合约）
- 您拥有的特定通证的 _id_

When the token is displayed, it's shown as a plane object. Its texture is shadeless, like a basic material.

通证显示为为平面对象。无阴影的，就像一种基本材质。


## Token certification
## 通证验证


When using the `NFTShape` component, the engine automatically runs a verification. The same Ethereum wallet that owns the LAND tokens where the scene is deployed must also own the token.

使用 `NFTShape` 组件时，引擎会自动进行验证。部署场景的 LAND 通证的同一个以太坊地址也必须有该通证。

If you don't own the token, the image isn't displayed in the scene.

如果您不拥有此通证，则图像不会显示在场景中。

This verification is carried out by users loading your scene, every time the entity with the `NFTShape` component is added to the engine.

每次 `NFTShape` 组件的实体添加到引擎时，用户加载场景时都会进行验证。

Above the image of the token, we display a badge that certifies its authenticity. The badge glows in a pulsating pattern, providing a stamp that is difficult to falsify.

在通证图像上方，会显示了一个用于验证真实性的徽章。这个闪烁的徽章构成了一个难以伪造的印章。

> Note: If you want to display a token that you don't own, or prefer to display it without the authenticity badge, you can otherwise obtain an NFT image from [OpenSea's API](https://docs.opensea.io/reference#api-overview) and set that as a texture for a material. Then you can use that material on any primitive object in the scene.

> 注意：如果您想显示一个您并不拥有的通证，或者要在没有真品徽章的情况下显示，您可以从[OpenSea's API](https://docs.opensea.io/reference#api-overview) 获取 NFT 图像，并将其设置为材质的纹理。然后，就可以在场景中的任何基本对象上使用。
