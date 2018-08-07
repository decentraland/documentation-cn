---
date: 2018-01-01
title: 简介
description: 帮助您了解客户端和 decentraland SDK 的工作方式
categories:
  - documentation
type: Document
set: building-scenes
set_order: 1
---

In this document, we introduce the concepts behind how the decentralized metaverse works.

在本文档中，我们将介绍去中心化虚拟世界的背后是如何工作的。

The content you deploy to your LAND is called a **scene**. It's usually an interactive program that renders content, this could be a game, a video screen, an art gallery, whatever you want!

部署到 LAND 上的内容被称为 **场景**。 通常是一个呈现内容的互动程序，可以是游戏、视频、艺术画廊等等任何你想要的内容!

If you have game development experience with other tools, you might expect to find some kind of render loop that periodically renders elements in the screen. Decentraland doesn't work like that, we run your scene in a different context from the engine, for safety and performance reasons. Also, we don't want developers to intervene with the internals of the engine or even need to know what lies inside the engine. We need to ensure a consistent experience for users throughout the Decentraland map, and mistakes are more like to happen at that "low" level.

如果你有使用其他工具进行游戏开发的经验，您可能希望找到某个在屏幕周期性显示元素的渲染循环。Decentraland 并不是这样，出于安全和性能原因，我们在于引擎不同的上下文中运行您的场景。此外，我们不希望开发人员介入引擎的内部，也不希望开发人员甚至需要知道引擎内部的内容。我们要为所有 Decentraland 地图中的用户提供一致的体验，因为在“低”级别上更有可能发生错误。


Another key difference with event-loop-based games is that we built the API based on _events_, so we expect scenes to be updated in reaction to events rather than by querying the world repeatedly.

与基于事件循环游戏的另一个关键区别是我们基于 _events_ 构建了 API，因此场景能够根据事件进行更新而不是通过反复查询来更新。

## How do scenes run?
## 场景如何运行？

Decoupling. It's all about decoupling. Your scenes don't run in the same context as the engine
(a.k.a. the main thread), they might even not run in the same computer as the engine. We created the SDK in a way that is
entirely decoupled from the rendering engine. It works using RPC protocol, this protocol assigns a small part of the client to only render the scene and control events.

解耦，一切都与解耦有关。 您的场景跟引擎不在相同的上下文中运行
（也就是主线程），它们甚至可能不会与引擎在同一台计算机上运行。 我们将渲染引擎完全分离开来，以此方式来创建 SDK。 它使用 RPC 协议工作，此协议仅指定客户端的一小部分来渲染场景和控制事件。

We have also abstracted the communication protocol. This allows us to run the scenes both locally in a WebWorker and remotely in a Node.js server thru WebSockets.

我们还抽象了通信协议。这不仅让我们能在 WebWorker 中本地运行场景，还可以通过 WebSockets 在 Node.js 服务器中远程运行场景。

#### Decoupling a scene from the engine

#### 解耦场景与引擎

Let's take a look at an example. Suppose you want to render a scene with the following content:

我们来看一个例子吧。 假设您要渲染具有以下内容的场景：

{% raw %}

```tsx
<scene>
  <gltf-model src="models/a.gltf" />
  <sphere position={{ x: 10, y: 10, z: 10 }} />
</scene>
```

{% endraw %}

While writing your scene's code, you have no need to actually load the `a.gltf` model, and you don't need to know the geometry indexes used by the sphere entity. All you need to do is describe the scene at a higher level, like you do in XML.

在编写场景代码时，您不需要去实际加载 `a.gltf` 模型，也无需知道球这个实体使用的几何索引。 您需要做的就是在更高级别上描述场景，就像在 XML 中一样。

Once you send the scene to the engine, it takes care of the positions, assets and geometries.

一旦将场景发送到引擎，它会自行计算处理相关的位置、资源和几何形状等。

To optimize things, we only send the differences in the scene to the actual client. So if the scene has a shoal of
fish and only one of them moves, the SDK will send only that delta to the client, the fish that moved. This makes things faster for the client and is completely transparent to the developer of the scene.

出于性能优化，我们只将场景中的变化的内容发送给实际的客户端。比如，如果场景有一群鱼，但只有其中一条在游动，SDK 只会向客户端发送移动的鱼这个变量。这将提高客户端的处理的速度，并且场景开发人员对此也非常容易理解。

## Entities and Components
## 实体和组件

Entities are the basic unit for building everything in Decentraland scenes, think of them as the equivalent of Elements in a DOM tree in web development. Three dimensional scenes in Decentraland are based on the [Entity-Component](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system) model, where everything in a scene is an _entity_, and each entity can include _components_ that shape its characteristics and functionality.

在 Decentraland 场景中，实体是构建所有内容的基本单元，可以把它们视为 Web 开发中 DOM 树中的元素。 Decentraland 中的三维场景都是基于[实体和组件](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system) 模型，场景中的所有内容都是_实体_，每个实体都可以包含形成其特征和功能的_组件_。

For example, you can include the `color` component on an entity to set its color, or include the `ignoreCollision` component to change how it reponds to collisions with other entities.

例如，您可以在实体上包含 `color` 组件以设置颜色，或者包含 `ignoreCollision` 组件以更改它与其他实体碰撞时的响应方式。

An entity can have other entities as children, these inherit the components from the parent. If a parent entity is positioned, scaled or rotated, its children are also affected. Thanks to this, we can arrange entities into trees.

实体还可以将其他实体作为它的子实体，这些实体将从父实体继承组件。如果父实体被定位、缩放或旋转，其子实体也会受到影响。这样，我们可以使用实体“树”来管理实体。

