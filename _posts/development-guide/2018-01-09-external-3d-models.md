---
date: 2018-01-09
title: 3D 模型考虑因素
description: 外部 3D 模型支持哪些资产和组件，将它们导入 Decentraland 之前如何配置
redirect_from:
  - /sdk-reference/external-3d-models/
categories:
  - development-guide
type: Document
set: development-guide
set_order: 9
---

3D 模型以 glTF 格式导入到 decentraland 中。这些模型可以包含许多支持的功能。本节介绍如何使它们与Decentraland 兼容及最佳实践。

有关如何在 Decentraland 场景中配置 3D 模型的位置，比例，激活动画等的信息，请参阅[场景内容指南]({{ site.baseurl }}{% post_url /development-guide/2018-01-21-scene-content %})。

请记住，所有模型，着色器和纹理都必须在[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})的参数范围内。


## 支持的 3D 模型格式

Decentraland 中的所有 3D 模型都必须采用 glTF 格式。 [glTF](https://www.khronos.org/gltf) (GL Transmission Format) 是 Khronos 的一个开源项目，为 3D 资产提供了一种通用的，可扩展的格式，既高效又可与现代网络技术进行高度互操作。

glTF 模型可以具有 _.gltf_ 或 _.glb_ 扩展名。 glTF 文件是可读的，您可以在文本编辑器中打开它并像 JSON 文件一样读取它。例如，可以用来验证动画是否已正确附加并检查其名称。 glb 文件是二进制文件，所以它们不可读，但是文件要小得多，这对场景的性能有好处。

我们建议您在处理场景时使用 _.gltf_，但在上传时切换到 _.glb_。

以下内容可以嵌入 _glTF_ 文件中或从外部引用：

- 纹理可以嵌入或从外部图像文件引用。
- 有关几何、动画和模型的其他缓冲区相关方面的二进制数据可以嵌入或使用外部 _.bin_ 文件引用。

> 注意：动画_必须_嵌入在_glTF_文件中以在 Decentraland 中使用。

#### 在 Blender 中导出 glTF

默认情况下，Blender 不支持 glTF 格式导出，但可以安装插件来支持 glTF 格式导出。

1. 首先下载[Khronos Exporter](https://github.com/KhronosGroup/glTF-Blender-Exporter)
2. 解压缩 _.zip_ 文件，然后将 `scripts/addons/io_scene_gltf2` 目录复制到 Blender 安装目录的`scripts / addons` 文件夹下。
3. 激活插件：在 Blender 中打开 _User Preferences ..._。 在 _Add-ons_ 选项卡中，启用 **Import-Export: glTF 2.0 format**。 不要忘记单击 _Save User Settings_。
    > 注意：如果已安装有另外的 glTF 2.0 导出插件，请先将其禁用。 只能同时使用一个。   

导出包含多个动画的 3D 模型时，请确保所有动画都嵌入到模型中。 为此，请打开 _NLA editor_ 并单击 _Stash_ 将每个动画添加到模型中。

导出带动画的模型时，建议使用以下导出设置：

![](/images/media/blender-export-settings-animations.png)

#### 从 3D Studio Max 导出 glTF

默认情况下，3D Studio Max 不支持 glTF 格式导出，但可以安装插件来支持。

1. [在此链接](https://github.com/BabylonJS/Exporters/tree/master/3ds%20Max) 下载插件。
2. 按照[说明](http://doc.babylonjs.com/resources/3dsmax#how-to-install-the-3ds-max-plugin) 安装插件。
3. 按[此说明](http://doc.babylonjs.com/resources/3dsmax_to_gltf)使用插件导出 glTF 文件。

#### 从 Maya 中导出 glTF

默认情况下，Maya 不支持 glTF 格式导出，但可以安装插件来获得支持。

1. 按照[这些说明](http://doc.babylonjs.com/resources/maya)安装插件。
2. 按[此说明](http://doc.babylonjs.com/resources/maya_to_gltf#pbr-materials) 导出 glTF .

> 注意：作为替代方案，您也可以尝试[此插件](https://github.com/WonderMediaProductions/Maya2glTF)。

#### 从 Unity 导出 glTF

默认情况下，Unity 不支持 glTF 格式导出，但可以安装插件来获得支持。

[从此下载插件](https://github.com/sketchfab/Unity-glTF-Exporter).

> 注意：作为替代方案，您也可以尝试[此插件](https://assetstore.unity.com/packages/tools/utilities/collada-exporter-for-unity2017-99793)

#### 从 SketchUp 导出 glTF

默认情况下，SketchUp 不支持 glTF 格式导出，但可以安装插件来获得支持。

[从此下载插件](https://extensions.sketchup.com/en/content/gltf-exporter).

#### 预览 glTF 模型

在将 glTF 模型导入场景之前预览 glTF 模型内容的快速简便方法是使用[Babylon.js Sandbox](https://sandbox.babylonjs.com/)。 只需将 glTF 文件（及其 _.bin_ 文件，如果有的话）拖放到画布中即可查看模型。

在 sandbox 中，要查看模型中嵌入的动画，可以从下拉菜单中选择要显示的动画。

![](/images/media/babylon-sandbox.png)

#### 为什么使用 glTF

与旧的只支持 vertices 顶点、normals 法线、纹理坐标和基本材质的 _OBJ format_ 格式相比，

glTF 提供了一组更强大的功能，包括：

- 分层对象
- 骨骼结构和动画
- 更强大的材质和着色器
- 场景信息（光源，相机）

> 注意：作为遗留功能，Decentraland 场景支持 _.obj_ 模型，但以后可能不再支持。

与 _COLLADA_ 相比，支持的功能非常相似。但是，因为 glTF 专注于提供“传输格式”而不是编辑器格式，它与 Web 技术更具互操作性。

考虑这个类比：.PSD（Adobe Photoshop）格式有助于编辑 2D 图像，但必须将图像转换为 .JPG 才能使用。同样，在网络上，COLLADA 可用于编辑 3D 资源，但要在传输的同时进行渲染， glTF 是一种更简单的方式。

## 材质

#### Shader 着色器支持

并非所有着色器都可导入 Decentraland 的模型。 如果您正在使用 Blender，确保使用以下的一种方式：

- 标准材质：支持任何着色器，例如漫反射，镜面反射，透明度等。

  > 提示：使用 Blender 时，这些是 Blender Render 渲染支持的材质。

- PBR（基于物理的渲染）材质：此着色器非常灵活，因为它包含漫反射，粗糙度，金属度和辐射等属性，允许您配置材质与光的交互方式。

  > 提示：使用 Blender 时，可以通过设置 Cycles 渲染器并添加 Principled BSDF 着色器来使用 PBR 材质。请注意，Cycles 渲染器的其他着色器均不受支持。


下图显示了两个相同的模型，使用相同的颜色和纹理创建。左侧的模型使用所有_PBR_材质，其中一些包括 _metalness_，_transparency_ 和 _emissiveness_ 效果。 右侧的模型全部使用 _standard_ 材质，一些包括 _transparency_ 和 _emissiveness_ 效果。

![](/images/media/materials_pbr_basic.png)  

请参阅 [实体接口]({{ site.baseurl }}{% post_url /development-guide/2018-06-21-entity-interfaces %}) 查看材质可以配置的所有属性列表

#### 透明和发光材料

您可以将材质设置为 _transparent 透明_。 透明的程序取决于 _alpha_ 设置。为此，请设置材质的透明属性，然后将其 _alpha_ 设置为所需的量。_alpha_ 设为 1 将使材料完全不透明，0 的将会完全透明而不可见。

你也可以制作 _emissive 发光_ 材料。发光材料能自己发光。请注意，在渲染时，实际上它们并不能照亮场景中的附近物体，似乎只是在它们周围有模糊的光晕。

下图显示了使用标准材料创建的两个相同模型。 左侧的材料仅使用不透明材料，右侧的材料在其某些部分使用透明和发光材料。

![](/images/media/materials_transparent_emissive.png)

#### 纹理

纹理可以嵌入到导出的 glTF 文件中，也可以从外部文件中引用。 这两种方式都是支持的。

<!--

There are different kinds of textures you can use in a 3D model:

- albedo textures: don't use light
- alpha textures: determine only the transparency regions and its degree
- bump texture: Stores surface normal data used to displace a mesh in a texture. Used with BPR.
- emisiveTexture
- refractionTexture



link to content guide to show how you set materials for primitives

what extensions are supported for image files?
anything special to use alpha

what special layers PBR uses?

show how to change a model with an unsopported shader. Delete material, create new and assign the same texture it used to have

-->

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

#### 材质的最佳实践

- 如果场景中包含有多个使用相同纹理的模型，请将纹理引用为外部文件，而不是将其嵌入到 3D 模型中。因为嵌入的纹理会在每个模型中复制，从而增加场景的大小。
   > 注意：在引用未嵌入的纹理的文件后，请确保不会移动或重命名该文件，否则将丢失对该文件的引用。 该文件也必须位于场景文件夹中，以便与场景一起上传。
- 阅读[本文](https://www.khronos.org/blog/art-pipeline-for-gltf)，详细了解在 glTF 模型中使用 PBR 纹理的完整流程。
- 在[cgbookcase](https://cgbookcase.com/) 上可以找到免费高质量 PBR 纹理。


## 网格

3D 模型是由三角形的 _面_ 组成的 _网格_。 这些面交叉形成 _边_（面跟面交叉形成的线）和 _点_ 。

#### 平滑形状

您可以将网格配置为 _smooth_。这告诉引擎在渲染形状时，如同是用无数个中间面围绕形成的。此设置可以极大地帮助您减少圆角形状的三角形数量。

下图显示了具有相同材质的两个相同模型。不同之处在于，右侧的大部分 geometry 几何体设为 _smooth_。

![](/images/media/meshes_smooth_vs_sharp.png)

现在可以注意到，在左侧的模型的圆柱形状上能看到不同的面，但右侧的模型没有。

可以在模型的各个 _faces 面_，_edges 边_ 和 _vertices 点_ 上单独配置此设置。同个模型可以将其某些面或边设置为 _smooth_，而将其他面设置为 _sharp_

#### 形状的最佳实践

- 注意您添加到 3D 模型中的面数，因为更多的面它的渲染要求更高。有关场景的限制，请参阅[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})。
- 确保没有隐藏的面，它们虽然无法看到，但一样会增加三角形数量。
- 对于应具有圆边的形状，请将它们设置为 _smooth_，而不是添加更多的面。
- 确保所有面的 _normals 法线_ 朝外而不是朝内。在渲染时如果发现模型中有的面似乎不存在，则很可能是这个原因。

## Colliders 碰撞

要启用 3D 模型与场景用户之间的碰撞，必须创建一个新对象作为 collider 。如果没有 collider，用户就可以如同模型不存在一样穿越过模型。出于性能原因，Colliders 通常比模型本身的几何形状简单得多。

Colliders 目前不会影响模型和实体之间的相互作用，它们可以重叠。Colliders 仅影响模型与用户游戏化身的交互方式。

要被 Decentraland 场景识别为 collider 对象，只需要以某种方式命名。即对象的名称必须在末尾包含后缀 “\_collider”。

例如，要为树创建 collider ，可以在其主干周围创建一个简单的方框对象。场景的用户不会看到此框，但系统会阻止他们的前进。

![](/images/media/collision-tree.png)

在这种情况下，我们可以将方框命名为“Box*Tree_collider”，并将树和方框导出为单个 \_gltf* 模型文件。 \_collider 标签告诉 Decentraland 的虚拟世界引擎，方框对象属于 collider 集合，使得 \__collider 网格不可见。

![](/images/media/collision-hierarchy.png)

每当玩家在场景中查看树模型时，他们都会看到树的复杂模型。然而，当他们走近树时，他们就会与方框相撞，而不是树。

#### 楼梯的 Collider 对象

楼梯是 Collider 对象非常常见的用例。用户爬楼梯时，必须有一个用户可以踩到的对应的 \_collider 对象。

我们建议您将楼梯 Collider 设为斜坡物体，这样可以在上下行走时提供更好的体验。当他们爬上你的楼梯时，它会显得平滑上升或下降，而不是要他们毎步“跳跃”着走。

使用斜坡对象还可以避免创建不必要的几何图形，从而为其他更复杂的模型节省空间。请记住，在计算[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})时会考虑 Collider 几何体

1. 创建一个类似于原始楼梯的大小和比例的斜坡形状的新对象。

![](/images/media/collision-stairs-both.png)

2. 将渐变对象命名为类似于 _stairs_collider_ 。 必须以 \__collider_ 结尾。

3. 将斜坡物体叠加到楼梯上，使它们占据相同的空间。

![](/images/media/collision-stairs-collider.png)

4. 将两个对象一起导出为单个_glTF_模型。

![](/images/media/collision-stairs.png)

现在，当用户在你的场景中查看楼梯时，他们会看到更精细的楼梯模型，但是当他们爬楼梯时，他们会与斜坡相撞。

#### collider 的最佳实践

- 创建 collider 时，始终使用最少数量的三角形。避免复制复杂对象用作 collider。简单的 collider 可确保良好的用户体验并使您的场景保持在三角形限制之内。

- 碰撞对象不能有任何材质，因为用户永远不会看到它。collider 对用户是不可见的。

> 注意：请记住，每个场景受限于 log2（n + 1）x 10000 个三角形，其中 n 是场景中地块的数量。

- 所有 collider 对象名称必须以 \__collider_结尾。例如，_tree_collider_。

- 复制 collider 对象时，请注意其名称。有些程序会在文件名的末尾附加一个 \__1_ 以避免重复，例如_tree_collider_1_。像这样命名的物体将被 Decentraland 的虚拟世界引擎解释为普通物体，而不是 collider。

- 要查看 Decentraland 场景中所有 collider 的限制网格，请使用 `dcl start` 启动场景预览，然后单击 `c`。会显示绘制的蓝色线条，用于区分所有 collider 的位置。

## 动画

可以使用骨骼动画在 Decentraland 场景中对 3D 模型进行动画处理。 3D 模型的所有动画必须嵌入其 _glTF_ 文件中，您不能在单独的文件中引用动画。

目前，不支持不基于 armatures （骨架）的其他形式的动画。

关于动画必须具有的名称没有具体规则。您可以通过使用文本编辑器打开 _.gltf_ 文件的内容来验证导出模型中动画的名称。通常，动画名称由其 armature 名称，下划线及其动画名称组成。例如`myArmature_animation1`。

您可以在 _glTF 模型中包含任意数量的动画。在将模型加载到 Decentraland 场景时，默认情况下，_glTF_ 模型中的所有动画都是不可用的。有关如何激活和处理场景中动画的说明，请参阅[场景内容指南]({{ site.baseurl }}{% post_url /development-guide/2018-01-21-scene-content %})。

> 注意：目前无法更改场景中显示的动画的帧速率，速度固定为默认设置。要更改动画的速度，您必须更改帧数。

在 Decentraland 场景中，您可以使用 `weight` 来混合多个动画或使动画更精细。

> 提示：您也可以下载通用动画并将其应用于模型，而不是创建自己的动画。例如，对于具有类似人类特征的 3D 角色，您可以从 [Mixamo](https://www.mixamo.com/#/) 下载免费或付费动画。

#### 创建动画

您可以使用 Blender 之类的工具为 3D 模型创建动画。

1. 根据模型的形状和要移动的部分来创建 armature (骨架) 。你要做的是添加一个初始骨骼然后从这个骨骼顶点伸出其它骨骼。armature (骨架)中的骨头定义可以铰接的点。骨架必须与网格重叠。

    ![](/images/media/armature_hummingbird1.png)

2. 使得骨架和网络为同一物体的子资源。

3. 按照您计划移动的方式旋转骨骼时，检查网格是否自然移动。如果网格的某些部分以不希望的方式拉伸，请使用 weight paint 来更改模型的哪些部分受到骨架中骨骼的影响。

    ![](/images/media/animations_hummingbird_wp1.png)

    ![](/images/media/animations_hummingbird_wp2.png)

> 注意：Babylon.js 报告了一个错误，网格的某些面没有渲染当它们与骨架中的任何骨骼无关时。因此，如果您绘制一些weight 为 0 的面，然后为模型设置动画，您可能会看到这些面消失。为了解决这个问题，我们建议确保每个面至少与骨架的一个骨骼相关，并且 weight 至少为 0.01。

4. 将骨架移动到所需姿势，所有骨骼都可以旋转或缩放。然后锁定你要控制的动画的骨骼的旋转和比例。

    ![](/images/media/armature_hummingbird2.png)

5. 切换到动画中的其他帧，将骨架定位到新姿势并再次锁定。对要设置的所有关键帧重复此过程以描述动画。

    ![](/images/media/armature_hummingbird_animation.png)

6. 默认情况下，您定义的所有帧之间将从一个姿势线性转换到下一个姿势。您还可以将这些过渡配置为 exponentially, ease-in, bounce 等。

#### 在 Blender 中处理多个动画

在 Blender 中，要在一个模型中导出多个动画，需要在 _Dope-Sheet_ 动画摄影表创建多个 _actions 动作_。

![](/images/media/blender-dope-sheet.png)

您也可以在 Dope-Sheet 动画摄影表视图中编辑动画，例如您可以调整调整两个关键帧之间的距离。

要预览不同的动作，请打开 _Action Editor 动作编辑器_（只有在 Dope Sheet 动画摄影表中才能访问）。

![](/images/media/blender-action-editor.png)

要导出多个动画，您需要使用 _NLA Editor_ 存储所有操作。 我们建议在单独的编辑器选项卡上打开 NLA 编辑器，同时保持 Dope Sheet 动画摄影表 也打开。

![](/images/media/blender-nla-editor.png)

在 NLA 编辑器中，选择要嵌入 glTF 模型的每个动作，然后单击 _Stash_。

![](/images/media/blender-nla-editor2.png)

将模型添加到 Decentraland 场景时，必须通过配置 _gltf-model_ 实体来激活动画。 有关说明，请参阅[场景内容指南]({{ site.baseurl }}{% post_url /development-guide/2018-01-21-scene-content %})。

#### 动画的最佳实践

- 保持 armature 骨架简单，仅为要制作动画的模型部分创建骨骼。
- 如果动画将在场景中循环，请确保最终姿势与起始姿势相同，以避免引起跳跃感。
- 有时在动画中你可能只想控制骨架部分的运动，并保留其他骨骼未定义。这可以更容易地将动画组合在一起。
- 场景中的动画角色不应该完全静止，即使他们没有做任何事情。最好创建一个“空闲”动画，以便于角色静止时使用。“空闲”动画可以包括一些细微的动作，比如呼吸和偶尔的一瞥。
- 确保模型在导出时只有一个骨架。有时，当您将另一个动画导入到您正在编辑模型的程序时，它会引入一个骨架的副本。您希望模型的所有动画都由相同的基础骨架执行。
- 导出 _glTF_ 模型时，请确保导出所有对象和动画。某些导出器默认只导出 _当前选择的物体_。