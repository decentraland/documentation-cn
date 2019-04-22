---
date: 2018-01-06
title: 场景限制
description: 设计场景时有哪些限制？
redirect_from:
  - /documentation/scene-limitations/
categories:
  - development-guide
type: Document
set: development-guide
set_order: 6
---

In order to improve performance in the metaverse, we have established a set of limits that every scene must follow. If a
scene exceeds these limitations, then the parcel won't be loaded and the preview will display an error message.

为了提高虚拟世界的性能，我们设计了每个场景必须遵循的一套限制规则。如果场景超出这些限制规则，将不会被加载，并且在预览时会显示一个错误消息。

For a straight-forward reference of what limitations you'll have for a specific number of parcels, check the following table:

特定数量土地相关限制的参考表，请查看下表：

[土地限制参考表](https://docs.google.com/spreadsheets/d/1BTm0C20PqdQDAN7vOQ6FpnkVncPecJt-EwTSNHzrsmg/edit#gid=0)

## Scene limitation rules
## 场景限制规则

Below are the maximum number of elements allowed allowed in a scene:

以下是场景中允许的最大元素数：

> _n_ 表示场景占用的地块数。

- **三角形:** `n x 10000` 场景中所有模型的三角形总量。
- **实体:** `n x 200` 场景中的网格数量。
- **Bodies:** `n x 300` 场景中的网格数量。
- **材质:** `log2(n+1) x 20` 场景中的材质数量。 包括作为模型的一部分导入的材质。
- **纹理:** `log2(n+1) x 10` 场景中的纹理量。 包括作为模型的一部分导入的纹理。
- **高度:** `log2(n+1) x 20` 以米为单位的高度。

  > 注意：用户头像和用户从场景外带来的任何项目都不在这些限制内。

## Query scene limitations via code
## 通过代码查询场景限制

From a scene's code, you can query both the limitations that apply to the scene and how much the scene is currently using. This is especially useful with scenes where the content changes dynamically. For example, in a scene where you add a new entity each time the user clicks, you could stop adding entities when you reach the scene limits.

从场景的代码中，您可以查询适用于场景的限制以及场景当前使用的数量。这对于其内容动态更改的场景尤其有用。例如，在每次用户单击就添加新实体的场景中，您就可以在达到场景限制时停止添加实体。


To use this functionality, you must first import `EntityController` into your scene.

要使用此功能，必须先将 `EntityController` 导入场景。


```ts
import { querySceneLimits } from "@decentraland/EntityController"
```

#### Obtain scene limitations
#### 获取场景限制

Run `this.entityController.querySceneLimits()` to obtain the limits of your scene. The limits are calculated for your scene based on how many parcels it occupies, according to the _scene.json_ file. The values returned by this command don't change over time, as the scene's size is always the same.

运行 `this.entityController.querySceneLimits()` 以获取场景的限制。根据 _scene.json_ 文件，根据场景占用的土地数计算场景的限制。此命令返回的值不会随时间变化，因为场景的大小是不变的。


The `querySceneLimits()` is [asynchronous]({{ site.baseurl }}{% post_url /development-guide/2018-02-25-async-functions %}), so we recommend calling it using the `executeTask()` function, including an `await` statement.

`querySceneLimits()`是[异步]({{ site.baseurl }}{% post_url /development-guide/2018-02-25-async-functions %})的，所以我们在 `executeTask()` 函数中调用，包括 `await` 语句。


```ts
executeTask(async () => {
  try {
    const limits = await querySceneLimits()
    log('limits' + limits)
  }
})
```

The `querySceneLimits()` function returns a promise of an object with the following properties, all of type _number_.

`querySceneLimits()` 函数返回具有以下属性的 promise 对象，所有类型都是 _number_。


```tsx
// import controller
import { querySceneLimits } from '@decentraland/EntityController'


// get limits object
executeTask(async () => {
  try {
    const limits = await querySceneLimits()

    // print maximum triangles
    log(limits.triangles)

    // print maximum entities
    log(limits.entities)

    // print maximum bodies
    log(limits.bodies)

    // print maximum materials
    log(limits.materials)

    // print maximum textures
    log(limits.textures)
  }
}
```


For example, if your scene has only one parcel, logging `limits.triangles` should print `10000`.

例如，如果您的场景只有一块地，则 `limits.triangles` 应该是 `10000`。


#### Obtain the current use
#### 获取当前使用情况


Just as you can check via code the maximum allowed values for your scene, you can also check how much of that is currently used by the scene. You do this by running `this.querySceneMetrics()`. The values returned by this command change over time as your scene renders different content.

就像您可以通过代码检查场景的最大允许值一样，您也可以检查场景当前使用的值。运行 `this.querySceneMetrics（）` 可以做到这一点。当场景呈现不同的内容时，此命令返回的值会随着时间的不同而变化。


The `querySceneMetrics()` is asynchronous, so we recommend calling it using the `executeTask()` function, including an `await` statement.

`querySceneMetrics()` 是异步的，所以我们建议在 `executeTask()` 函数中调用，包括 `await` 语句。


```ts
executeTask(async () => {
  try {
    const limits = await querySceneLimits()
    log('limits' + limits)
  }
})
```

The `querySceneMetrics()` function returns a promise of an object with the following properties, all of type _number_.

`querySceneMetrics()` 函数返回具有以下属性的 promise 对象，所有类型都是 _number_。


```tsx
// import controller
import { querySceneMetrics } from '@decentraland/EntityController'


// get limits object
executeTask(async () => {
  try {
    const limits = await querySceneMetrics()

    // print maximum triangles
    log(limits.triangles)

    // print maximum entities
    log(limits.entities)

    // print maximum bodies
    log(limits.bodies)

    // print maximum materials
    log(limits.materials)

    // print maximum textures
    log(limits.textures)
  }
}
```

For example, if your scene is only rendering one box entity at the time, logging `limits.entities` should print `1`.

例如，如果您的场景当时只渲染一个 box 实体，则`limits.entities` 应该为 `1`。

## Scene boundaries
## 场景边界

When running a preview, any content that is located outside the parcel boundaries is highlighted in red when rendered. If any content is outside these boundaries, you won't be allowed to deploy this scene to Decentraland.

运行预览时，位于土块边界外的任何内容在渲染时都会以红色突出显示。如果内容超出这些边界，将不允许将场景部署到 Decentraland。


## Shader limitations
## Shader 限制

3D models used in decentraland must use supported shaders and materials. See [3D model considerations]({{ site.baseurl }}{% post_url /development-guide/2018-01-09-external-3d-models %}) for a list of supported shaders.

在decentraland中使用的3D模型必须使用支持的着色器和材料。有关支持的着色器列表，请参阅[3D模型注意事项]（{{site.baseurl}} {％post_url / development-guide / 2018-01-09-external-3d-models％}）。

在 decentraland 中使用的 3D 模型必须使用支持的 shaders 和材质。有关支持的着色器列表，请参阅 [3D 模型注意事项]({{ site.baseurl }}{% post_url /development-guide/2018-01-09-external-3d-models %}) 。

## Lighting
## 灯光效果

The scene's lighting conditions can't be changed from the default setting.

灯光采用默认设置，无法更改场景的灯光效果。

Entities don't cast shadows over other entities and dynamic lighting isn’t supported.

实体不会投射阴影到其他实体上，也不支持动态光照。

## Texture size constraints
## 纹理大小限制

Texture sizes must use width and height numbers (in pixels) that match the following numbers:
纹理大小必须使用与以下数字匹配的宽度和高度数字（以像素为单位）：


```
1, 2, 4, 8, 16, 32, 64, 128, 256, 512
```

> This sequence is made up of powers of two: `f(x) = 2 ^ x` . 512 is the maximum number we allow for a texture size. This is a fairly common requirement among other rendering engines, it's there due internal optimizations of the graphics processors.

> 该序列由 2 的幂组成：`f(x) = 2 ^ x`。 512 是我们允许的纹理大小的最大数量。 这在其他渲染引擎中也是相当普遍的要求，它有关图形处理器的内部优化。


The width and height don't need to have the same number, but they both need to belong to this sequence.

宽度和高度不需要具有相同的数字，但它们都必需属于此序列中。


**The recommended size for textures is 512x512**, we have found this to be the optimal size to be transported through domestic networks and to provide reasonable loading/quality experiences.

**纹理的建议大小为 512x512**，我们发现这是通过国内网络传输的最佳尺寸，并提供合理的加载/品质体验。


Examples of other valid sizes:

其他有效尺寸的示例：

```
32x32
64x32
512x256
512x512
```

> Although textures of arbitrary sizes work in the alpha release, the engine displays an alert in the console. We will enforce this restriction in coming releases and invalid texture sizes will cease to work.

> 虽然任意大小的纹理在 alpha 版本中能起作用，但渲染引擎会在控制台中显示警报。 我们将在即将发布的版本中强制执行此限制，并且无效的纹理大小设置将停止工作。


<!--
## File amount limitations

When deploying your scene, you can't upload more than 100 files to IPFS, as having too many files in a scene will make it take too long to load in the client.

If you have more than 100 files in your scene folder, it's likely that many of those files aren't being used directly when loading the scene. You can make the CLI ignore specific files from the scene folder and not upload them to IPFS by specifying them in the _dclignore_ file for the scene. Learn more about it in [Scene files]({{ site.baseurl }}{% post_url /development-guide/2018-01-11-scene-files %}).
-->
