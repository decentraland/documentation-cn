---
date: 2018-01-06
title: 场景限制
description: 我可以在场景中放多少东西？
categories:
  - documentation
type: Document
set: sdk-reference
set_order: 6
---
In order to improve performance in the metaverse, we have established a set of limits that every scene must follow. If a
scene exceeds these limitations, then the parcel won't be loaded and the preview will display an error message.

为了提高虚拟实境的性能，我们建立每个场景必须遵循的限制。 如果一个场景超出这些限制，将不加载地块，预览将显示错误消息。

## Entity constraints
### 实体约束

Below are the maximum number of elements allowed allowed in a scene:
以下是场景中允许的最大元素数：
> *n* represents the number of parcels that a scene occupies. 
> *n* 表示场景占用的地块数。

* **Triangles:** `log2(n+1) x 10000` Total amount of triangles for all the models in the scene.
* **Entities:** `log2(n+1) x 200` Amount of entities in the scene.
* **Bodies:** `log2(n+1) x 300` Amount of meshes in the scene.
* **Materials:** `log2(n+1) x 20` Amount of materials in the scene. It includes materials imported as part of models.
* **Textures:** `log2(n+1) x 10` Amount of textures in the scene. It includes textures imported as part of models.
* **Height:** `log2(n+1) x 20` Height in meters.

* **三角形:** `log2(n+1) x 10000` 场景中所有模型的三角形总量。
* **实体:** `log2(n+1) x 200` 场景中中实体数。
* **支架:** `log2(n+1) x 300` 场景中的网格数量.
* **材质:** `log2(n+1) x 20` 场景中的材质数量。 包括作为模型的一部分导入的材质。
* **纹理:** `log2(n+1) x 10` 场景中的纹理量。 包括作为模型的一部分导入的纹理。
* **高度:** `log2(n+1) x 20` 以米为单位的高度。

When running a preview, any content that exceeds parcel boundaries are highlighted in red when rendered.

运行预览时，任何超出地块边界的内容在渲染时都会以红色突出显示。

When your parcel is rendered, any static content extending beyond your parcel's boundaries is replaced with an error message. All dynamic entities that cross your parcel boundaries are deleted from the rendered scene.

渲染地块时，超出地块边界的任何静态内容都将替换为错误消息。 跨越地块边界的所有动态实体都将从渲染场景中删除。

## Texture size constraints
## 纹理大小限制

Texture sizes must use width and height numbers (in pixels) that match the following numbers:
纹理大小必须使用与以下数字匹配的宽度和高度数字（以像素为单位）：

```
1, 2, 4, 8, 16, 32, 64, 128, 256, 512
```

>  This sequence is made up of powers of two: `f(x) = 2 ^ x` . 512 is the maximum number we allow for a texture size. This is a fairly common requirement among other rendering engines, it's there due internal optimizations of the graphics processors.
> 该序列由 2 的幂组成：`f(x) = 2 ^ x`。 512 是我们允许的纹理大小的最大数量。 这在其他渲染引擎中也是相当普遍的要求，它有关图形处理器的内部优化。

The width and height don't need to have the same number, but they both need to belong to this sequence.
宽度和高度不需要具有相同的数字，但它们都必需属于此序列中。

**The recommended size for textures is 512x512**, we have found this to be the optimal size to be transported thru domestic networks and to provide reasonable loading/quality experiences.
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
> 虽然任意大小的纹理在 alpha 版本中能起作用，但引擎会在控制台中显示警报。 我们将在即将发布的版本中强制执行此限制，并且无效的纹理大小将停止工作。

## Shader support
## 着色器支持

Not all shaders can be used in models that are imported into Decentraland. If you're working with Blender, you can either use:
并非所有着色器都可导入 Decentraland 的模型。 如果您正在使用 Blender，您可以使用：

* Using Blender Render, any of its shaders are supported, for example diffuse, specular, transparency, etc.
* Using the Cycles renderer, you can *only* use PBR (Physically Based Rendering). That's done by using the `Principled BSDF` shader. This shader is extremely flexible, as it includes properties like diffuse, roughness, metalness and emission that allow you to configure how a material interacts with light.
  
* 使用 Blender Render，支持所有的着色器，例如漫反射，镜面反射，透明度等。
* 使用 Cycles 渲染器，您*只能*使用 PBR（基于物理的渲染）。 这是通过使用 `Principled BSDF` 着色器完成的。 此着色器非常灵活，它包含漫反射，粗糙度，金属度和辐射等属性，允许您配置材质与光的交互方式。

> None of the other shaders of the Cycles renderer are supported.
> Cycles 渲染器的其他着色器均不受支持。

