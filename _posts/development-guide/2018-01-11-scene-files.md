---
date: 2018-02-11
title: 场景文件
description: 场景缺省文件
categories:
  - development-guide
redirect_from:
  - /documentation/scene-files/
type: Document
set: development-guide
set_order: 4
---

在使用 CLI [创建新场景](https://docs.decentraland.org/#create-your-first-scene) 之后，场景文件夹将包含一系列具有缺省内容的文件。

## 本地场景中的文件

场景包括以下文件：

- **src/game.ts**: ：场景的入口。
- **scene.json**: 包含场景元数据的清单。
- **package.json** and **package-lock.json**: 指定场景所有依赖库的版本。
- **tsconfig.json**: Typescript 配置文件。
- **.dclignore**: 不用部署到 IPFS 上的文件列表.

#### game.ts

game.ts 是场景代码的入口点。您可以将整个场景的逻辑放入这个文件中，但为了清晰起见，在大多数情况下，我们建议将您的代码分散到几个_.ts_ 文件中，然后再将文件导入到 _game.ts_ 中。

在大多数情况下，您只需编辑此文件即可创建场景。它包含生成实体树的代码，即最终用户看到的您的地块内容。

以下是 _game.ts_ 文件的基本示例：

```ts
// Create a component group to track entities with Transform components
let group = engine.getComponentGroup(Transform)

// Create a system
export class RotatorSystem {
  // The update() function runs on every frame.
  update() {
    // Cycle over the entities in the component group
    for (let entity of group.entities) {
      const transform = entity.getComponent(Transform)
      transform.rotation.y += 2
    }
  }
}

// Create an entity
const cube = new Entity()

// Add a cube shape to the entity
cube.addComponent(new BoxShape())

// Add a transform component to the entity
cube.addComponent(new Transform({
  position: new Vector3(5, 0, 5)
}))

// Add the entity to the engine
engine.addEntity(cube)

// Add the system to the engine
engine.addSystem(new RotatorSystem())
```

#### scene.json

_scene.json_ 是虚拟土地上场景的 JSON 格式清单文件。场景可以跨越单个或多个 LAND 土地。 _scene.json_ 清单描述了场景中存在的对象，渲染场景时所需的材质列表，地块所有者的联系信息以及安全设置。_scene.json_ 文件的更多信息，请访问 [Decentraland 规范建议](https://github.com/decentraland/proposals/blob/master/dsp/0020.mediawiki) 。

所有这些数据对于场景本地预览是可选的，但是部署时却需要使用其中的一部分。您可以随时手动更改此信息。

#### package.json

此文件向 NPM 提供信息，使其能够识别项目，以及处理项目的依赖项。 Decentraland 场景需要两个包：

- **decentraland-api**，使场景与虚拟世界引擎进行通信。
- **typescript**，将文件 _game.ts_ 编译为 javascript。

#### package-lock.json

此文件列出了项目的所有其他依赖项的版本。这些版本是固定的，这意味着编译器将使用与此处列出的相同的版本。

您可以通过编辑此文件手动更改版本。

#### tsconfig.json

包含 _tsconfig.json_ 文件的目录是 TypeScript 项目的根目录。 _tsconfig.json_ 文件指定了编译成 JavaScript 项目所需的根文件和选项。

> 只要将脚本包含在单个 Javascript 文件（scene.js）中，您也可以使用其他工具或语言而不是 TypeScript 。但是提供的所有类型声明都是由 TypeScript 生成的，其他语言和转换器不受官方支持。

## 推荐的文件位置

请记住，将场景部署到 Decentraland 时，场景所需的任何资源或外部库都必须打包在场景文件夹中，或者通过远程服务器提供。

任何要在用户的客户端中运行的东西都必须位于场景文件夹中。不能引用安装在本地计算机上的其它文件或库，因为部署的场景不能访问它们。

我们建议始终使用这些文件夹名称来存储场景可能需要的不同类型的资源：

- 3d模型：`/ models`
- 视频：`/videos`
- 声音文件：`/sounds`
- 纹理的图像文件（除 glTF 模型外）：`/materials`
- _.ts_ 组件定义 `/src/components`
- _.ts_ systems 定义 `/src/systems`

> 注: glTF 模型文件的支持文件，如纹理图像文件或 _.bin_ 文件，应该始终与模型的放在同一个文件夹中。

## dclignore 文件

所有场景都包含一个 _.dclignore_ 文件，此文件指定在将场景部署到 Decentraland 时要忽略的场景文件夹中哪些文件。

例如，您可能希望在场景文件夹中保存场景中 3D 模型的 Blender 文件，但是您不希望将这些文件部署到 Decentraland。在这种情况下，您可以将 `*.blend` 添加到 _.dclignore_ 以忽略具有该扩展名的所有文件。

| 需要忽略的 | 示例         | 说明                                                  |
| ---------- | ------------ | ----------------------------------------------------- |
| 具体文件   | `BACKUP.ts` | 忽略特定文件                                          |
| 目录       | `drafts/`    | 忽略文件夹及其子文件夹的全部内容                      |
| 扩展名     | `*.blend`    | 忽略具有给定扩展名的所有文件                          |
| 文件名    | `test*`      | 忽略所有匹配的文件。 这里是以 _test_ 文件名开头的文件 |
