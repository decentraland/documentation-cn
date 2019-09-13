---
date: 2018-01-13
title: 动画
description: 如何创建可嵌入的导入 Decentraland 的 3D 模型的动画。

categories:
  - 3d-modeling
type: Document
set: 3d-modeling
set_order: 10
---


可以使用 animations 在 Decentraland 场景中对 3D 模型进行动画处理。 3D 模型的所有动画必须嵌入其 _glTF_ 文件中，您不能在单独的文件中引用动画。

大多数3D模型动画都是[骨骼动画](https://en.wikipedia.org/wiki/Skeletal_animation)。 这些动画将模型的复杂几何形状简化为“剪贴画”，将网格中的每个顶点链接到骨架中最近的骨骼。 建模者将骨架调整为不同的姿势，网格伸展和弯曲以跟随这些运动。

作为一种替代方法，_vertex 动画_不需要骨架。这些动画直接指定模型中每个顶点的位置。Decentraland 也支持这些动画。

关于动画必须具有的名称没有具体规则。您可以通过使用文本编辑器打开 _.gltf_ 文件的内容来验证导出模型中动画的名称。通常，动画名称由其 armature 名称，下划线及其动画名称组成。例如`myArmature_animation1`。

您可以在 _glTF 模型中包含任意数量的动画。在将模型加载到 Decentraland 场景时，默认情况下，_glTF_ 模型中的所有动画都是不可用的。有关如何激活和处理场景中动画的说明，请参阅[3D 模型动画]({{ site.baseurl }}{% post_url /development-guide/2018-02-13-3d-model-animations %})。

在 Decentraland 场景中，您可以使用 `weight` 来混合多个动画或使动画更精细。

> 提示：您也可以下载通用动画并将其应用于模型，而不是创建自己的动画。例如，对于具有类似人类特征的 3D 角色，您可以从 [Mixamo](https://www.mixamo.com/#/) 下载免费或付费动画。

本文档介绍了如何将动画添加到 3D 模型中。 有关如何激活和处理场景中动画，请参阅[使用动画]({{ site.baseurl }}{% post_url /development-guide/2018-02-13-3d-model-animations %})。

#### 如何创建动画

您可以使用 Blender 之类的工具为 3D 模型创建动画。

1. 根据模型的形状和要移动的部分来创建 armature (骨架) 。你要做的是添加一个初始骨骼然后从这个骨骼顶点伸出其它骨骼。armature (骨架)中的骨头定义可以铰接的点。骨架必须与网格重叠。

   <img src="/images/media/armature_hummingbird1.png" alt="Armature" width="300"/>

2. 使得骨架和网络为同一物体的子资源。

3. 按照您计划移动的方式旋转骨骼时，检查网格是否自然移动。如果网格的某些部分以不希望的方式拉伸，请使用 weight paint 来更改模型的哪些部分受到骨架中骨骼的影响。

    <img src="/images/media/animations_hummingbird_wp1.png" alt="Weight paint view for one bone" width="300"/>

    <img src="/images/media/animations_hummingbird_wp2.png" alt="Weight paint view for another bone" width="300"/>

> 注意：Babylon.js 报告了一个错误，网格的某些面没有渲染当它们与骨架中的任何骨骼无关时。因此，如果您绘制一些weight 为 0 的面，然后为模型设置动画，您可能会看到这些面消失。为了解决这个问题，我们建议确保每个面至少与骨架的一个骨骼相关，并且 weight 至少为 0.01。

4. 将骨架移动到所需姿势，所有骨骼都可以旋转或缩放。然后锁定你要控制的动画的骨骼的旋转和比例。

   <img src="/images/media/armature_hummingbird2.png" alt="Shifted armature" width="300"/>

5. 切换到动画中的其他帧，将骨架定位到新姿势并再次锁定。对要设置的所有关键帧重复此过程以描述动画。

   <img src="/images/media/armature_hummingbird_animation.png" alt="Frames in animation" width="450"/>

6. 默认情况下，您定义的所有帧之间将从一个姿势线性转换到下一个姿势。您还可以将这些过渡配置为 exponentially, ease-in, bounce 等。

#### 如何在 Blender 中处理多个动画

在 Blender 中，要在一个模型中导出多个动画，需要在 _Dope-Sheet_ 动画摄影表创建多个 _actions 动作_。

<img src="/images/media/blender-dope-sheet.png" alt="Open dope sheet" width="250"/>

您也可以在 Dope-Sheet 动画摄影表视图中编辑动画，例如您可以调整调整两个关键帧之间的距离。

要预览不同的动作，请打开 _Action Editor 动作编辑器_（只有在 Dope Sheet 动画摄影表中才能访问）。

<img src="/images/media/blender-action-editor.png" alt="Open action editor" width="250"/>

要导出多个动画，您需要使用 _NLA Editor_ 存储所有操作。 我们建议在单独的编辑器选项卡上打开 NLA 编辑器，同时保持 Dope Sheet 动画摄影表 也打开。

<img src="/images/media/blender-nla-editor.png" alt="Open NLA editor" width="250"/>

在 NLA 编辑器中，选择要嵌入 glTF 模型的每个动作，然后单击 _Stash_。

<img src="/images/media/blender-nla-editor2.png" alt="Stash actions into glTF model" width="600"/>

将模型添加到 Decentraland 场景时，必须通过配置 _gltf-model_ 实体来激活动画。 有关说明，请参阅[3D 模型动画]({{ site.baseurl }}{% post_url /development-guide/2018-02-13-3d-model-animations %})。

#### 动画的最佳实践

- 保持 armature 骨架简单，仅为要制作动画的模型部分创建骨骼。
- 如果动画将在场景中循环，请确保最终姿势与起始姿势相同，以避免引起跳跃感。
- 有时在动画中你可能只想控制骨架部分的运动，并保留其他骨骼未定义。这可以更容易地将动画组合在一起。
- 场景中的动画角色不应该完全静止，即使他们没有做任何事情。最好创建一个“空闲”动画，以便于角色静止时使用。“空闲”动画可以包括一些细微的动作，比如呼吸和偶尔的一瞥。
- 确保模型在导出时只有一个骨架。有时，当您将另一个动画导入到您正在编辑模型的程序时，它会引入一个骨架的副本。您希望模型的所有动画都由相同的基础骨架执行。
- 导出 _glTF_ 模型时，请确保导出所有对象和动画。某些导出器默认只导出 _当前选择的物体_。