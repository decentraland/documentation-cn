---
date: 2018-01-06
title: Decentraland MVP 构建指南
description: 使用 SDK 制作您的第一个 MVP 场景或体验的建议指南
categories:
  - documentation
type: Document
set: building-scenes
set_order: 9
---


The purpose of this document is to help guide you through the process of building your initial experiences and environments in Decentraland. We’ll refer to these initial experiences and environments as the Minimum Viable Product (MVP).

本文档的目的是帮助和指导您在 Decentraland 中完成构建初始体验和环境的过程。我们将这些初始体验和环境称为最小可行产品（MVP）。

**When creating your MVP, you need to think about two areas of focus:**

**在创建 MVP 时，您需要考虑两个重点领域：**

1.  The basic user experience and functionality in your project

1. 项目中基本的用户体验和功能

2.  The creation of a basic "pipeline", or team workflow and content management system for building your experience and iteratively improving it.

2. 创建一个构建您的体验并迭代改进的基本“渠道”，如团队工作流程和内容管理系统。

An MVP should not try to demonstrate every possible outcome of every possible experience. Instead, an MVP should be the best first impression of your experience that you can make using Decentraland’s SDK.

在 MVP 中不要试图展示所有可能的体验效果，相反，MVP 应该是您使用 Decentraland 的 SDK 可以获得的最佳体验。

It is important to consider your own limitations, how you plan to provide content to your users, and the expectations of your users. Approaching your MVP in this way requires three different perspectives:

请务必考虑自己能力的局限性，你计划如何向用户提供内容，用户的期望是什么。需要从三个不同的视角来接近 MVP：

1.  As a developer or producer, how do I deliver an experience to my user/player?

1. 作为开发人员或制作人，我如何向用户/玩家提供体验？

2.  As a user or player, what do I expect from this experience?

2. 作为用户或玩家，我对此体验有何期望？

3.  As a contributor or stakeholder, how do I contribute to the pipeline or experience?

3. 作为贡献者或利益相关者，我如何为应用的筹备和体验做出贡献？

It is incredibly important to distinguish this approach from traditional agile development, because you may have to use non-optimum methods to meet your design goals.

将此方法与传统的敏捷开发区分开来非常重要，因为您可能不得不使用非最佳方法来满足您的设计目标。

You will have to examine your own goals in the context of your users’ expectations to decide if a certain release is focused more on the player, the pipeline and content contributors, or little of both.

您必须在用户期望的背景下检查自己的目标，以确定某个版本是更多地关注玩家，渠道和内容贡献者，或者两者中的一小部分。

When planning each release, it is critical that you conscientiously and deliberately set your priorities according to each of these three perspectives.

在规划每个版本时，您必须根据这三个视角中的每一个认真并刻意地设置优先级。

Separating these goals will allow you to provide your users with greater value in addition to optimizing your pipeline.

除了优化渠道之外，分离这些目标还可以为您的用户提供更大的价值。

You can expect your development backlog to follow two tracks:

您可以将您的待办开发事项分成两条轨道。

- The development of the tools and interfaces needed to build your delivery pipeline. (Or to optimize your existing pipeline for contributors as well as your development team.)

- 开发构建交付渠道所需的工具和接口。（或者为贡献者和开发团队优化现有渠道。）

- The backlog of user experiences you want to create.

- 要创建的用户体验的待办事项。

These two tracks will also follow two different approaches to testing:

这两个轨道也将遵循两种不同的测试方法：

- Testing your tools and pipeline interfaces will require more technical resources.

- 测试工具和渠道接口需要更多技术资源。

- Testing your user experiences is more akin to traditional user interface testing, and does not require the same scripting resources.

- 测试用户体验更类似于传统的用户界面测试，并且不需要相同的脚本资源。

The sooner you can get a value proposition in front of your user or player, the sooner you can get feedback to either confirm or reject that proposition. Confirming value quickly is critical. Many experienced developers will share stories of how they were certain beyond a shadow of a doubt of how amazing a new mechanic would be until they used it and it felt awkward and glitchy, the players didn’t respond to it at all, or it didn’t solve a consumer want/need. You want to fail quickly with as little effort as possible, so that you can learn from your failure and plan the next iteration.

越早获得您的用户或玩家有价值的建议，您就可以越早获得反馈，以确认或拒绝该提议。对建议有无价值的快速确认至关重要。许多经验丰富的开发人员都会分享这样的故事，原来认为一个新工具是多么神奇，直到真正使用它才感觉尴尬和异常，因为玩家根本没有回应，或者它没有解决消费者的需求。一定要在尽可能少的付出下快速失败，以便您可以从失败中吸取教训并计划下一次迭代。

