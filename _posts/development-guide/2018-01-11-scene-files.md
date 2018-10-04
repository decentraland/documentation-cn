---
date: 2018-01-10
title: 场景文件
description: 新建场景生成的缺省文件
redirect_from:
  - /documentation/scene-files/
categories:
  - development-guide
type: Document
set: development-guide
set_order: 11
---

在使用 CLI [创建新场景]({{ site.baseurl }}{% post_url /getting-started/2018-01-03-create-scene %})之后，场景文件夹将包含一系列具有缺省内容的文件。文件结构和内容将根据您选择的场景类型(_basic_, _interactive_, _remote_, or _static_)而有所不同。

## 本地场景中的文件

当您选择使用 CLI 创建 _basic_ 或 _interactive_ 场景时，就会创建一个 _local scene_。

*本地场景*包括以下文件：

1. `scene.tsx`：场景的入口点。
2. `scene.json`：包含场景元数据的清单。
3. **package.json** 和 **package-lock.json**: 指定场景所有依赖库的版本。
4. `build.json`：包含构建场景的指令的文件。
5. `tsconfig.json`：Typescript 配置文件。

#### scene.tsx

在大多数情况下，您只需编辑此文件即可创建场景。它包含生成实体树的代码，即最终用户看到的您的地块内容。

以下是 _scene.tsx_ 文件的基本示例：

{% raw %}

```tsx
import { ScriptableScene, createElement } from "decentraland-api"

// The ScriptableScene class is a React-style component.
export default class MyScene extends ScriptableScene<any, any> {
  async render() {
    return (
      <scene>
        <box position={{ x: 5, y: 0, z: 5 }} scale={{ x: 1, y: 1, z: 1 }} />
      </scene>
    )
  }
}
```

{% endraw %}


> **重要说明：** 您的 `scene.tsx` 必须始终包含 `export default class`，这样我们的 SDK 才能找到相应的初始化场景类。


#### scene.json

