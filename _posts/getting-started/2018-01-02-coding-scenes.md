---
date: 2018-01-01
title: 场景开发
description: 帮助您了解 decentraland 客户端和 SDK 的工作方式。
redirect_from:
  - /documentation/introduction/
  - /docs/sdk-overview/
  - /docs/command-line-interface/
  - /docs/sdk-quick-start-guide/
  - /sdk-reference/introduction/
categories:
  - getting-started
type: Document
set: getting-started
set_order: 4
---

## The development tools
## 开发工具

At a very high level, the **Decentraland Software Development Kit** (SDK) allows you to do the following:

在高层级上，**Decentraland 软件开发工具包**（SDK）可以：

- Generate a default _project_ containing a Decentraland scene, including all the assets needed to render and run your content.
- Build, test, and preview the content of your scene locally in your web browser - completely offline, and without having to make any Ethereum transactions or own LAND.
- Write TypeScript code using the Decentraland API to add interactive and dynamic behavior to the scene.
- Upload the content of your scene to the content server.
- Link your LAND tokens to the URL of the content you have uploaded.

- 生成包含一个 Decentraland 场景的缺省 _项目_，包括用于渲染和运行场景内容所需的所有资源。
- 在本地 Web 浏览器中构建、测试和预览场景内容 - 完全脱机，无需进行任何以太坊交易或拥有 LAND。
- 使用 Decentraland API 编写 TypeScript 代码，为场景添加交互式和动态行为。
- 将场景内容上传到内容服务器。
- 将您的 LAND 通证链接到您上传的内容的 URL 地址上。

Our SDK includes the following components:
它包括以下组件：

<!--
- **The Decentraland Editor**: Use it to create and preview Decentraland scenes. You don't need to download any software to your machine, the editor runs entirely on your browser.
-->

- **The Decentraland CLI** (Command Line Interface): Use it to [generate]({{ site.baseurl }}{% post_url /getting-started/2018-01-02-coding-scenes %}) new Decentraland scenes locally on your own machine, preview them and upload them to the content server.
- **The Decentraland API**: A TypeScript package containing the library of helper methods that allows you to create interactive experiences. Use it to create and manipulate objects in the scene and also to facilitate in-world transactions between users or other applications.

- **Decentraland CLI**（命令行界面）：可以在您自己的机器上本地[生成]({{ site.baseurl }}{% post_url /getting-started/2018-01-02-coding-scenes %}) 新的 Decentraland 场景，预览它们并将它们上传到内容服务器。
- **Decentraland API**：一个 TypeScript 包，包含创建交互式体验的辅助方法库。可以用它创建和操作场景中的对象，并提供用户或其他应用程序之间的交易支持。

- **Scene examples**: Take inspiration and coding best practices from the [sample scenes]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %}).
- **场景示例**：从[场景示例]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %})中获取灵感并编写最佳实践。

## CLI Requirements
## CLI 必要条件

To develop a scene locally, you don't need to own LAND tokens. Developing and testing a scene can be done completely offline, without the need to deploy a scene to the Ethereum network (the system Decentraland uses to establish ownership of LAND), or the content server.

场景开发并不需要 LAND 通证。开发和测试场景可以完全脱机完成，无需将场景部署到以太坊网络（Decentraland 用于建立 LAND 的所有权的系统）或内容服务器。

These early releases of the SDK are intended for users that are comfortable working with code and a terminal. You must have the following:

早期版的 SDK 需要用户熟悉代码和终端的使用。须要有：

