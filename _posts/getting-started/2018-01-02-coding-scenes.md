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

## 开发工具

在高层级上，**Decentraland 软件开发工具包**（SDK）可以：

- 生成包含一个 Decentraland 场景的缺省 _项目_，包括用于渲染和运行场景内容所需的所有资源。
- 在本地 Web 浏览器中构建、测试和预览场景内容 - 完全脱机，无需进行任何以太坊交易或拥有 LAND。
- 使用 Decentraland API 编写 TypeScript 代码，为场景添加交互式和动态行为。
- 将场景内容上传到内容服务器。
- 将您的 LAND 通证链接到您上传的内容的 URL 地址上。

它包括以下组件：

<!--
- **The Decentraland Editor**: Use it to create and preview Decentraland scenes. You don't need to download any software to your machine, the editor runs entirely on your browser.
-->

- **Decentraland CLI**（命令行界面）：可以在您自己的机器上本地[生成]({{ site.baseurl }}{% post_url /getting-started/2018-01-02-coding-scenes %}) 新的 Decentraland 场景，预览它们并将它们上传到内容服务器。
- **Decentraland API**：一个 TypeScript 包，包含创建交互式体验的辅助方法库。可以用它创建和操作场景中的对象，并提供用户或其他应用程序之间的交易支持。

- **场景示例**：从[场景示例]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %})中获取灵感并编写最佳实践。

## CLI 必要条件

场景开发并不需要 LAND 通证。开发和测试场景可以完全脱机完成，无需将场景部署到以太坊网络（Decentraland 用于建立 LAND 的所有权的系统）或内容服务器。

早期版的 SDK 需要用户熟悉代码和终端的使用。须要有：