So, how do you fail quickly? Very easily. You do the minimum needed to get your player to touch your product.

那么，如何快速失败？非常简单，只做最小需求，并尽量早地让玩家触摸到您的产品。

For example, let’s say that you’ve determined your players want to ride unicorns, so you spend months developing a pipeline to create blue unicorns and only blue unicorns. Then you give your blue unicorns to your players, only to find out that they despise blue unicorns and want only purple unicorns. You’ve wasted months of effort, and now you have to create a new pipeline to deliver your users the experience they want.

例如，假设你已经确定你的玩家想骑独角兽，所以你花了几个月的时间来开发了蓝色独角兽。然后把你的蓝色独角兽提供给玩家，却发现他们鄙视蓝色的独角兽，只想要紫色的独角兽。您浪费了数月的努力，现在您必须重新创建来为您的用户提供他们想要的体验。

However, if you gave the minimum viable blue unicorn to your players as quickly as possible, with a pipeline you could modify, then you would quickly learn that they want purple unicorns. Putting forward the minimum amount of effort needed to poll your users allows you meet their needs faster, without wasting effort and resources.

但是，如果你尽可能快地给你的玩家提供最小可行的蓝色独角兽，并且有修改的渠道，那么你很快就会知道他们想要紫色的独角兽。将用户投票所需的最少工作量前移，可以让您更快地满足他们的需求，而不会浪费精力和资源。

## Factors for Minimum Viable Products

## 最小可行产品考虑的因素

Here is the list of factors to consider for your basic MVP. It is absolutely acceptable to state that you will use X and will phase in Y. Planning your post-MVP product using a phased approach will help guarantee success.

以下是基本 MVP 需要考虑的因素列表。声明你将使用 X 并将分阶段进入 Y 是完全可以接受的。使用分阶段的方法来规划你的 MVP 产品将有助于保证成功。

1.  Art Creation

1. 艺术创作

- First, begin with basic still images

- 首先，从基本静止图像开始

- Your first test is for style: does the style you’ve chosen appeal to your users?

- 您的第一个测试是针对风格：您选择的风格是否会吸引您的用户？

- This could be the start of a style guide to provide to an outsourced artist

- 这可能是向外包艺术家提供风格指南的开始

2.  Scene Creation

2. 场景创作

- Develop a basic sense of your space

- 培养您的空间基本感

- Player should feel they are in a new, unique space

- 应该让玩家感觉到他们处在一个新的，独特的空间

- Delineate your space from neighboring spaces

- 从相邻空间勾画出你的空间

- Borders are evident and obvious – if only by a drawn line

- 边界是显而易见的 - 如果只是画线

- Cover entire area with static content/art

- 用静态内容/艺术覆盖整个区域

3. Art Rendered in Scene

3. 在场景中渲染的艺术品

- Using billboards is ok or other signage (this could simply be actual billboards or more sophisticated camera facing sprites)

- 使用广告牌（billboards）或其它标记是可以的（可以是简单现行的广告牌或更复杂的面向摄像头的精灵）

- Establish the tone and aesthetics of your space (i.e. style, bright, dark)

- 建立空间的色调和美感（即风格，明亮，黑暗）

- Note your process: how was art created and deployed into the scene?

- 注意你的过程：如何创作艺术并将其部署到场景中？

- How do you want to organize your art files for repeated deployment?

- 您希望如何组织您的艺术文件以进行重复部署？

4.  Player experience

4. 玩家体验

- Players are able to log in and visit your space/scene

- 玩家可以登录并访问您的空间/场景

- Players can distinguish your space from neighboring spaces

- 玩家可以将您的空间与相邻空间区分开来

5.  Pipeline Goals

5. 目标流程

- Deploy sample static scene: no interaction with player

- 部署示例静态场景：不与玩家交互

- Deploy interactive scene: including player engagement

- 部署交互式场景：包括玩家参与

- Deploy animated scene: not necessarily VR just demo movement

- 部署动画场景：不一定是 VR 可以只是演示动作

- Demonstrate deploy pipeline by re-deploying content: from art creation to in scene including scripting + QA]

- 通过重新部署内容展示部署流程：从艺术创作到场景，包括脚本+ QA]

- Expose pipeline gaps: identify the unknowns in specific content deployment areas

- 公开流程问题：标识特定内容部署区域中存在的未知数