See [Entity interfaces]({{ site.baseurl }}{% post_url /sdk-reference/2018-06-21-entity-interfaces %}) for a more information on entities, a reference of all the available constructors for predefined entities and how to define your own types.

有关实体的更多信息，请参阅[实体接口]({{ site.baseurl }}{% post_url /sdk-reference/2018-06-21-entity-interfaces %})，这是一个有关预定义实体的所有可用构造函数以及如何定义自己的类型的参考。

## Supported languages and syntax
## 支持的语言和语法

- To create scenes with only static content, use [XML](https://en.wikipedia.org/wiki/XML).
- To create dynamic scenes, you must use [TypeScript (.tsx)](https://www.typescriptlang.org/docs/handbook/jsx.html).
- In Typescript scenes, XML assets are handled in a way that resembles [React](https://reactjs.org/docs/hello-world.html).

- 仅使用静态内容来创建场景，请使用[XML](https://en.wikipedia.org/wiki/XML)。
- 创建动态场景，必须使用[TypeScript (.tsx)](https://www.typescriptlang.org/docs/handbook/jsx.html)。
- 在 Typescript 场景中，使用类似于[React](https://reactjs.org/docs/hello-world.html)来处理 XML 资源。

> **Note:** Even though dynamic scenes are written in a way that intentionally looks a lot like React, **our SDK DOES NOT USE REACT**.

> **注意：** 虽然动态场景的代码被有意设计成类似 React，**但我们的 SDK 并不使用 REACT**

#### Programming principles
####　设计原理

**Why do we use Typescript?**
**为什么使用 Typescript?**

TypeScript is a superset of JavaScript and allows you to employ object-oriented programming and type declaration. Features like autocomplete and type-checking speed up development times and allow for the creation of a solid codebase. These features are all key components to a positive developer experience.

Type Script 是 JavaScript 的一个超集，在这里你可以使用面向对象编程和类型声明。像自动完成和类型检查这样的特性能够加速开发，并有助于构建坚实的代码库。所有这些特性都是决定开发人员体验的关键组成部分。

> You can use another tool or language instead of TypeScript, so long as your scripts are contained within a single Javascript file (scene.js). All provided type declarations are made in TypeScript, and other languages and transpilers are not officially supported.

> 除了 TypeScript 外，您也可以使用其他工具或语言，只要您把脚本逻辑放在一个 Javascript 文件（scene.js）中即可。 所有提供的类型声明都是由 TypeScript 生成的，其他语言和转换器不受官方支持。

**Taking Inspiration from React**
**从 React 中获取的灵感**

One of the goals we had when designing our SDK was to reduce the learning curve as much as possible. We also wanted to incentive good practices and the writing of maintainable code, respecting the remote async-rpc constraints in every case.

我们设计 SDK 时的目标之一就是尽可能减少学习曲线。 我们还希望通过它来鼓励好的实践和编写可维护代码，遵守 async-rpc 远程调用的限制。

To make a long story short, we were considering two approaches for doing this:
长话短说，我们考虑两种方法：

- **the jQuery way**: tell the system how to handle entities, create, mutate and try to reach a desired state.
- **the React way**: tell the system the desired state and let it figure out how to get there.

- **jQuery 方式**：告诉系统如何处理实体的创建和变化来尝试达到所需状态。
- **React 方式**：只告诉系统所需的状态，并由它自行处理。

---

If we had chosen the jQuery way (which we didn't), the code we would have needed to create our example scene would look like this:

如果我们选择了 jQuery 方式（实际上我们没有），那么我们创建示例场景所需的代码将如如下所示：

```ts
// WARNING: 此代码示例仅是一个假设示例，我们的工具并不支持

let scene = metaverse.createScene();
let objModel = metaverse.createObjModel();
let sphere = metaverse.createSphere();

objModel.setAttribute("src", "models/a.gltf");
objModel.appendTo(scene);

sphere.setAttribute("position", { x: 10, y: 10, z: 10 });
sphere.appendTo(scene);

EntityController.render(scene);
```

In this example, we're telling the system how to reach a desired state, the example (ab)uses mutations and
side effects of how the code works to reach the desired state.

在这个例子中，我们告诉系统如何达到所需的状态，这个例子使用了可变的和具有副作用（side effects）的代码来达到所需状态。

---

Thankfully, we chose to do things the React way, so our code for creating the same scene as above looks like this:

值得庆幸的是，我们选择了 React 方式，因此我们创建上述相同场景的代码如下所示：

```tsx
// IMPORTANT: 这段代码只是一个例子，它有可能无效

const scene = (
  <scene>
    <obj-model src="models/a.gltf" />
    <sphere position={{ x: 10, y: 10, z: 10 }} />
  </scene>
);

EntityController.render(scene);
```

In this example, we're just telling the system the desired state, instead of describing all of the logic to get to that state.

在这个例子中，我们只是告诉系统我们需要的状态，而不用描述进入该状态的所需要的处理逻辑。

---

We went for the React approach, for several reasons:
采用类 React 方法，原因如下：

- It takes advantage of the evolution of web technologies that occured in the past 10 years.
- It's simpler to understand, it removes tons of boilerplate code that's not related to the business logic of the scene.
- It's declarative. You describe **what** you want, not **how** you want that to happen.
- It helps onboard developers that are already familiar with React.
- The pattern is well known and well documented, getting help should be easy.
- It has a low memory footprint and it makes it easy to do garbage collection.

- 它利用了过去 10 年网络技术的发展。
- 更容易理解，它删除了大量与场景的业务逻辑无关的公式化代码。
- 这是声明性的。 你描述**你想要什么**，而不是**你要怎样做**。
- 它有助于已经熟悉 React 的开发人员。
- 这种模式众所周知，并且有很好的文档，容易取得帮助。
- 内存占用少，可以轻松进行垃圾回收。