---
date: 2018-01-10
title: 材质
description: 导入到 Decentraland 的 3D 模型上支持哪些材质属性和纹理。

categories:
  - 3d-modeling
type: Document
set: 3d-modeling
set_order: 10
---

材质被嵌入到 _.gltf_ 或 _.glb_ 文件中。

本文档涉及在 3D 模型中导入的材质。 对于通过代码定义的材质应用于基本形状，请参阅[材质]({{ site.baseurl }}{% post_url /development-guide/2018-02-7-materials %})。

> 注意：您当前无法从场景代码中动态更改 3D 模型的材质，除非是基本形状。

#### Shader 着色器支持

并非所有着色器都可导入 Decentraland 的模型。 如果您正在使用 Blender，确保使用以下的一种方式：

- 标准材质：支持任何着色器，例如漫反射，镜面反射，透明度等。

  > 提示：使用 Blender 时，这些是 Blender Render 渲染支持的材质。

- PBR（基于物理的渲染）材质：此着色器非常灵活，因为它包含漫反射，粗糙度，金属度和辐射等属性，允许您配置材质与光的交互方式。

  > 提示：使用 Blender 时，可以通过设置 Cycles 渲染器并添加 Principled BSDF 着色器来使用 PBR 材质。请注意，Cycles 渲染器的其他着色器均不受支持。


下图显示了两个相同的模型，使用相同的颜色和纹理创建。左侧的模型使用所有_PBR_材质，其中一些包括 _metalness_，_transparency_ 和 _emissiveness_ 效果。 右侧的模型全部使用 _standard_ 材质，一些包括 _transparency_ 和 _emissiveness_ 效果。

![](/images/media/materials_pbr_basic.png)  

## 透明材质

您可以将材质设置为 _transparent 透明_。 透明的程序取决于 _alpha_ 设置。为此，请设置材质的透明属性，然后将其 _alpha_ 设置为所需的量。_alpha_ 设为 1 将使材料完全不透明，0 的将会完全透明而不可见。

下图显示了使用标准材料创建的两个相同模型。 左侧的材料仅使用不透明材料，右侧的材料在其某些部分使用透明和发光材料。

![](/images/media/materials_transparent_emissive.png)

有两种主要的不同透明度模式：_Aplha Clip_ 和 _Aplha Blend_。

_Alpha Clip_ 设置模型的每个部分是 100％ 不透明或 100％ 透明。 _Alpha Blend_ 可以为每个区域选择中间值。

![](/images/media/transparency-modes.png)

除非您特别希望能够具有中间级透明度，否则最好使用 _Alpha Clip_。

## 发光材质

你也可以制作_发光_材质。发光材质能自己发光。请注意，在渲染时，实际上它们并不能照亮场景中的附近物体，而是似乎在它们周围有模糊的光晕。

下图显示了使用标准材料创建的两个相同模型。 右侧的那个在其一些表面上使用了发光材质。

![](/images/media/materials_transparent_emissive.png)


要在 Blender 中制作发光材质，只需在材质上添加 `emission` 着色器即可。

![](/images/media/simple-emissive.png)

为了使材料既具有发光性又具有纹理，您可以并行使用两个着色器，一个是`emission`，另一个是纹理的 `principled BDSF` 。 然后，您可以使用 `mix shader` 节点将它们联接起来。

![](/images/media/apply-emissive.png)

> 提示：通过将颜色图集作为纹理，可以将各种可能的颜色计算为单个纹理。 这对于确保您不超过[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})非常有用。

![](/images/media/neon-texture.png)

#### 减弱发光

`emission` 着色器具有 `strength` 属性，可以降低发光材质的发光度。 但是，由于 _.glTF_ 规范存在的问题，导出的 _.glTF_ 或 _.glb_ 文件不会保留此属性。 将模型导入 Decentraland 场景时，它的发光度始终为 100％。

为了减少材质发光度，最好的解决方法是将 `emission` 着色器上的 `color` 属性设置为不太明亮的颜色，或者从不太明亮的纹理中引用颜色。

例如，如果使用下面的颜色贴图，则可以通过从图像的下半部分选择颜色来实现不太明亮的发光材质。 上半部分都是完全发光的，选择更低的，材料的发光会降低。

![](/images/media/neon-texture.png)


#### 纹理

纹理可以嵌入到导出的 glTF 文件中，也可以从外部文件中引用。 这两种方式都是支持的。

<!--

