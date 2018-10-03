---
date: 2018-01-06
title: 场景限制
description: 我可以在场景中放多少东西？
redirect_from:
  - /documentation/scene-limitations/
categories:
  - development-guide
type: Document
set: development-guide
set_order: 6
---

为了提高虚拟实境的性能，我们建立每个场景必须遵循的限制。 如果一个场景超出这些限制，将不加载地块，预览将显示错误消息。

## 场景限制规则

以下是场景中允许的最大元素数：

> *n* 表示场景占用的地块数。

- **三角形:** `log2(n+1) x 10000` 场景中所有模型的三角形总量。
- **实体:** `log2(n+1) x 200` 场景中中实体数。
- **支架:** `log2(n+1) x 300` 场景中的网格数量.
- **材质:** `log2(n+1) x 20` 场景中的材质数量。 包括作为模型的一部分导入的材质。
- **纹理:** `log2(n+1) x 10` 场景中的纹理量。 包括作为模型的一部分导入的纹理。
- **高度:** `log2(n+1) x 20` 以米为单位的高度。

## 通过代码查询场景限制

从场景的代码中，您可以查询适用于场景的限制以及场景当前使用的数量。这对于其内容动态更改的场景尤其有用。例如，在每次用户单击就添加新实体的场景中，您就可以在达到场景限制时停止添加实体。

 #### 获得场景限制

`this.entityController.querySceneLimits()` 可以获取场景的限制。它根据 _scene.json_ 文件中场景占用的地块数计算场景的限制。此命令返回的值不会随时间变化，因为场景的大小是不变的。

`querySceneLimits()` 是异步的，所以我们建议用 `await` 语句调用。

`querySceneLimits()` 函数返回具有以下属性的 promise 对象，所有属性都是 _number_。

{% raw %}

 ```tsx
 // get limits object
 const limits = await this.entityController.querySceneLimits()

 // print maximum triangles
 console.log(limits.triangles)

 // print maximum entities
 console.log(limits.entities)

 // print maximum bodies
 console.log(limits.bodies)

 // print maximum materials
 console.log(limits.materials)

 // print maximum textures
 console.log(limits.textures)
 ```

{% endraw %}

例如，如果您的场景只有一块地，则 `limits.triangles` 应该是 `10000`。

#### 获取当前使用情况

就像您可以通过代码检查场景的最大允许值一样，您也可以检查场景当前使用的值。运行 `this.entityController.querySceneMetrics()` 可以做到这一点。当场景呈现不同的内容时，此命令返回的值会随着时间的不同而变化。

`querySceneMetrics()` 是异步的，因此我们建议使用 `await` 语句调用。

`querySceneMetrics()` 函数返回具有以下属性的 promise 对象，所有属性都是 _number_。

 {% raw %}

 ```tsx
 //get metrics object
 const limits = await this.entityController.querySceneMetrics()

 //print maximum triangles
 console.log(limits.triangles)

 //print maximum entities
 console.log(limits.entities)

 //print maximum bodies
 console.log(limits.bodies)

 //print maximum materials
 console.log(limits.materials)

 //print maximum textures
 console.log(limits.textures)
 ```

 {% endraw %}

例如，如果您的场景只用了一块地，则 limits.triangles 显示为 10000。

## 场景边界

运行预览时，位于土块边界外的任何内容在渲染时都会以红色突出显示。如果内容超出这些边界，将不允许将场景部署到 Decentraland。

## Shader 着色器限制

在 decentraland 中使用的3D模型必须使用支持的着色器和材质。有关支持的着色器列表，请参阅[3D模型注意事项]({{ site.baseurl }}{% post_url /development-guide/2018-01-09-external-3d-models %}) 。

## 纹理大小限制

纹理大小必须使用与以下数字匹配的宽度和高度数字（以像素为单位）：

```
1, 2, 4, 8, 16, 32, 64, 128, 256, 512
```

> 该序列由 2 的幂组成：`f(x) = 2 ^ x`。 512 是我们允许的纹理大小的最大数量。 这在其他渲染引擎中也是相当普遍的要求，它有关图形处理器的内部优化。

宽度和高度不需要具有相同的数字，但它们都必需属于此序列中。

**纹理的建议大小为 512x512**，我们发现这是通过国内网络传输的最佳尺寸，并提供合理的加载/品质体验。

其他有效尺寸的示例：
```
32x32
64x32
512x256
512x512
```

> 虽然任意大小的纹理在 alpha 版本中能起作用，但渲染引擎会在控制台中显示警报。 我们将在即将发布的版本中强制执行此限制，并且无效的纹理大小设置将停止工作。

## 文件数量限制

部署场景时，超过 100 个文件不能上传到 IPFS，因为场景中包含太多文件会导致在客户端中加载时间过长。

如果场景文件夹中有超过 100 个文件，加载场景时其中许多文件可能没有直接使用。您可以使 CLI 忽略场景文件夹中的特定文件，并在 _dclignore_ 文件中指定不将它们上传到 IPFS。在[场景文件]({{ site.baseurl }}{% post_url /development-guide/2018-01-11-scene-files %})中了解有关更多信息。