_scene.json_ 是虚拟土地上场景的 JSON 格式清单文件。场景可以跨越单个或多个 LAND 土地。 _scene.json_ 清单描述了场景中存在的对象，渲染场景时所需的材质列表，地块所有者的联系信息以及安全设置。_scene.json_ 文件的更多信息，请访问 [Decentraland 规范建议](https://github.com/decentraland/proposals/blob/master/dsp/0020.mediawiki) 。

当您运行 `dcl init` 命令时，它会提示您输入一些数据，这些数据存储在场景的 scene.json 文件中。 这些数据都是可选的，用于在本地预览场景，但部署时需要部分数据。您可以随时手动更改此信息。

#### package.json

此文件向 NPM 提供信息，使其能够识别项目，以及处理项目的依赖项。 Decentraland 场景需要两个包：

- **decentraland-api**，使场景与虚拟世界引擎进行通信。
- **typescript**，将文件 _scene.tsx_ 编译为 javascript。

#### package-lock.json

此文件列出了项目的所有其他依赖项的版本。这些版本是固定的，这意味着编译器将使用与此处列出的相同的版本。

您可以通过编辑此文件手动更改版本。

#### build.json

这是 Decentraland 构建配置文件。

#### tsconfig.json

包含 _tsconfig.json_ 文件的目录是 TypeScript 项目的根目录。 _tsconfig.json_ 文件指定了编译成 JavaScript 项目所需的根文件和选项。

> 只要将脚本包含在单个 Javascript 文件（scene.js）中，您也可以使用其他工具或语言而不是 TypeScript 。但是提供的所有类型声明都是由 TypeScript 生成的，其他语言和转换器不受官方支持。

## 远程场景的缺省文件

远程场景包含以下文件:

1.  **server/RemoteScene.tsx**: 场景入口.
2.  **server/State.ts**: 处理场景状态。会在远程服务器中运行。
3.  **server/Server.ts**: 远程服务器的配置。
4.  **server/ConnectedClients.ts**: 处理场景的用户。
5.  **server/build.json**: 场景构建说明。
6.  **server/tsconfig.json**: Typescript 配置文件。
7.  **server/build.json**: 该文件包含构建场景的说明。
8.  **package.json** and **package-lock.json**: 指定场景的所有依赖项的版本。
9.  **scene.json**: 场景配置数据。

#### RemoteScene.tsx

包含生成实体树的代码，即最终用户将看到的您的土地内容。它与静态场景的 _scene.tsx_ 文件的最大区别在于它没有定义场景状态，而是从 _State.ts_ 导入状态和访问它的函数。

以下是 _RemoteScene.tsx_ 文件的基本示例：

{% raw %}

```tsx
import * as DCL from "decentraland-api"
import { setState, getState } from "./State"

// The ScriptableScene class is a React-style component.
export default class MyScene extends ScriptableScene<any, any> {
  async render() {
    return (
      <scene>
        <box position={{ x: 5, y: 0, z: 5 }} scale={{ x: 1, y: 1, z: 1 }} />
      </scene>
    )
  }
}
```

{% endraw %}

> **重要说明：** 您的 _RemoteScene.tsx_ 文件必须始终包含 `export default class`，这样我们的 SDK 才能找到相应的初始化场景类。

#### State.ts

此文件处理场景状态。远程场景状态保存在远程服务器中。

该文件包括场景状态的定义和两个用于从此远程状态获取和设置信息的函数。

以下是 _State.ts_ 文件的基本示例：

{% raw %}

```tsx
import { updateAll } from "./ConnectedClients"

let state = {
  isDoorClosed: false
}

export function getState(): typeof state {
  return state
}

export function setState(deltaState: Partial<typeof state>) {
  state = { ...state, ...deltaState }
  console.log("new state:")
  console.dir(state)
  updateAll()
}
```

{% endraw %}

Learn more about how the scene state works in [scene state]({{ site.baseurl }}{% post_url /development-guide/2018-01-04-scene-state %}).

详细了解[场景状态]({{ site.baseurl }}{% post_url /development-guide/2018-01-04-scene-state %})的工作原理。

#### scene.json

_scene.json_ 是虚拟土地上场景的 JSON 格式文件。场景可以跨越单个或多个 LAND 土地。 _scene.json_ 清单描述了场景中存在的对象，渲染场景时所需的材质列表，地块所有者的联系信息以及安全设置。_scene.json_ 文件的更多信息，请访问 [Decentraland 规范建议](https://github.com/decentraland/proposals/blob/master/dsp/0020.mediawiki) 。

当您运行 `dcl init` 命令时，它会提示您输入一些数据，这些数据存储在场景的 scene.json 文件中。 这些数据都是可选的，用于在本地预览场景，但部署时需要部分数据。您可以随时手动更改此信息。

#### package.json

此文件向 NPM 提供信息，使其能够识别项目，以及处理项目的依赖项。 Decentraland 场景需要两个包：

- **decentraland-api**，使场景与虚拟世界引擎进行通信。
- **typescript**，将文件 _scene.tsx_ 编译为 javascript。

#### package-lock.json

此文件列出了项目的所有其他依赖项的版本。这些版本是固定的，这意味着编译器将使用与此处列出的相同的版本。

您可以通过编辑此文件手动更改版本。

#### build.json

这是 Decentraland 构建配置文件。

#### tsconfig.json

包含 _tsconfig.json_ 文件的目录是 TypeScript 项目的根目录。 _tsconfig.json_ 文件指定了编译成 JavaScript 项目所需的根文件和选项。

> 只要将脚本包含在单个 Javascript 文件（scene.js）中，您也可以使用其他工具或语言而不是 TypeScript 。但是提供的所有类型声明都是由 TypeScript 生成的，其他语言和转换器不受官方支持。

## 静态场景中的缺省文件

_静态场景_ 包括以下文件:

1.  **scene.json**: 场景配置数据。
2.  **scene.xml**: 静态场景的内容。

#### scene.xml (静态场景)

For both static and dynamic scenes, the end result is the same: a tree of entities. The root of the tree is always a `<scene>` element. XML scenes call out this structure explicitly, TypeScript scenes provide the script to build and update this structure.

对于静态和动态场景，最终结果都是：实体树。树的根始终是一个 `<scene>` 元素。 XML 场景显式调用此结构， Type Script 场景提供用于构建和更新此结构的脚本。

```xml
<scene>
  <sphere position="1 1 1"></sphere>
  <box position="3.789 2.3 4.065" scale="1 10 1"></box>
  <box position="2.212 7.141 4.089" scale="2.5 0.2 1"></box>
  <gltf-model src="crate/crate.gltf" position="5 1 5"></box>
</scene>
```

由于根 scene 元素是个转换节点，因此它也可以被平移、缩放和旋转。这些功能是非常有用的，例如，更改整个地块的坐标中心：

```xml
<scene position="5 5 5">
  <box position="0 0 0"></box>
  <!-- in this example, the box is located at the world position 5 5 5 -->
</scene>
```

#### package.json

此文件向 NPM 提供信息，使其能够识别项目，以及处理项目的依赖项。 Decentraland 场景需要两个包：

- **decentraland-api**，使场景与虚拟世界引擎进行通信。

## 推荐的文件位置

请记住，将场景部署到 Decentraland 时，场景所需的任何资源或外部库都必须打包在场景文件夹中，或者通过远程服务器提供。

任何要在用户的客户端中运行的东西都必须位于场景文件夹中。包括像 Babylon.js 这样的外部库。您不应该引用本地计算机在其他位置安装的库，因为对部署的场景它们不可用。

我们建议始终使用这些文件夹名称来存储场景可能需要的不同类型的资源：

- 3d模型：`/ models`
- 视频：`/videos`
- 声音文件：`/sounds`
- 纹理的图像文件：`/materials`
- _.tsx_ 可重用组件 `/ components`


##  dclignore 文件

所有场景都包含一个 _.dclignore_ 文件，此文件指定在将场景部署到 Decentraland 时要忽略的场景文件夹中哪些文件。

例如，您可能希望在场景文件夹中保存场景中 3D 模型的 Blender 文件，但是您不希望将这些文件部署到 Decentraland。在这种情况下，您可以将 `*.blend` 添加到 _.dclignore_ 以忽略具有该扩展名的所有文件。


| 需要忽略的 | 示例         | 说明                                                  |
| ---------- | ------------ | ----------------------------------------------------- |
| 具体文件   | `BACKUP.tsx` | 忽略特定文件                                          |
| 目录       | `drafts/`    | 忽略文件夹及其子文件夹的全部内容                      |
| 扩展名     | `*.blend`    | 忽略具有给定扩展名的所有文件                          |
| 文件名    | `test*`      | 忽略所有匹配的文件。 这里是以 _test_ 文件名开头的文件 |