- **npm**（Node 包管理器）：终端中用于处理场景依赖。[下载链接](https://www.npmjs.com/)

- **源代码编辑器**：可以帮助您更快地创建场景并减少错误，编辑器可以提示语法错误、代码自动提示，甚至根据您所处的上下文向您显示智能建议。单击代码中的对象上能查看其类的完整定义以及它所支持的属性。我们建议使用 [Visual Studio Code](https://code.visualstudio.com/)或 [Atom](https://atom.io/)。

- **Decentraland CLI**：用于构建、预览和上传场景。 请参阅[安装指南]({{ site.baseurl }}{% post_url /getting-started/2018-01-01-installation-guide %})

## 支持的语言和语法

#### TypeScript (推荐)

我们使用 [TypeScript（.tsx）](https://www.typescriptlang.org/docs/handbook/jsx.html) 创造场景。

TypeScript 是 JavaScript 的一个超集，因此如果您熟悉 JavaScript，您会发现它们几乎相同，但是 TypeScript 允许您使用面向对象的编程和类型声明。自动完成和类型检查等功能可以加快开发时间，并能创建可靠的代码库。这些功能是优秀的开发体验的关键组成部分。

<!--
See [TypeScript tips]({{ site.baseurl }}{% post_url /development-guide/2018-01-08-typescript-tips %}) for best practices and recommendations for writing Decentraland scenes using TypeScript.
-->

#### XML

对于仅呈现静态内容且不具有交互性的场景，可以使用 [XML](https://en.wikipedia.org/wiki/XML)。学习和使用 XML 会比较容易。

我们鼓励开发人员使用 TypeScript 来构建他们的场景。 这会为场景带来更多有趣的可能。

#### 其他语言

只要编译后的脚本中包含一个名为 _game.js_ 的 JavaScript 文件，您就可以使用其他工具或语言（而不是 TypeScript）， 然后将其编译为 JavaScript。不过所有的类型声明都是在 TypeScript 中生成的，并且其他语言或转换器不受官方支持。

## 场景

部署到 LAND 的内容我们称为 **场景**。 场景是一个显示内容的交互式程序，它可以是游戏、互动体验、艺术画廊，或任何你想要的内容！

场景被部署到 Decentraland 中的虚拟土地上，LAND 是一个保存在以太坊智能合约中的稀缺和不可替代的资产。场景可以部署到单个**地块**，一个 10 米乘 10 米的虚拟土地 LAND，也可以部署到由多块相邻地块组成的**连块土地**上。

我们正在开发允许用户探索 Decentraland 的 Web 客户端。您上传到 LAND 的所有内容都可以通过此客户端呈现和查看。我们在 SDK 中包含了一个预览工具，同时您可以预览、测试或与您的内容进行交互。

## 实体和组件

Decentraland 中的三维场景基于 [实体-组件](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system) 模型，场景中的所有内容都是_entity 实体_，并且每个实体可以包括用来描述其特性和功能的 _components 组件_。

[DIAGRAM : ENTITY W COMPONENTS]

实体可以嵌套在其他实体中并形成树形结构。如果您熟悉 Web 开发，您可以将实体视为 DOM 树中的元素，组件则作为每个元素的属性。

请参阅[实体和组件]({{ site.baseurl }}{% post_url /development-guide/2018-02-1-entities-components %})，以进一步了解这些概念及如何在 Decentraland 场景中使用它们。

## 游戏主循环

[游戏循环](http://gameprogrammingpatterns.com/game-loop.html)是 Decentraland 场景代码的支柱。 它会定期执行代码进行以下操作：

- 取得用户输入
- 更新场景
- 重新渲染场景

在大多数传统软件程序中，所有事件都由用户操作直接触发。 在用户单击按钮，打开菜单等之前，程序状态中的任何内容都不会更改。

但是互动环境和游戏与此不同。 并非所有对场景的更改都必然是由用户的操作引起的。 你的场景可以有自己的动画对象，甚至是拥有 AI 的非玩家角色。 用户某些操作也可能需要多个帧才能完成，例如，如果打开一扇门需要一秒钟，则门移动时 rotation 旋转值必须逐步更新约 30 次。

我们将一次循环称为 _frame 帧_。 Decentraland 场景尽可能以每秒 30 帧的速度来渲染场景。 如果有一帧需要花费更多的时间进行渲染，那么能处理帧数就会减少。

在每个帧中，场景都会更新; 然后根据更新的值重新渲染场景。

在 Decentraland 场景中，没有明确声明的游戏循环，而是由[Systems]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %})上的 `update()` 函数构成游戏循环。

场景的编译和渲染是在后端执行的，开发场景时不需要处理这些内容。

## Systems

实体和组件存储有关场景中对象的信息。 用于更改组件中存储的信息的函数则保存于 _Systems_ 中。

_Systems_ 使静态场景动态化，使得事物随时间变化或响应用户交互。

每个 System 都有一个 `update()` 方法，它会在游戏循环中逐帧执行，请查看[_update pattern_](http://gameprogrammingpatterns.com/update-method.html)。

有关如何在场景中使用 systems 的更多详细信息，请参阅[Systems]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %})。

## 组件组

组件组（Component groups）跟踪场景中具有特定组件的所有实体。 创建组件组后，在增加或移除实体或组件时，它会自动更新这个列表以保持数据同步。

如果在每个帧上更新场景中的所有实体，有时可能产生很大的性能要求。 通过仅使用组件组中的实体，可以确保只处理相关的实体。

组件组可以由 system 中的函数引用。 通常，`update()` 函数将遍历组件组中的实体，对每个实体执行相同的操作。

## 组合使用

_engine_ 一方面位于 _实体（entities）_, _组件（components）_ 和 _组件组（component groups）_ 之间，另一方面位于 _systems_ 之间。 它用于调用系统的功能，在添加实体时更新组等等。

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

在上面的例子中，一个 `cube` 实体和一个 `RotatorSystem` 系统（system）被添加到引擎（engine）中。 `cube` 实体有一个 `Transform` 和一个 `BoxShape` 组件。 在游戏循环的每一帧中，会调用 `RotationSystem` 的 `update（）` 函数，改变 `cube` 实体的 `Transform` 组件中的旋转值。

[DIAGRAM]

请注意，加载场景时，上面的大多数代码只执行一次。 唯一的例外是系统的 `update（）` 方法，它会在游戏循环的每一帧中被调用。

## 场景隔离

您的场景不在与引擎相同的上下文中运行（即主线程）。 出于安全和性能原因，我们创建的 SDK 将渲染引擎与场景完全隔离。

因为这种隔离，您的场景代码无法访问 DOM 或 `window` 对象，因此您无法访问用户浏览器或地理位置等数据。

场景隔离通过 RPC 协议实现，该协议设定客户端的一小部分仅渲染场景和控制事件。

我们还抽象了通信协议。 这可以让我们在本地 WebWorker 中运行场景。

我们不希望开发人员介入引擎的内部，或需要知道引擎内部的内容。因为我们需要确保整个 Decentraland 地图中用户的一致体验，而错误往往更有可能发生在这个“低”级别上。