- **npm** (Node package manager): Used in the terminal to handle scene dependencies. [Download link](https://www.npmjs.com/)

- **A source code editor**: Helps you create scenes a lot faster and with less errors. A source code editor marks syntax errors, autocompletes while you write and even shows you smart suggestions that depend on the context that you're in. You can also click on an object in the code to see the full definition of its class and what attributes it supports. We recommend [Visual Studio Code](https://code.visualstudio.com/) or [Atom](https://atom.io/).

- **The Decentraland CLI**: Used to build, preview and upload scenes. See [Installation guide]({{ site.baseurl }}{% post_url /getting-started/2018-01-01-installation-guide %})

- **npm**（Node 包管理器）：终端中用于处理场景依赖。[下载链接](https://www.npmjs.com/)

- **源代码编辑器**：可以帮助您更快地创建场景并减少错误，编辑器可以提示语法错误、代码自动提示，甚至根据您所处的上下文向您显示智能建议。单击代码中的对象上能查看其类的完整定义以及它所支持的属性。我们建议使用 [Visual Studio Code](https://code.visualstudio.com/)或 [Atom](https://atom.io/)。

- **Decentraland CLI**：用于构建、预览和上传场景。 请参阅[安装指南]({{ site.baseurl }}{% post_url /getting-started/2018-01-01-installation-guide %})


## Supported languages and syntax

#### TypeScript (recommended)

## 支持的语言和语法

#### TypeScript (推荐)

We use [TypeScript (.tsx)](https://www.typescriptlang.org/docs/handbook/jsx.html)
to create our scenes.

TypeScript is a superset of JavaScript, so if you're familiar with JavaScript you'll find it's almost the same, but TypeScript allows you to employ object-oriented programming and type declarations. Features like autocomplete and type-checking speed up development times and allow for the creation of a more solid codebase. These features are all key components to a positive developer experience.

我们使用 [TypeScript（.tsx）](https://www.typescriptlang.org/docs/handbook/jsx.html) 创造场景。

TypeScript 是 JavaScript 的一个超集，因此如果您熟悉 JavaScript，您会发现它们几乎相同，但是 TypeScript 允许您使用面向对象的编程和类型声明。自动完成和类型检查等功能可以加快开发时间，并能创建可靠的代码库。这些功能是优秀的开发体验的关键组成部分。

<!--
See [TypeScript tips]({{ site.baseurl }}{% post_url /development-guide/2018-01-08-typescript-tips %}) for best practices and recommendations for writing Decentraland scenes using TypeScript.
-->

#### XML

For scenes that only render motionless content and that won't be interactive, you can use [XML](https://en.wikipedia.org/wiki/XML). XML takes a lot less effort to learn and use.

We encourage developers to build their scenes using TypeScript, as that allows for far more interesting possibilities.

对于仅呈现静态内容且不具有交互性的场景，可以使用 [XML](https://en.wikipedia.org/wiki/XML)。学习和使用 XML 会比较容易。

我们鼓励开发人员使用 TypeScript 来构建他们的场景。 这会为场景带来更多有趣的可能。

#### Other languages

You can use another tool or language instead of TypeScript and compile it into JavaScript, as long as your compiled scripts are contained within a single JavaScript file named _game.js_. All provided type declarations are made in TypeScript, and other languages and transpilers are not officially supported.

#### 其他语言

只要编译后的脚本中包含一个名为 _game.js_ 的 JavaScript 文件，您就可以使用其他工具或语言（而不是 TypeScript）， 然后将其编译为 JavaScript。不过所有的类型声明都是在 TypeScript 中生成的，并且其他语言或转换器不受官方支持。

## Scenes

The content you deploy to your LAND is called a **scene**. A scene is an interactive program that renders content, this could be a game, an interactive experience, an art gallery, whatever you want!

Scenes are deployed to virtual LAND in Decentraland. LAND is a scarce and non-fungible asset maintained in an Ethereum smart contract. Deploy to a single **parcel**, a 10 meter by 10 meter piece of LAND, or to an **estate**, comprised of multiple adjacent parcels.

We are developing the web client that will allow users to explore Decentraland. All of the content you upload to your LAND will be rendered and viewable through this client. We have included a preview tool in the SDK so that you can preview, test, and interact with your content in the meantime.

## 场景

部署到 LAND 的内容我们称为 **场景**。 场景是一个显示内容的交互式程序，它可以是游戏、互动体验、艺术画廊，或任何你想要的内容！

场景被部署到 Decentraland 中的虚拟土地上，LAND 是一个保存在以太坊智能合约中的稀缺和不可替代的资产。场景可以部署到单个**地块**，一个 10 米乘 10 米的虚拟土地 LAND，也可以部署到由多块相邻地块组成的**连块土地**上。

我们正在开发允许用户探索 Decentraland 的 Web 客户端。您上传到 LAND 的所有内容都可以通过此客户端呈现和查看。我们在 SDK 中包含了一个预览工具，同时您可以预览、测试或与您的内容进行交互。

## Entities and Components
## 实体和组件

Three dimensional scenes in Decentraland are based on an [Entity-Component-System](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system) architecture, where everything in a scene is an _entity_, and each entity can include _components_ that determine its characteristics.

Decentraland 中的三维场景基于 [实体-组件](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system) 模型，场景中的所有内容都是_entity 实体_，并且每个实体可以包括用来描述其特性和功能的 _components 组件_。

[DIAGRAM : ENTITY W COMPONENTS]

Entities are nested inside other entities to form a tree structure. If you're familiar with web development, you might find it useful to think of entities as elements in a DOM tree and of components as the attributes of each of these elements.

实体可以嵌套在其他实体中并形成树形结构。如果您熟悉 Web 开发，您可以将实体视为 DOM 树中的元素，组件则作为每个元素的属性。

See [Entities and components]({{ site.baseurl }}{% post_url /development-guide/2018-02-1-entities-components %}) for an in-depth look of both these concepts and how they're used by Decentraland scenes.

请参阅[实体和组件]({{ site.baseurl }}{% post_url /development-guide/2018-02-1-entities-components %})，以进一步了解这些概念及如何在 Decentraland 场景中使用它们。

## The game loop
## 游戏主循环

The [game loop](http://gameprogrammingpatterns.com/game-loop.html) is the backbone of a Decentraland scene's code. It cycles through part of the code at a regular interval and does the following:

[游戏循环](http://gameprogrammingpatterns.com/game-loop.html)是 Decentraland 场景代码的支柱。 它会定期执行代码进行以下操作：

- Listen for user input
- Update the scene
- Re-render the scene

- 取得用户输入
- 更新场景
- 重新渲染场景

In most traditional software programs, all events are triggered directly by user actions. Nothing in the program's state will change until the user clicks on a button, opens a menu, etc.

在大多数传统软件程序中，所有事件都由用户操作直接触发。 在用户单击按钮，打开菜单等之前，程序状态中的任何内容都不会更改。

But interactive environments and games are different from that. Not all changes to the scene are necessarily caused by a user's actions. Your scene could have animated objects that move on their own or even non-player characters that have their own AI. Some user actions might also take multiple frames to be completed, for example if the opening of a door needs to take a whole second, the door's rotation must be incrementally updated about 30 times as it moves.

但是互动环境和游戏与此不同。 并非所有对场景的更改都必然是由用户的操作引起的。 你的场景可以有自己的动画对象，甚至是拥有 AI 的非玩家角色。 用户某些操作也可能需要多个帧才能完成，例如，如果打开一扇门需要一秒钟，则门移动时 rotation 旋转值必须逐步更新约 30 次。

We call each iteration over the loop a _frame_. Decentraland scenes are rendered at 30 frames per second whenever possible. If a frame takes more time than that to be rendered, then less frames will be processed.

In each frame, the scene is updated; then the scene is re-rendered, based on the updated values.

我们将一次循环称为 _frame 帧_。 Decentraland 场景尽可能以每秒 30 帧的速度来渲染场景。 如果有一帧需要花费更多的时间进行渲染，那么能处理帧数就会减少。

在每个帧中，场景都会更新; 然后根据更新的值重新渲染场景。

In Decentraland scenes, there is no explicitly declared game loop, but rather the `update()` functions on the [Systems]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %}) of the scene make up the game loop.

The compiling and rendering of the scene is carried out in the backend, you don't need to handle that while developing your scene.

在 Decentraland 场景中，没有明确声明的游戏循环，而是由[Systems]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %})上的 `update()` 函数构成游戏循环。

场景的编译和渲染是在后端执行的，开发场景时不需要处理这些内容。

## Systems

Entities and components are places to store information about the objects in a scene. _Systems_ hold functions that change the information that's stored in components.

_Systems_ are what make a static scene dynamic, allowing things to change over time or in response to user interaction.

实体和组件存储有关场景中对象的信息。 用于更改组件中存储的信息的函数则保存于 _Systems_ 中。

_Systems_ 使静态场景动态化，使得事物随时间变化或响应用户交互。

Each System has an `update()` method that's executed on every frame of the game loop, following the [_update pattern_](http://gameprogrammingpatterns.com/update-method.html).

See [Systems]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %}) for more details about how systems are used in a scene.

每个 System 都有一个 `update()` 方法，它会在游戏循环中逐帧执行，请查看[_update pattern_](http://gameprogrammingpatterns.com/update-method.html)。

有关如何在场景中使用 systems 的更多详细信息，请参阅[Systems]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %})。

## Component groups
## 组件组

Component groups keep track of all entities in the scene that have certain components in them. Once a component group is created, it automatically keeps its list up to date with each new entity or component that is added or removed.

组件组（Component groups）跟踪场景中具有特定组件的所有实体。 创建组件组后，在增加或移除实体或组件时，它会自动更新这个列表以保持数据同步。

If you attempt to update all the entities in the scene on every frame, that can sometimes have a significant cost in performance. By referring only to the entities in a component group, you ensure you're only dealing with those that are relevant.

如果在每个帧上更新场景中的所有实体，有时可能产生很大的性能要求。 通过仅使用组件组中的实体，可以确保只处理相关的实体。

Component groups can be referenced by the functions in a system. Typically an `update()` function will loop over the entities in the component group, performing the same actions on each.

组件组可以由 system 中的函数引用。 通常，`update()` 函数将遍历组件组中的实体，对每个实体执行相同的操作。

## Putting it all together
## 组合使用

The _engine_ is what sits in between _entities_, _components_ and _component groups_ on one hand and _systems_ on the other. It calls system's functions, updates groups when entities are added, etc.

_engine_ 一方面位于 _实体（entities）_, _组件（components）_ 和 _组件组（component groups）_ 之间，另一方面位于 _systems_ 之间。 它用于调用系统的功能，在添加实体时更新组等等。

All of the values stored in the components in the scene represent the scene's state at that point in time. With every frame of the game loop, the engine runs the `update()` function of each of the systems to update the values stored in the components.

After all the systems run, the components on each entity will have new values. When the engine renders the scene, it will use these new updated values and users will see the entities change to match their new states.

场景中存储在组件中的所有数据表示该时间点的场景状态。对于游戏循环的每一帧，引擎运行每个系统（systems）的 `update（）`函数来更新存储在组件中的值。

所有系统运行后，每个实体上的组件将具有新的值。 当引擎渲染场景时，它会使用这些新的更新值进行渲染，用户就能看到实体基于其新状态的变化。

```ts
// Create a group to track all entities with a Transform component
const myGroup = engine.getComponentGroup(Transform)

// Define a System
export class RotatorSystem implements ISystem {
  // The update function runs on every frame of the game loop
  update() {
    // The function iterates over all the entities in myGroup
    for (let entity of myGroup.entities) {
      const transform = entity.get(Transform)
      transform.rotate(Vector3.Left(), 0.1)
    }
  }
}

// Add the system to the engine
engine.addSystem(new RotatorSystem())

// Create an entity
const cube = new Entity()

// Give the entity a transform component
cube.set(new Transform({
    position: new Vector3(5, 1, 5)
  }))

// Give the entity a box shape
cube.set(new BoxShape())

// Add the entity to the engine
engine.addEntity(cube)
```

In the example above, a `cube` entity and a `RotatorSystem` system are added to the engine. The `cube` entity has a `Transform`, and a `BoxShape` component. In every frame of the game loop, the `update()` function of `RotationSystem` is called, and it changes the rotation values in the `Transform` component of the `cube` entity.

在上面的例子中，一个 `cube` 实体和一个 `RotatorSystem` 系统（system）被添加到引擎（engine）中。 `cube` 实体有一个 `Transform` 和一个 `BoxShape` 组件。 在游戏循环的每一帧中，会调用 `RotationSystem` 的 `update（）` 函数，改变 `cube` 实体的 `Transform` 组件中的旋转值。

[DIAGRAM]

Note that most of the code above is executed just once, when loading the scene. The exception is the `update()` method of the system, which is called on every frame of the game loop.

请注意，加载场景时，上面的大多数代码只执行一次。 唯一的例外是系统的 `update（）` 方法，它会在游戏循环的每一帧中被调用。

## Scene Decoupling
## 场景隔离

Your scenes don't run in the same context as the engine (a.k.a. the main thread). We created the SDK in a way that is entirely decoupled from the rendering engine. We designed it to be like this for both safety and performance reasons.

您的场景不在与引擎相同的上下文中运行（即主线程）。 出于安全和性能原因，我们创建的 SDK 将渲染引擎与场景完全隔离。

Because of this decoupling, your scene's code doesn't have access to the DOM or the `window` object, so you can't access data like the user's browser or geographical location.

The decoupling works by using RPC protocol, this protocol assigns a small part of the client to only render the scene and control events.

因为这种隔离，您的场景代码无法访问 DOM 或 `window` 对象，因此您无法访问用户浏览器或地理位置等数据。

场景隔离通过 RPC 协议实现，该协议设定客户端的一小部分仅渲染场景和控制事件。

We have also abstracted the communication protocol. This allows us to run the scenes locally in a WebWorker.

We don't want developers to intervene with the internals of the engine or even need to know what lies inside the engine. We need to ensure a consistent experience for users throughout the Decentraland map, and mistakes are more likely to happen at that "low" level.

我们还抽象了通信协议。 这可以让我们在本地 WebWorker 中运行场景。

我们不希望开发人员介入引擎的内部，或需要知道引擎内部的内容。因为我们需要确保整个 Decentraland 地图中用户的一致体验，而错误往往更有可能发生在这个“低”级别上。