---
date: 2018-01-10
title: 场景中的文件
description: 新建场景生成的缺省文件
redirect_from:
  - /documentation/scene-files/
categories:
  - development-guide
type: Document
set: development-guide
set_order: 11
---

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
import { ScriptableScene, createElement } from "decentraland-api";

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

* `decentraland-api`, which allows the scene to communicate with the world engine.
* `typescript`, to compile the file `scene.tsx` to javascript.

* `decentraland-api`，它允许场景与虚拟世界引擎进行通信。
* `typescript`，将文件 `scene.tsx` 编译为 javascript。

> 创建静态场景时不需要 `typescript` 包。它只有在构建远程和交互式场景时才需要。

#### build.json

这是 Decentraland 构建配置文件。

我们提供了一个名为 `metaverse-compiler` 的工具，它来自于 `decentraland-api` 包。这个工具负责读取 `build.json` 文件并以客户端可以运行的方式编译场景。它唯一真正做的是使用 WebPack 将 Typescript 代码捆绑到 WebWorker 中。

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


After [creating a new scene]({{ site.baseurl }}{% post_url /getting-started/2018-01-03-create-scene %}) using the CLI, the scene folder will have a series of files with default content. The file structure and content will vary depending on the type of scene you selected (_basic_, _interactive_, _remote_, or _static_).

## Default files in a local scene

When you choose to create a _basic_ or an _interactive_ scene with the CLI, this creates a _local scene_.

Local scenes inclde the following files:

1.  **scene.tsx**: The entry point of the scene.
2.  **scene.json**: The manifest that contains metadata for the scene.
3.  **package.json** and **package-lock.json**: Specify the versions of all dependencies of the scene.
4.  **build.json**: The file with the instructions to build the scene.
5.  **tsconfig.json**: Typescript configuration file.

#### scene.tsx

In most cases, you'll only need to edit this file to create your scene. It contains the code that generates an entity tree, which is what end users of your parcel will see.

Below is a basic example of a _scene.tsx_ file:

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

> **Important note:** Your _scene.tsx_ file must always include an `export default class`, that's how our SDK finds the class to initialize the scene.

#### scene.json

