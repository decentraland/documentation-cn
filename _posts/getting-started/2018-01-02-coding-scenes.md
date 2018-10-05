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

- 生成 Decentraland 场景的默认 _项目_，包括渲染和运行内容所需的所有资源。
- 在本地 Web 浏览器中构建、测试和预览场景内容 - 完全脱机，无需进行任何以太坊交易或拥有 LAND。
- 使用 Decentraland API 编写 TypeScript 代码，为场景添加交互式和动态行为。
- 将场景内容上传到 [IPFS](https://ipfs.io)。
- 将您的 LAND 通证链接到您上传的内容的 IPFS URL。

它包括以下组件：

- **Decentraland CLI**（命令行界面）：可以在您自己的机器上本地生成新的 Decentraland 场景，预览它们并将它们上传到IPFS。
- **Decentraland API**（以前称为 _Metaverse API_，通常也称为 _API_）：一个 TypeScript 包，其中包含可帮助您创建交互式体验的辅助方法库。可以用它创建和操作场景中的对象，并提供用户或其他应用程序之间的交易支持。

- **场景示例**：从[场景示例]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %})中获取灵感并编写最佳实践。

## 必要条件

要在本地开发场景，不需要拥有 LAND 通证。开发和测试场景可以完全脱机完成，无需将场景部署到以太坊网络（Decentraland 用于建立 LAND 的所有权的系统）或 IPFS（我们用于分发和传送内容的 P2P 网络）。

此 SDK 旨在供熟悉代码和终端使用的用户使用。您须要有：

