---
date: 2018-01-06
title: 示例
description: 使用 SDK 的一些代码和示例
categories:
  - examples
type: Document
set: examples
set_order: 1
---

为了帮助您快速入门，并阐明可以使用 SDK 构建的各种体验类型，我们将一些代码和场景示例放在一起。

## 初级示例

### 静态场景

这是个完全静态的场景示例。用来展示如何使用 blender 或 [Sketchfab](https://sketchfab.com/) 等资源构建您的第一个静态 Decentraland 场景。请查看此[链接](https://github.com/decentraland/sample-scenes/tree/master/01-static-scene)


### 动态动画

通过此动态动画，我们演示了如何将简单的数据绑定到场景中的对象中。[移动，旋转和缩放]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %})等属性都可以绑定到状态属性中。[链接](https://github.com/decentraland/sample-scene-dynamic-animation)


### 交互式内容

这个简单的示例场景展示了一个可以打开和关闭的门。单击门会产生一个 [event 事件]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-03-event-handling %})，来更改场景的状态。场景的状态然后改变门的旋转度，由于使用了 transition 过渡效果，门会平稳旋转。
[链接](https://github.com/decentraland/sample-scene-script)

### 骨骼动画

在场景中，您可以加载交互式 [GLTF 模型]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %})并触发其动画。这是如何做到这一点的一个例子。 [链接](https://github.com/decentraland/sample-scenes/tree/master/04-skeletal-animation)


## 中级示例

### 声音支持

这个例子的特点是一个发出[声音]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %})的实体，注意音量是如何随着距离的增加而减小的。它还包括一个动态的 GLTF 对象和一个随机改变颜色的地板。[链接](https://github.com/decentraland/sample-scene-sound-support)

#### 点唱机：按钮和声音

这个例子，在[视频教程](https://steemit.com/tutorial/@hardlydifficult/decentraland-tutorial-creating-a-music-jukebox)中有更详细的描述，你可以操作点唱机。每个按钮播放不同的歌曲。按钮是动画的，点击一个按钮会取消之前点击的其他按钮。
[链接](https://github.com/decentraland/sample-scene-jukebox)

#### 视频支持

在此示例中，您可以与两个视频播放器进行交互。一个将视频加载到场景的资源中，另一个使用外部源来加载。您还可以暂停、停止和更改视频播放器的音量。 [链接](https://github.com/decentraland/sample-scene-video-support)

#### 多人游戏

一个基于初级示例中门的例子，您可以通过打开和关闭门来与门互动，而另一个玩家则在同一个房间，可以看到门状态的改变。构建这个简单的示例是为了让您了解多用户环境是如何工作的，其中有多个用户与同一实体交互。 [链接](https://github.com/decentraland/sample-scene-server)

注：类似的例子在[博客](https://blog.decentraland.org/sdk-highlight-building-an-underwater-landscape-5bfcce73ff35)中有更详细的讨论。

#### 动态实体数量

In this example, that's described in greater detail in a [blogpost](https://blog.decentraland.org/developer-tutorial-creating-a-dynamic-flock-of-hummingbirds-8c2cd41f8296), a new bird appears and starts flying randomly around the scene each time you click on a tree. It's a good example of how to build multiple entities from an array and of how to handle 3D model animations.
[Link](https://github.com/decentraland/sample-scene-array-of-entities/blob/master/README.md)

这个例子中，在 [博客](https://blog.decentraland.org/developer-tutorial-creating-a-dynamic-flock-of-hummingbirds-8c2cd41f8296) 中有更详细的描述，每次点击树都会有一只新的鸟出现并开始在场景中随机飞行。这是如何从数组构建多个实体以及如何处理 3D 模型动画的一个很好的示例。
[链接](https://github.com/decentraland/sample-scene-array-of-entities/blob/master/README.md)


## 高级示例

#### 简单记忆游戏

在这个例子中，在[博客](https://blog.decentraland.org/building-a-memory-game-using-decentralands-sdk-87ee35968f8d)中有更详细的描述，你玩的是一个“西蒙说”的游戏。这个游戏是如何将更复杂的逻辑添加到场景中以及如何根据用户与其交互方式更改其状态的一个很好的示例。 [链接](https://github.com/decentraland/sample-scene-memory-game)


#### 付款使用

这是个基于初级示例中的门的例子，你只有支付 10 MANA 到特定的钱包，你才能打开这个门。这个示例展示了如何使用 SDK 来跟踪区块链交易。
[链接](https://github.com/decentraland/sample-scene-payments)

#### Block Dog

这个例子在[视频教程](https://steemit.com/tutorial/@hardlydifficult/decentraland-tutorial-basic-ai-with-block-dog)中有更详细的描述，你可以控制一只宠物狗。 狗有自己的随机地自主行为。它也会跟着你走来走去，或坐在那里，当你点击碗时就会去喝水。
[链接](https://github.com/decentraland/sample-scene-Block-Dog)

#### 国际象棋游戏

This example, that's described in greater detail in a [blogpost](https://blog.decentraland.org/developer-tutorial-port-a-redux-chess-game-to-decentraland-49f509b2eba6), takes an existing 2D chess game and builds a 3D scene around it in decentraland. The game can only start when two players have accepted to join the game, and each can only interact with the scene when it's their turn.
[link](https://github.com/cazala/decentraland-redux-chess-app)

这个例子在[博客](https://blog.decentraland.org/developer-tutorial-port-a-redux-chess-game-to-decentraland-49f509b2eba6)中有更详细的描述，取自现有的 2D 国际象棋游戏，然后在 decentraland 3D 场景中构建。只有在两个玩家接受加入游戏，游戏才会开始，并且每个玩家只有在轮到他们时才能与场景互动。
[链接](https://github.com/cazala/decentraland-redux-chess-app)

#### 塔防游戏

这个例子在[视频教程](https://steemit.com/tutorial/@hardlydifficult/decentraland-tutorial-a-simple-tower-defense-game)中有更详细的描述，它展示了一个简单的塔防游戏。游戏生成随机路径，并沿着该路径随机放置陷阱。然后生成敌人实体并遵循此路径过来，除非您激活陷阱以阻止它们。该游戏支持多个玩家，有一个记分牌，并有一个重置按钮，可以随时重启游戏。
[链接](https://github.com/decentraland/sample-scene-tower-defense-game)