The _scene.json_ file is a JSON formatted manifest for a scene in the world. A scene can span a single or multiple LAND parcels. The _scene.json_ manifest describes what objects exist in the scene, a list of any assets needed to render it, contact information for the parcel owner, and security settings. For more information and an example of a
_scene.json_ file, please visit the [Decentraland specification proposal](https://github.com/decentraland/proposals/blob/master/dsp/0020.mediawiki).

When you run the `dcl init` command, it prompts you to enter some descriptive metadata, these datais are stored in
the scene.json manifest file for the scene. All of this
metadata is optional for previewing the scene locally, but part of it is needed for deploying. You can change this information manually at any time.

#### package.json

This file provides information to NPM that allows it to identify the project, as well as handle the project's dependencies. Decentraland scenes need two packages:

- **decentraland-api**: allows the scene to communicate with the world engine.
- **typescript**: used to compile the file _scene.tsx_ to javascript.

#### package-lock.json

This file lists the versions of all the other dependencies of the project. These versions are locked, meaning that the compiler will use literally the same minor release listed here.

You can change any package version manually by editing this file.

#### build.json

This is the Decentraland build configuration file.

#### tsconfig.json

Directories containing a _tsconfig.json_ file are root directories for TypeScript Projects. The _tsconfig.json_ file specifies the root files and options required to compile your project from TypeScript into JavaScript.

> You can use another tool or language instead of TypeScript, so long as your scripts are contained within a single Javascript file (scene.js). All provided type declarations are made in TypeScript, and other languages and transpilers are not officially supported.

## Default files in a remote scene

Remote scenes inclde the following files:

1.  **server/RemoteScene.tsx**: The entry point of the scene.
2.  **server/State.ts**: Handles the scene state. This is executed in the remote server.
3.  **server/Server.ts**: Configuration for the remote server.
4.  **server/ConnectedClients.ts**: Handles users of the scene.
5.  **server/build.json**: Instructions for the scene build.
6.  **server/tsconfig.json**: Typescript configuration file.
7.  **server/build.json**: The file with the instructions to build the scene.
8.  **package.json** and **package-lock.json**: Specify the versions of all dependencies of the scene.
9.  **scene.json**: The manifest that contains metadata for the scene.

#### RemoteScene.tsx

It contains the code that generates an entity tree, which is what end users of your parcel will see. Its biggest difference with the _scene.tsx_ file of a static scene is that it doesn't define a scene state, but instead it imports the state and functions to access it from _State.ts_.

Below is a basic example of a _RemoteScene.tsx_ file:

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

> **Important note:** Your _RemoteScene.tsx_ file must always include an `export default class`, that's how our SDK finds the class to initialize the scene.

#### State.ts

This file handles the scene state. Remote scenes keep the state stored in a remote server.

The file includes the definition of the scene state and two functions that are used to get and to set information from this remote state.

Below is a basic example of a _State.ts_ file:

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

#### scene.json

The _scene.json_ file is a JSON formatted manifest for a scene in the world. A scene can span a single or multiple LAND parcels. The _scene.json_ manifest describes what objects exist in the scene, a list of any assets needed to render it, contact information for the parcel owner, and security settings. For more information and an example of a
_scene.json_ file, please visit the [Decentraland specification proposal](https://github.com/decentraland/proposals/blob/master/dsp/0020.mediawiki).

When you run the `dcl init` command, it prompts you to enter some descriptive metadata, these datais are stored in
the scene.json manifest file for the scene. All of this
metadata is optional for previewing the scene locally, but part of it is needed for deploying. You can change this information manually at any time.

#### package.json

This file provides information to NPM that allows it to identify the project, as well as handle the project's dependencies. Decentraland scenes need two packages:

- **decentraland-api**: allows the scene to communicate with the world engine.
- **typescript**: used to compile the file _scene.tsx_ to javascript.

#### package-lock.json

This file lists the versions of all the other dependencies of the project. These versions are locked, meaning that the compiler will use literally the same minor release listed here.

You can change any package version manually by editing this file.

#### build.json

This is the Decentraland build configuration file.

#### tsconfig.json

Directories containing a _tsconfig.json_ file are root directories for TypeScript Projects. The _tsconfig.json_ file specifies the root files and options required to compile your project from TypeScript into JavaScript.

> You can use another tool or language instead of TypeScript, so long as your scripts are contained within a single Javascript file (scene.js). All provided type declarations are made in TypeScript, and other languages and transpilers are not officially supported.

## Default files in a static scene

_A static scene_ includes the following files:

1.  **scene.json**: The manifest that contains metadata for the scene.
2.  **scene.xml**: The content of the static scene.

#### scene.xml (static scenes)

For both static and dynamic scenes, the end result is the same: a tree of entities. The root of the tree is always a `<scene>` element. XML scenes call out this structure explicitly, TypeScript scenes provide the script to build and update this structure.

```xml
<scene>
  <sphere position="1 1 1"></sphere>
  <box position="3.789 2.3 4.065" scale="1 10 1"></box>
  <box position="2.212 7.141 4.089" scale="2.5 0.2 1"></box>
  <gitf-model src="crate/crate.gitf" position="5 1 5"></box>
</scene>
```

Since the root scene element is a transform node, it can also be translated, scaled and rotated. Those capabilities are useful to, for example, change the center of coordinates of the entire parcel:

```xml
<scene position="5 5 5">
  <box position="0 0 0"></box>
  <!-- in this example, the box is located at the world position 5 5 5 -->
</scene>
```

#### package.json

This file provides information to NPM that allows it to identify the project, as well as handle the project's dependencies. Decentraland static scenes need this package:

**decentraland-api**: allows the scene to communicate with the world engine.

## Recommended file locations

Keep in mind that when you deploy your scene to Decentraland, any assets or external libraries that are needed to use your scene must be either packaged inside the scene folder or available via a remote server.

Anything that is meant to run in the user's client must located inside the scene folder. That includes external libraries like Babylon.js. You shouldn't reference libraries that are installed elsewhere in your local machine, because they won't not be available to the deployed scene.

We suggest using these folder names consistently for storing the different types of assets that your scene might need:

- 3d models: `/models`
- Videos: `/videos`
- Sound files: `/sounds`
- Image files for textures: `/materials`
- _.tsx_ definitions for reusable components `/components`

## The dclignore file

All scenes include a _.dclignore_ file, this file specifies what files in the scene folder to ignore when deploying a scene to Decentraland.

For example, you might like to keep the Blender files for the 3D models in your scene inside the scene folder, but you want to prevent those files from being deployed to Decentraland. In that case, you could add `*.blend` to _.dclignore_ to ignore all files with that extension.

| What to ignore | Example      | Description                                                                             |
| -------------- | ------------ | --------------------------------------------------------------------------------------- |
| Specific files | `BACKUP.tsx` | Ignores a specific file                                                                 |
| Folders        | `drafts/`    | Ignores entire contents of a folder and its subfolders                                  |
| Extensions     | `*.blend`    | Ignores all files with a given extension                                                |
| Name sections  | `test*`      | Ignores all files with names that match the query. In this case, that start with _test_ |