- **npm**（Node 包管理器）：终端中用于处理场景依赖。[下载链接](https://www.npmjs.com/)

- **源代码编辑器**：可以帮助您更快地创建场景并减少错误，因为它能标记出语法错误、并在您编写时自动提示，甚至根据您所处的上下文向您显示智能建议。单击代码中的对象上能查看其类的完整定义以及它所支持的属性。我们建议使用 [Visual Studio Code](https://code.visualstudio.com/)或 [Atom](https://atom.io/)。

- **Decentraland CLI**：用于构建、预览和上传场景。 请参阅[安装指南]({{ site.baseurl }}{% post_url /getting-started/2018-01-01-installation-guide %})

## 支持的语言和语法

#### TypeScript (推荐)

我们使用 [TypeScript（.tsx）](https://www.typescriptlang.org/docs/handbook/jsx.html) 创造场景。

TypeScript 是 JavaScript 的超集，因此如果您熟悉 JavaScript，您会发现它们几乎相同，但是 TypeScript 允许您使用面向对象的编程和类型声明。自动完成和类型检查等功能可以加快开发时间，并能创建可靠的代码库。这些功能都是积极开发者体验的关键组成部分。

在 Typescript 场景中，您将在场景中呈现的实体作为嵌入的 XML 资源进行处理。这与 [React](https://reactjs.org/docs/hello-world.html) 框架非常相似。这是场景的主文件 _scene.tsx_ 使用 _.tsx_ 而不是 _.ts_ 的原因。

> **注意：** 尽管场景的编写方式有点像 React，**我们的 SDK 并没有使用 React**。

有关使用 TypeScript 编写 Decentraland 场景的最佳做法和建议，请参阅[TypeScript 开发技巧]({{ site.baseurl }}{% post_url /development-guide/2018-01-08-typescript-tips %})。

#### XML

对于仅呈现静态内容且不具有交互性的场景，可以使用 [XML](https://en.wikipedia.org/wiki/XML)。

对于静态场景，在使用 CLI 构建时，请选择 _Static_ 选项。

我们鼓励开发人员使用 TypeScript 构建他们的场景。 TypeScipt 场景包括了处理实体渲染的嵌入式 XML。 如果忽略 _Basic_ 场景中的所有 Typescript代 码并且只编辑 `render()` 函数中的内容，那么你基本上是在处理 XML 只是语法略有不同的内容。

#### 其他语言

只要编译后的脚本包含名为 _scene.js_ 的单个 JavaScript 文件，您就可以使用其他工具或语言而不是 TypeScript 将其编译为JavaScript。不过所有的类型声明都是在 TypeScript 中生成的，并且其他语言或转换器不受官方支持。

## 场景

部署到 LAND 的内容我们称为 **场景**。 场景是一个显示内容的交互式程序，它可以是游戏、互动体验、艺术画廊，或任何你想要的东东！

场景被部署到 **地块**，一个 10 米乘 10 米的虚拟土地 LAND，LAND 是一个保存在以太智能合约中的稀缺和不可替代的资产。 这些虚拟空间的土地是您使用 SDK 创建和上传的内容跟用户交互的地方。

我们正在开发允许用户探索 Decentraland 的 Web 客户端。您上传到 LAND 的所有内容都可以通过此客户端呈现和查看。我们在 SDK 中包含了一个预览工具，同时您可以预览、测试或与您的内容进行交互。

## 实体和组件

实体是构建 Decentraland 场景中的所有内容的基本单元，可以将它们视为 Web 开发中 DOM 树中的元素。

Decentraland 中的三维场景基于 [Entity-Component](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system) 模型，场景中的所有内容都是_entity 实体_，并且每个实体可以包括 _components 组件_，用来描述其特性和功能。

实体是场景中包含的，用户可以通过 Web 浏览器进行渲染、查看和交互的所有资源。包括 3D 对象和音频文件。

组件定义实体的特征。 例如，您可以在实体上包含 `color` 组件以设置其颜色，或者包含 `lookAt` 组件将其旋转，使其面向空间中的某个点。

> 注意：引用此模型时使用的术语 _component 组件_ 与它在 _React_ 生态系统中的使用方式有很大不同，后者所有内容都被视为_component_。 在整个文档中，我们在使用实体-组件模型时使用术语 _component 组件_。

实体可以将其他实体作为子实体，这些实体从父实体继承组件。 如果父实体被定位、缩放或旋转，其子项也会受到影响。不可见实体也可以
作为将多个实体包装成一个组的容器。多亏了这一点，我们可以使用树形结构管理实体。

有关其他术语、定义和说明，请参阅我们的[完整词汇表]({{ site.baseurl }}{% post_url /general/2018-01-03-glossary %})。

请参阅[实体接口]({{ site.baseurl }}{% post_url /development-guide/2018-06-21-entity-interfaces %})查看实体的所有可用构造函数以及对应支持的组件。

## Render 函数

All [scene objects]({{ site.baseurl }}{% post_url /development-guide/2018-01-05-scriptable-scene %}) have a 'render()` method that outputs what users of your scene will see on their browser. This function must always output a hierarchical tree of entities that starts at the root level with a _scene_ entity.

所有[场景对象]({{ site.baseurl }}{% post_url /development-guide/2018-01-05-scriptable-scene %}) 都有一个'render()`方法，用于输出场景的用户在他们的浏览器上看到的内容。此函数必须始终返回从根级别开始的分层实体树，其中包含 _scene_ 实体。

{% raw %}

```tsx
 async render() {
    return (
      <scene position={{ x: 5, y: 0, z: 5 }}>
        <box
          position={{ x: 2, y: 1, z: 0 }}
          scale={{ x: 2, y: 2, z: 0.05 }}
          color="#0000FF"
        />
        <entity
          rotation={this.state.doorRotation}
          transition={{ rotation: { duration: 1000, timing: 'ease-in' } }}
        >
          <box
            id="door"
            scale={{ x: 1, y: 2, z: 0.05 }}
            position={{ x: 0.5, y: 1, z: 0 }}
            color="#00FF00"
          />
        </entity>
      </scene>
    )
  }
```

{% endraw %}

如果您具有使用其他工具开发游戏的经验，可能会认为场景具有某种渲染循环，用来定期渲染屏幕中的元素。但是 Decentraland 并不这样工作的。我们的 API 是基于_事件_构建的，因此 `render()` 函数是通过响应事件从而更新场景，而不是通过反复查询。

场景存在[状态]({{ site.baseurl }}{% post_url /development-guide/2018-01-04-scene-state %})，这是一组随时间变化并代表场景的当前设置的变量。状态通过场景中发生的 [事件]({{ site.baseurl }}{% post_url /development-guide/2018-01-03-event-handling %}) 而发生变化。当状态变化时，将使用状态的新值重新触发场景渲染。

这是受 [React](https://reactjs.org/) 框架的启发，其中大部分关于 React 的内容也适用于 decentraland 场景。

## 场景的解耦

您的场景不会在与引擎相同的上下文（也就是主线程）中运行，它们甚至可能不会与引擎在同一台计算机上运行。我们以完全与渲染引擎分离的方式创建 SDK。我们的这个设计出于安全和性能原因。

场景的解耦通过 RPC 协议工作，该协议指定客户端的一小部分仅渲染场景和控制事件。

我们还抽象了通信协议。 这可以让我们在本地 WebWorker 中运行场景，或通过 WebSockets 在 Node.js 服务器中远程运行场景。

我们不希望开发人员介入引擎的内部，或需要知道引擎内部的内容。并且错误更有可能发生在这个“低”级别上，我们需要确保整个 Decentraland 地图中用户的一致体验。

#### 将场景与引擎的解耦

我们来看一个例子吧，假设您要渲染以下内容的场景：

{% raw %}

```tsx
<scene>
  <gltf-model src="models/a.gltf" />
  <sphere position={{ x: 10, y: 10, z: 10 }} />
</scene>
```

{% endraw %}

在编写场景代码时，您无需实际加载 `a.gltf` 模型，也无需知道球体实体使用的几何索引。您需要做的是在更高级别描述场景，就像在 XML 中一样。

一旦 `render()` 函数将此场景发送到引擎，引擎会处理好位置、资源和几何问题。

为了优化性能，我们只将场景中的更改发送给实际的客户端，而不是所有内容。所以，如果场景中有一群鱼，只有其中一个移动，API 将只发送该变化量到客户端，即移动的那条鱼。这使得客户端的速度更快，并且对场景开发人员完全透明。

## 从 React 获取灵感

我们设计 SDK 的目标之一是尽可能减少学习曲线。我们还希望激励良好实践和编写可维护代码，在各种情况下遵守远程 async-rpc 约束。

总而言之，为此我们考虑采用两种方法：

- **jQuery 方式**：告诉系统如何处理实体，创建、转换并尝试达到所需状态。
- **React 方式**：告诉系统所需的状态，由它来解决如何达到目的。

####  _jQuery_ 方式

如果我们选择了 jQuery 方式（实际上我们没有选择这种方式），创建示例场景所需的代码如下所示：

{% raw %}

```ts
// WARNING: This code sample is only hypothetical,
// This is not valid syntax
// and is not supported by our tools

let scene = metaverse.createScene()
let objModel = metaverse.createObjModel()
let sphere = metaverse.createSphere()

objModel.setAttribute("src", "models/a.gltf")
objModel.appendTo(scene)

sphere.setAttribute("position", { x: 10, y: 10, z: 10 })
sphere.appendTo(scene)

EntityController.render(scene)
```

{% endraw %}

在这个例子中，我们告诉系统如何达到所需的状态，这个例子为达到所需的状态使用（滥用）了可变和具有副作用的变量和函数。

#### _React_ 方式

我们选择了 [React](https://reactjs.org/) 方式，所以创建上面相同场景的代码如下所示：

{% raw %}

```tsx
// IMPORTANT: This code is only an example, it does not exist nor work
 async render() {
   return (
    <scene>
      <obj-model src="models/a.gltf" />
      <sphere position={{ x: 10, y: 10, z: 10 }} />
    </scene>
  )
}
```

{% endraw %}

在这个例子中，我们只是告诉系统所需的状态，而不是描述进入该状态的所有逻辑。

我们按照 React 方式设计 SDK，原因如下：

 - 它利用了过去10年中发生的网络技术的发展。
 - 它更容易理解，它删除了大量与场景的业务逻辑无关的公式化代码。
 - 这是声明性的。描述**你想要什么**，而不是**你想要怎么样**。
 - 它有助于已经熟悉 React 的开发人员。
 - 这种模式众所周知，并且有很好的文档，比较容易获得帮助。
 - 内存占用少，可以轻松进行垃圾回收。