## Levels of prototypes

## 原型级别

Failing quickly allows you to develop your experience by creating successive prototypes, with each iteration building upon the last.

快速失败允许您通过创建连续的原型来开发您的体验，每次迭代都在最后一次的基础上构建。

**Start with a single player prototype. Then you can plan for scripting multiplayer interactions. Finally, you can tackle your persistent core loop that demonstrates transactional layers.**

**从单人游戏原型开始。然后，您可以计划开发多人互动。最后，您可以处理演示事务层的核心循环（persistent core loop）。**

**What’s a persistent core loop?**

**什么是核心循环（persistent core loop）？**

In game design, a persistent core loop is the fundamental “game loop” that drives player actions and the game’s response to those actions. These persistent loops extend to any form of virtual experience (like those provided by Districts).

在游戏设计中，核心循环是驱动玩家行为和游戏对这些动作的响应的基本“游戏循环”。这些循环扩展到任何形式的虚拟体验（如小区提供的那些）。

> Note: The Decentraland client borrows some architectural ideas from [React.js](https://reactjs.org/) and only renders a scene when a change has taken place, not at a constant rate.

>注意：Decentraland客户端借用 [React.js](https://reactjs.org/) 中的一些架构构思，只在发生更改时显示场景，而不是以恒定速率来显示。

**What are transactional layers?**

**什么是事务层？**

The transactional layers are the interfaces between systems like an update to the blockchain or another application that has been interfaced with your experience to maintain a persistent record of player actions. Creating and maintaining this persistent record is what builds a personal and immersive experience.

事务层是系统之间的接口，例如更新区块链或另一个与您的体验相连接的应用程序，用于维护保存玩家动作记录。创建和维护这种记录是构建个人和身临其境的体验的基础。

We recommend creating your MVP as a single player experience. However, your MVP will ultimately be limited by the current capabilities of the SDK.

我们建议您将 MVP 创建为单人游戏体验。但是，您的 MVP 最终将受到 SDK 当前功能的限制。

For example, you could design a scene with the following successive experiences:

例如，您可以设计具有以下连续体验的场景：

- A single player can enter the world.

- 单个玩家可以进入这个世界。

- That player can navigate within the world, moving from point A to B.

- 该玩家可以在世界范围内导航，从 A 点移动到 B.

- The player can interact with one or two simple entities within the scene.

- 玩家可以与场景中的一个或两个简单实体进行交互。

- Another players can join and interact with the world and the other player.

- 其他玩家可以加入并与虚拟世界或其他玩家互动。

- Finally, you can add the ability to remember that each player entered the scene, and to track the players’ events and activities.

- 最后，您可以添加记住每个玩家进入场景、并跟踪玩家事件和活动的功能。

## Additional considerations

## 其他考虑因素

Once these basic use cases are covered, you can start to get more sophisticated with your release management strategy by focusing on mechanics. **Mechanics** are a broad term covering all of the actions a player can take and the responses the system will provide based on those player actions.

一旦涵盖了这些基本用例，通过关注机制，您可以开始更加成熟地使用发行版管理策略。**机制**是一个广义术语，涵盖了玩家可以采取的所有行动以及系统根据玩家行为提供的响应。

**Camera perspective** is another critical aspect of the user experience that will require specific and sequential goals. Since the ultimate vision of Decentraland is to build a VR world, the camera will eventually take a first person perspective. So, depending on the experience, this perspective will drive the individual experience of each player.

**摄像机视角**是用户体验的另一个重要方面，需要特定的和连续的目标。由于 Decentraland 的终极愿景是建立一个 VR世界，因此摄像机最终将采用第一人称视角。因此，根据经验，这种视角将推动每个球员的个人体验。

**Audio** is another critical aspect that may require its own prototype release. As with all major software releases, you may both number and name them for fun and for feedback engagement.

**音频**是另一个可能需要原型发布的关键方面。与所有主要软件版本一样，您可以为它们编号和命名，以获得乐趣和反馈。

Consider the MVP as one of many, many prototypes that you can use to establish your cadence for releases once you have established your pipeline. The focus of each release may vary, or it may be a hybrid of each aspect of the experience. However, you should aim to deliver successively more complicated experiences, each iteration building upon the last.

将 MVP 视为众多原型中的一种，一旦建立了你的流程，你就可以用它来建立你的发布节奏。每个版本的关注点可能会有所不同，或者它可能是各个方面的混合体验。但是，您应该致力于提供更复杂的体验，每次都建立在最后一次的基础上进行迭代。

1.  **MVP**: Single player

1. **MVP**：单人游戏

2.  **Release 2**: Add multiplayer and/or interaction support

2. **第 2 版**：添加多人游戏和/或互动支持

3.  **Release 3**: Introduce your first mechanic

3. **第 3 版**：介绍您的第一个游戏机制

4.  **Release 4**: Add audio support

4. **第 4 版**：添加音频支持

5.  **Release 5**: Finalize your art pipeline

5. **第 5 版**：完成您的艺术流程

For example, let’s say we are building an MVP for a Frisbee golf game. The MVP will include some still images of the course that can be seen by the player in the world. The player may even be able to throw a disc, in a very rudimentary, block-style fashion. This allows us to work out our basic throwing mechanics. The next release may include a prototype for multiplayer support so we can demo and test two users logged in and playing on our LAND at the same time.

例如，假设我们正在为飞碟高尔夫球比赛打造一个 MVP。 MVP 将包括一些世界各地的玩家可以看到的球场静止图像。玩家甚至可以以非常简陋的块式方式抛出光盘。这使我们能够计算出基本的投掷机制。下一个版本可能包含多人游戏支持原型，因此我们可以演示和测试两个用户同时登录并在我们的土地上玩。

Remember, while the end goal is a truly immersive 3D world, that is not where your MVP will start. Getting a player into your world as quickly as possible should be your first goal. Taking weeks, not months, to test your releases is critical to learning and iterating without wasting effort.

请记住，虽然最终目标是真正身临其境的 3D 世界，但这不是您的 MVP 将要开始的地方。让玩家尽快进入你的世界应该是你的第一个目标。花几周而不是几个月来测试你的版本对学习和迭代至关重要，而不会浪费精力。

Finally, we strongly recommend that you stay mindful of the first impression your experience presents. An empty experience will leave players disappointed. On the other hand, a district with some initial content and basic experiences shows players the potential for what is to come and encourages them to engage with your community and return to the next few releases.

最后，我们强烈建议您留意您的体验所带来的第一印象。空洞的体验会让玩家感到失望。另一方面，一个拥有一些初始内容和基本体验的小区向玩家展示了即将到来的游戏的潜力，并鼓励他们与你的社区互动，并断续回来玩接下来的发行版。

## A note on persistence and security

## 关于持久性和安全性的说明

Ultimately, you want reach a level of persistence where you can demonstrate that the transactional layers of your architecture are operational. Transactional is not limited to the players actions, but also the system’s reactions to players.

最终，您希望达到一定程度的持久性，让您可以在其中证明您的架构的事务层是可操作的。事务不仅限于玩家行为，还包括系统对玩家的反应。

If you plan to include a number of applications within your experience, then you may need to think about authentication at multiple layers. The interaction with these applications should be seamless for your players. They should only have to log in once, with your applications operating “behind the scenes” to ensure that their login info is passed on to your other downstream systems. This will require a robust and thoroughly tested security architecture.

如果您计划在您的体验中包含许多应用程序，那么您可能需要考虑多层身份验证。玩家对这些应用程序的交互应该是无感的。他们应该只需登录一次，您的应用程序在“幕后”运行，以确保他们的登录信息传递到您的其他下游系统。这将需要一个强大且经过全面测试的安全架构。

Given this complexity and stakes of security, please allocate the time and attention to these processes. Do not rush your security architecture to delivery.

鉴于这种复杂性和安全性，请分配给这些流程一定的时间和注意力。不要急于交付安全架构。

#### Persistence factors to consider

#### 要考虑的持久性因素

1.  **Account information**: login name, time zone, location for your specific experience/game

1. **帐户信息**：登录名，时区，特定体验/游戏的位置

2.  **Leaderboard stats**: previous game play results, global/regional standings, competitions

2. **排行榜统计数据**：之前的比赛结果，全球/区域排名，比赛

3.  **Identity validation**: Ethereum or BitCoin IDs, or any other backend identity management

3. **身份验证**：以太坊或比特币 ID，或任何其他后端身份管理

4.  **Blockchain updates**: as required based on your experience/game to update the blockchain ledger for transactional transparency

4. **区块链更新**：根据您的体验或游戏需要更新区块链分类帐以实现事务透明度

5.  **Runtime persistence**: temporary data for persistence across a potentially distributed platform (i.e. health for just the single game experience)

5. **运行时持久性**：跨越潜在的分布式平台的临时数据持久性存储（如仅针对单个游戏体验的健康状况）
