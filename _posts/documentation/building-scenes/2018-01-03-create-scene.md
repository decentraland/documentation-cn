---
date: 2018-01-01
title: 创建您的第一个场景
description: 了解 Decentraland 场景的基础知识
categories:
  - documentation
type: Document
set: building-scenes
set_order: 2
tag: introduction
---

在 Decentraland，场景是地产或 LAND 的内容的表现。 所有场景都由[实体]({{ site.baseurl }}{% post_url /sdk-reference/2018-06-21-entity-interfaces %})组成，它们代表场景中的所有元素并构成树形结构，非常类似于 Web 开发中 DOM 树中的元素。

## 开始前

请确保首先安装 CLI 工具。在 Mac OS 中，您可以通过运行以下命令来执行此操作：

```bash
npm install -g decentraland
```

针对 Windows 和 Linux 系统的更多详细信息和特定说明，请参阅[安装指南]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-01-installation-guide %})。

## 创建文件结构

可以使用我们的 CLI 工具来自动创建初始的场景模板。为此，请在空文件目录中运行 `dcl init`。有关如何安装和使用 CLI 的详细信息，请参阅[SDK概述]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-01-SDK-Overview %})。

`dcl init` 命令在当前工作目录中创建包含一个 **场景** 的 Decentraland **项目**。
它提示你回答一系列可选问题，如关于场景所有权的以及Decentraland 最终上传的位置，然后它要求你选择一个场景模板。根据您选择的选项，CLI 构建具有不同默认内容的不同文件结构。

有四种不同的场景模板可供使用：

- **基本场景**：在一个简单的 TypeScript 文件中定义的，用来显示单一的 glTF 模型。
- **本地场景**：在一个 TypeScript 文件中定义，该示例以一个可以打开的门为例。这是开始学习如何使用 SDK 的最佳模板。场景具有基本状态并处理单击事件。场景状态被本地存储在用户的浏览器中，因此用户的行为不会影响其他用户的场景呈现。
- **远程场景**：在用于本地场景的示例文件中定义的 TypeScript 文件，但其不同之处在于场景状态存储在使用 WebSockets 通信的远程服务器中。正因为如此，所有的用户都看到场景呈现一致。如果你正在开发一种游戏或者另一种互动体验，这很可能是你希望的工作方式。为了测试场景，可以同时在本地运行服务器和客户端。
- **静态场景**：单独一个 glTF 模型定义在 **XML** 文件中。不能将任何动态或交互式内容添加到这类场景中，它只能显示静态实体。

## 场景内容

*本地场景*包括以下文件：

1. `scene.tsx`：场景的入口点。
2. `scene.json`：包含场景元数据的清单。
3. `package.json`:
4. `build.json`：包含构建场景的指令的文件。
5. `tsconfig.json`：Typescript 配置文件。