There are different kinds of textures you can use in a 3D model:

- albedo textures: don't use light
- alpha textures: determine only the transparency regions and its degree
- bump texture: Stores surface normal data used to displace a mesh in a texture. Used with BPR.
- emissiveTexture
- refractionTexture



link to content guide to show how you set materials for primitives

what extensions are supported for image files?
anything special to use alpha

what special layers PBR uses?


-->

##### 默认纹理

默认 Decentraland 资源库中（在场景编辑器中或作为可穿戴设备）的所有资源共享一组优化的平面纹理。 玩家在打开浏览器时会预先加载这些纹理，这样可以加快这些资源的加载。

如果您构建自己的自定义 3D 模型并使用了这些 Decentraland 默认纹理，当玩家走到您的地块时，资源加载速度也会更快。

这些纹理由纯色调色板组成，您可以将其映射到 3D 模型的不同部分。

<img src="/images/media/MiniTown_TX.png" alt="Minitown texture" width="250"/>

你可以在[这个库](https://github.com/decentraland/builder-assets/tree/master/textures)中找到Decentrlanad 全部的默认纹理。

#### 纹理大小限制

纹理大小必须使用与以下数字匹配的宽度和高度数字（以像素为单位）：

```
1, 2, 4, 8, 16, 32, 64, 128, 256, 512
```

> 该序列由 2 的幂组成：f(x) = 2 ^ x。 512 是我们允许的纹理大小的最大数量。 这在其他渲染引擎中也是相当普遍的要求，它与图形处理器内部优化有关。

宽度和高度不需要具有相同的数字，但它们都必需属于此序列中。

**纹理的建议大小为 512x512**，我们发现这是通过国内网络传输的最佳尺寸，并提供合理的加载/品质体验。

其他有效尺寸的示例:

```
32x32
64x32
512x256
512x512
```

> 虽然任意大小的纹理在 alpha 版本中能起作用，但引擎会在控制台中显示警报。 我们将在即将发布的版本中强制执行此限制，并且无效的纹理大小将停止工作。

#### 如何更换材质

假设您已导入了具有 Decentraland 所不支持的材质的 3D 模型。 您可以轻松更改此材质，同时仍保持相同的纹理及其贴图。

<img src="/images/media/materials-not-supported.png" alt="Model without valid material" width="250"/>

更换材质:

1. 检查当前材质设置，以查看正在使用的纹理文件及其配置方式。
2. 从网格中删除当前材质。

   ![](/images/media/materials_delete_material.png)

3. 创建一个新材质。

    <img src="/images/media/materials_new_material.png" alt="New default basic material" width="400"/>

   > 提示：如果您正在使用 Blender 并且在 _Blender Render_ 选项卡上，它默认会创建一个基本材质，这是Decentraland支持的。

4. 打开 _Textures_ 设置并创建新纹理，导入与原始材质相同的图像文件。

   <img src="/images/media/materials_new_texture.png" alt="New default basic texture" width="300"/>

5. 纹理应该映射到新材质，就像它映射到旧材质一样。

   <img src="/images/media/materials_final.png" alt="Model with valid material" width="300"/>

#### 材质的最佳实践

- 如果场景中包含有多个使用相同纹理的模型，请将纹理引用为外部文件，而不是将其嵌入到 3D 模型中。因为嵌入的纹理会在每个模型中复制，从而增加场景的大小。_.glb_ 文件默认嵌入了纹理，但您可以使用[glTF pipeline]（https://github.com/AnalyticalGraphicsInc/gltf-pipeline）将其提取到外部。

   > 注意：在引用未嵌入的纹理的文件后，请确保不会移动或重命名该文件，否则将丢失对该文件的引用。 该文件也必须位于场景文件夹中，以便与场景一起上传。
- 使用可由玩家预先加载的 Decentraland [默认纹理](https://github.com/decentraland/builder-assets/tree/master/textures)，能加快资源渲染速度。
- 阅读[本文](https://www.khronos.org/blog/art-pipeline-for-gltf)，详细了解在 glTF 模型中使用 PBR 纹理的完整流程。
- 在 [cgbookcase](https://cgbookcase.com/) 上可以找到免费高质量 PBR 纹理。
- 设置材质的透明度时，请尝试始终使用 _Alpha clip_ 而不是 _Alpha blend_，除非您特别需要具有部分透明的材质（如玻璃）。 这能避免引擎在其它模型之前显示错误模型的问题。