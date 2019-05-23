---
date: 2018-01-06
title: 场景示例
description: SDK 开发代码和场景示例
categories:
  - examples
type: Document
set: examples
set_order: 1
---

为了帮助您快速入门，以及说明可以使用 SDK 构建的体验类型，我们将一些代码和场景示例放在这里。

其中一些场景还有部署在远程服务器上的链接。 点击后可以直接交互，就像在本地运行 `dcl start` 一样。

## 克隆示例场景

可以克隆现有的示例场景来开始项目，而不是从头开始创建新场景。

为此：

1. 从下面列出的示例中找到您喜欢的示例。
2. 点击 **代码** 链接访问 GitHub 仓库。
3. 从那里你可以：
   1. _Fork 代码_ 克隆到自己的 GitHub 仓库中，然后在你自己的版本中修改。
   2. 单击 **Clone or Download** 然后下载为 _.zip_ 文件，这样在本地处理文件，而不涉及GitHub。

<!--
## 静态 XML 场景

#### 静态 XML 场景

完全静态的场景。 使用 XML 构建，容易编写和编辑，但不支持随时间的任何变化或与用户的交互。

![](/images/media/example-static.png)

[代码](https://github.com/decentraland-scenes/XML-static-scene)
-->

## 基础

#### 具有动画的静态场景

显示一个具有移动的蝴蝶，火焰等的动画的 3D 模型的静态场景。

![](/images/media/example-muna.png)

[代码](https://github.com/decentraland-scenes/the-munastery)

[浏览场景](https://the-munastery-bsapskmwaq.now.sh)


#### 催眠轮

这个简单的场景有几个轮子，可以点击让它们旋转。

 - 旋转实体
 - glTF 模型
 - 点击事件
 - 纹理
 - 自定义组件
 - 组件组

![](/images/media/example-hypno-wheel.png)

[代码](https://github.com/decentraland-scenes/Hypno-wheels)

[浏览场景](https://hypno-wheels-owyfnqfimw.now.sh/?position=0%2C-1)

[有关此场景的教程](https://decentraland.org/blog/tutorials/intro-to-sdk-v5) 

#### 动画鲨鱼

介绍了如何将动画添加到 `GLTFComponent` 并处理单击事件。

 - glTF 模型
 - 动画
 - 点击事件

![](/images/media/example-shark-animation.png)

[代码](https://github.com/decentraland-scenes/Shark-animation)

[浏览场景](https://shark-animation-xriykgapld.now.sh/)

有关此场景的教程:

- [第一部分]((https://decentraland.org/blog/tutorials/motion-animations-in-SDK-5))
- 第二部分 (即将发布)

#### 可打开的门

一个简单的交互式场景，可以打开和关闭一扇门。

 - `Slerp()` 旋转功能
 - 点击事件
 - 材质
 - 父实体
 - 自定义组件
 - 组件组

![](/images/media/example-door.png)

[代码](https://github.com/decentraland-scenes/Open-door)

[浏览场景](https://open-door-gssoyhoyrt.now.sh)

#### 滑行门

带有双面门的简单交互式场景，可通过单击打开。

 - `Lerp()` 移动功能
 - 点击事件
 - 材质
 - 父实体
 - 自定义组件
 - 组件组

![](/images/media/example-sliding-doors.png)

[代码](https://github.com/decentraland-scenes/Sliding-door)

[浏览场景](https://slidingdoor-fmydyuprjl.now.sh)

#### 点唱机

一个通过按钮可以播放不同歌曲的场景。

 - 音频
 - glTF 模型
 - `Lerp()` 移动功能
 - 点击事件
 - 材质
 - 父实体
 - 自定义组件
 - 组件组

![](/images/media/example-jukebox.png)

[代码](https://github.com/decentraland-scenes/Jukebox)

<!--
[浏览场景]()
-->

#### 舞池
一个有动画、声音和的变色地板的场景，地板会根据节拍随机改变颜色。

 - 音频
 - glTF 模型
 - 动画
 - 材质
 - 自定义组件
 - 组件组

![](/images/media/example-dance-floor.png)

[代码](https://github.com/decentraland-scenes/Dance-floor)

<!--
[浏览场景]()
-->

#### 烟雾

此场景演示如何用粒子系统来创建烟雾。每个烟雾是一个沿特定方向移动的实体。这些实体在对象池中重用，而不是创建新实体。当一个实体浮动远离火焰时，它将从场景中移除并在对象池中等待重用。

![](/images/media/example-smoke.png)

[代码](https://github.com/decentraland-scenes/Smoke)

[浏览场景](https://smoke-iqntujtjgb.now.sh)

#### 记忆游戏

一个“西蒙说”游戏，通过点击顺序互动。 游戏会生成随机的颜色序列，您必须单击匹配的相应按钮。

 - glTF 模型
 - 材质
 - 点击事件
 - 自定义组件
 - 组件组

![](/images/media/example-memory-game.png)

[代码](https://github.com/decentraland-scenes/Memory-game)

<!--
[Explore the scene]()
-->

## 动作

#### 蜂鸟

单击树会生成蜂鸟。 每只鸟自己随机移动位置。

![](/images/media/example-hummingbirds.png)

[代码](https://github.com/decentraland-scenes/Hummingbirds)

[浏览场景](https://hummingbirds-ujovmbtmui.now.sh/)

#### 巡逻的 Gnark

一个沿着固定路径行走的角色，在路径上使用了 lerp 线性插值。 如果你接近它，它会转向对你大喊大叫。

![](/images/media/example-gnark.png)

[代码](https://github.com/decentraland-scenes/Gnark-patrol)

[浏览场景](https://gnark-patrol-azhbtehsge.now.sh/)

#### 游泳的鲨鱼

场景展示了鲨鱼在一个弯曲的圆形路径上绕圈，每部分路径使用 lerp。并通过一个球形的 lerp函数平滑地旋转。

鲨鱼的速度和游动的强度取决于这一段曲线的陡度。

![](/images/media/example-shark-animation.png)

[Code](https://github.com/decentraland-scenes/Swimming-shark)

[Explore the scene](https://swimming-shark-mfakqegnto.now.sh)

## 网络联接

#### 天气模拟

根据天气 API 及位置，显示天气状况，如下雨、打雷或下雪
通过更改坐标来使用来自不同位置的真实天气数据，或更改 “fakeWeather” 变量的值以查看不同的天气状况。

- 调用 REST API

![](/images/media/example-weather.png)

[代码](https://github.com/decentraland-scenes/Weather-simulation)

[浏览场景](https://weather-yvahddfxgo.now.sh/)

#### 一个支持多用户的门

使用服务器和 REST API 在多个用户之间同步场景状态。 基于“可打开的门”基础示例。

- 创建 REST 服务
- 调用 REST API

![](/images/media/example-door.png)

[代码](https://github.com/decentraland-scenes/Remote-door)

#### 壁画

使用服务器和 REST API 在多个用户之间同步场景状态的场景。 您可以在壁画中绘制像素，同时其他用户也可以看到。 每个像素的颜色存储在远程服务器中。

- 创建 REST 服务
- 调用 REST API

![](/images/media/example-remote-mural.png)


[代码](https://github.com/decentraland-scenes/Remote-mural)

<!--
[Explore the scene]()
-->

## 区块链交易

#### MANA 交易

使用 MANA 智能合约和 EthConnect 库在 Ropsten 测试网络上发送 MANA 给用户。

[代码](https://github.com/decentraland-scenes/MANA-Transaction)

[浏览场景](https://mana-transaction-sxjmryeayj.now.sh)

#### MANA 祭坛

虚拟市场收取的手续费用存储在钱包中。这个场景与 MANA 合同相互作用，将这些 MANA 燃烧销毁。

火焰被作为一个粒子系统，用以处理各个旋转并变换颜色的实体。

![](/images/media/example-mana-altar.png)

[代码](https://github.com/decentraland-scenes/MANA-Burning-Altar)

[浏览场景](https://mana-altar-master-iehcppnlvz.now.sh/)

## 高级示例

#### 狗

一个具有简单 AI 特征的场景。 它会随机选择要采取的行动：跟随你、坐下或保持闲置状态。 您也可以通过点击它来让它坐下或站起来，或者通过点击碗让它去喝水。

![](/images/home/blockdog.png)

[代码](https://github.com/decentraland-scenes/Block-dog)

[浏览场景](https://blockdog-wtciaozdbo.now.sh/?position=0%2C0)

#### 塔防游戏（WIP）

一个完全成熟的游戏，会生成一个随机的 2d 路径，敌人会从那边过来，路径中会随机放置陷阱。 当敌人沿着路径过来时，需要激活陷阱，以杀死他们。 这是一个有关时间的场景。

![](/images/media/example-tower-defense.png)

[代码](https://github.com/decentraland-scenes/Tower-defense)


#### Castaway 2048（WIP）

一款基于流行的游戏 2048 的完全成熟的游戏，由一系列增值的宝石代表成绩。 通过单击和拖放重新放置宝石将它们合并为更大的值，直到达到 2048。

![](/images/media/example-2048.png)

[代码](https://github.com/decentraland-scenes/Castaway-2048)