`dcl init` 命令还会提示您输入一些描述性元数据，这些数据存储在 [scene.json](https://github.com/decentraland/proposals/blob/master/dsp/0020.mediawiki) 
清单文件中。除场景类型外，所有这些用于在本地构建场景的元数据都是可选的。

> 如果在其他 Decentraland 项目的文件目录中运行 `dcl init`，则任何具有重复名称的现有文件都将被新的初始化项目文件覆盖。

#### scene.tsx

此文件包含生成实体树的代码，这是您的地块最终用户将看到的内容。下面是 `scene.tsx` 文件的基本示例：

{% raw %}
```tsx
import { ScriptableScene, createElement } from "metaverse-api";

// The ScriptableScene class is a React-style component.
export default class MyScene extends ScriptableScene<any, any> {
  async render() {
    return (
      <scene>
        <box position={{ x: 5, y: 0, z: 5 }} scale={{ x: 1, y: 1, z: 1 }} />
      </scene>
    );
  }
}
```
{% endraw %}

> **重要说明：** 您的 `scene.tsx` 必须始终包含 `export default class`，这样我们的 SDK 才能找到相应的初始化场景的类。


#### scene.json

`scene.json` 是虚拟土地上场景的 JSON 格式清单文件。场景可以跨越单个或多个 LAND 土地。 `scene.json` 清单描述了场景中存在的对象，渲染场景时所需的材质列表，地块所有者的联系信息以及安全设置。`scene.json` 文件的更多信息，请访问 [Decentraland 规范建议](https://github.com/decentraland/proposals/blob/master/dsp/0020.mediawiki) 。

#### package.json

此文件向 NPM 提供信息，使其能够识别项目，以及处理项目的依赖项。 Decentraland 场景需要两个包：

* `metaverse-api`, which allows the scene to communicate with the world engine.
* `typescript`, to compile the file `scene.tsx` to javascript.

* `metaverse-api`，它允许场景与虚拟世界引擎进行通信。
* `typescript`，将文件 `scene.tsx` 编译为 javascript。

> 创建静态场景时不需要 `typescript` 包。它只有在构建远程和交互式场景时才需要。

#### build.json

这是 Decentraland 构建配置文件。

我们提供了一个名为 `metaverse-compiler` 的工具，它来自于 `metaverse-api` 包。这个工具负责读取 `build.json` 文件并以客户端可以运行的方式编译场景。它唯一真正做的是使用 WebPack 将 Typescript 代码捆绑到 WebWorker 中。

> 您还可以使用 CLI 为多人游戏体验创建 Node.js 服务器。

#### tsconfig.json

包含 `tsconfig.json` 文件的目录是 TypeScript 项目的根目录。 `tsconfig.json` 文件指定了编译成 JavaScript 项目所需的根文件和选项。

> 只要脚本包含在单个 Javascript 文件（scene.js）中，您也可以使用其他工具或语言而不是 TypeScript 。但是提供的所有类型声明都是由 TypeScript 生成的，其他语言和转换器不受官方支持。

## 静态场景内容

*静态场景*包括以下文件：

1. `scene.json`：包含场景元数据的清单。
2. `scene.xml`：静态场景的内容。

#### scene.xml（静态场景）

对于静态和动态场景，最终结果是相同的：实体树。树的根始终是一个 `<scene>` 元素。 XML 场景显式调用此结构， Type Script 场景提供用于构建和更新此结构的脚本。


```xml
<scene>
  <sphere position="1 1 1"></sphere>
  <box position="3.789 2.3 4.065" scale="1 10 1"></box>
  <box position="2.212 7.141 4.089" scale="2.5 0.2 1"></box>
  <gitf-model src="crate/crate.gitf" position="5 1 5"></box>
</scene>
```

由于根 scene 元素是个转换节点，因此它也可以被平移、缩放和旋转。这些功能是非常有用的，例如，更改整个地块的坐标中心：

```xml
<scene position="5 5 5">
  <box position="0 0 0"></box>
  <!-- in this example, the box is located at the world position 5 5 5 -->
</scene>
```

## 预览你的场景

要在本地预览渲染的场景，请在场景的主文件夹上运行以下命令：

```bash
dcl start
```

每次对场景进行更改时，预览都会自动重新加载和更新，因此无需再次运行该命令。

有关在场景预览中可以看到的内容以及如何运行远程场景预览的说明，请参阅[场景预览]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-04-preview-scene %})。

## 编辑你的场景

要编辑场景，我们建议使用源代码编辑器，如 [Visual Studio Code](https://code.visualstudio.com/) 或 [Atom](https://atom.io/)。像这样的编辑器可以帮助您更快地创建场景并减少错误，因为它会标记出语法错误，输入时自动完成，甚至向您提示依赖于上下文的智能建议。使用 Visual Studio Code，您甚至可以通过单击对象以查看其类的完整定义。

有关向场景添加内容的简单说明，请参阅[场景内容指南]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %}) 。

完成创建场景，想要将其上传到 LAND ，请参阅[发布]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-07-publishing %})。
