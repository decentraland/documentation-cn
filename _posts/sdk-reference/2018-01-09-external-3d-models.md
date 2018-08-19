---
date: 2018-01-09
title: 3D 模型考虑因素
description: 外部 3D 模型支持哪些资产和组件，将它们导入 Decentraland 之前如何配置
categories:
  - sdk-reference
type: Document
set: sdk-reference
set_order: 9
---

3D models are imported into decentraland in glTF format. There are a number of supported features that these models can include. This section goes over ways to make them compatible with Decentraland and best practices.

3D 模型以 glTF 格式导入到 decentraland 中。这些模型可以包含许多支持的功能。本节介绍如何使它们与Decentraland 兼容及最佳实践。

See [Scene content guide]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %}) for information on how you can configure a 3D model in a Decentraland scene to set its position, scale, activate its animations, etc.

有关如何在 Decentraland 场景中配置 3D 模型的位置，比例，激活动画等的信息，请参阅[场景内容指南]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %})。

Keep in mind that all models, their shaders and their textures must be within the parameters of the [scene limitations]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-06-scene-limitations %}).

请记住，所有模型，着色器和纹理都必须在[场景限制]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-06-scene-limitations %})的参数范围内。

## Supported 3D model formats

## 支持的 3D 模型格式

All 3D models in Decentraland must be in glTF format. [glTF](https://www.khronos.org/gltf) (GL Transmission Format) is an open project by Khronos providing a common, extensible format for 3D assets that is both efficient and highly interoperable with modern web technologies.

Decentraland 中的所有 3D 模型都必须采用 glTF 格式。 [glTF](https://www.khronos.org/gltf) (GL Transmission Format) 是 Khronos 的一个开源项目，为 3D 资产提供了一种通用的，可扩展的格式，既高效又可与现代网络技术进行高度互操作。

glTF models can have either a _.gltf_ or a _.glb_ extension. glTF files are human-readable, you can open one in a text editor and read it like a JSON file. This is useful, for example, to verify that animations are properly attached and to check for their names. glb files are binary, so they're not readable but they are considerably smaller in size, which is good for the scene's performance.

glTF 模型可以具有 _.gltf_ 或 _.glb_ 扩展名。 glTF 文件是可读的，您可以在文本编辑器中打开它并像 JSON 文件一样读取它。例如，可以用来验证动画是否已正确附加并检查其名称。 glb 文件是二进制文件，所以它们不可读，但是文件要小得多，这对场景的性能有好处。

We recommend using _.gltf_ while you're working on a scene, but then switching to _.glb_ when uploading it.

我们建议您在处理场景时使用 _.gltf_，但在上传时切换到 _.glb_。

> Note: When using Blender to create or edit 3D models, you need an add-on to export glTF files. For models that don't include animations we recommend installing the add-on by [Kronos group](https://github.com/KhronosGroup/glTF-Blender-Exporter). To export glTFs that include animations, you should instead install the add-on by [Kupoman](https://github.com/Kupoman/blendergltf).

> 注意：使用 Blender 创建或编辑 3D 模型时，需要一个加载项来导出 glTF 文件。对于不包含动画的模型，我们建议您安装 [Kronos group](https://github.com/KhronosGroup/glTF-Blender-Exporter) 附加组件。要导出包含动画的 glTF，您应该安装 [Kupoman](https://github.com/Kupoman/blendergltf) 附加组件。

#### Why we use glTF

#### 为什么我们使用 glTF

Compared to the older _OBJ format_, which supports only vertices, normals, texture coordinates, and basic materials,

与旧的只支持顶点，法线，纹理坐标和基本材质的 _OBJ format_ 格式相比，

glTF provides a more powerful set of features that includes:

glTF 提供了一组更强大的功能，包括：

- Hierarchical objects

- 分层对象

- Skeletal structure and animation

- 骨骼结构和动画

- More robust materials and shaders

- 更强大的材质和着色器

- Scene information (light sources, cameras)

- 场景信息（光源，相机）

> Note: _.obj_ models are supported in Decentraland scenes as a legacy feature, but will likely not be supported for much longer.

> 注意：作为遗留功能，Decentraland 场景支持 _.obj_ 模型，但以后可能不再支持。

Compared to _COLLADA_, the supported features are very similar. However, because glTF focuses on providing a "transmission format" rather than an editor format, it is more interoperable with web technologies.

与 _COLLADA_ 相比，支持的功能非常相似。但是，因为 glTF 专注于提供“传输格式”而不是编辑器格式，它与 Web 技术更具互操作性。

Consider this analogy: the .PSD (Adobe Photoshop) format is helpful for editing 2D images, but images must then be converted to .JPG for use
on the web. In the same way, COLLADA may be used to edit a 3D asset, but glTF is a simpler way of transmitting it while rendering the same result.

考虑这个类比：.PSD（Adobe Photoshop）格式有助于编辑 2D 图像，但必须将图像转换为 .JPG 才能使用。同样，在网络上，COLLADA 可用于编辑 3D 资源，但在传输的同时进行渲染 glTF 上是一种更简单的方式。

## Colliders

## Colliders

To enable collisions between a 3D model and users of your scene, you must create a new object to serve as a collider. Without a collider, users are able to walk through models as if they weren't there. For performance reasons, colliders usually have a much simpler geometry than the model itself.

要启用 3D 模型与场景用户之间的碰撞，必须创建一个新对象作为 collider 。如果没有 collider，用户就可以如同模型不存在一样走过模型。出于性能原因，Colliders 通常比模型本身的几何形状简单得多。

Colliders currently don't affect how models and entities interact with each other, they can always overlap. Colliders only affect how the model interacts with the user's avatar.

Colliders 目前不会影响模型和实体之间的相互作用，它们可以重叠。Colliders 仅影响模型与用户游戏化身的交互方式。

For an object to be recognized by a Decentraland scene as a collider, all it needs is to be named in a certain way. The object's name must include the the suffix “\_collider” at the end.

要被 Decentraland 场景识别为 collider 对象，只需要以某种方式命名。即对象的名称必须在末尾包含后缀 “\_collider”。

For example, to create a collider for a tree, you can create a simple box object surrounding its trunk. Users of the scene won't see this box, but it will block their path.

例如，要为树创建 collider ，可以在其主干周围创建一个简单的方框对象。场景的用户不会看到此框，但系统会阻止他们的前进。

![](/images/media/collision-tree.png)

In this case, we can name the box "Box*Tree_collider" and export both the tree and the box as a single \_gltf* model. The \_collider tag alerts the Decentraland world engine that the box object belongs to the collection of colliders, making the \_collider mesh invisible.

在这种情况下，我们可以将方框命名为“Box*Tree_collider”，并将树和方框导出为单个 \_gltf* 模型文件。 \_collider 标签告诉 Decentraland 的虚拟世界引擎，方框对象属于 collider 集合，使得 \__collider 网格不可见。

![](/images/media/collision-hierarchy.png)

Whenever a player views the tree model in your scene, they will see the complex model for your tree. However, when they walk into your tree, they will collide with the box, not the tree.

每当玩家在场景中查看树模型时，他们都会看到树的复杂模型。然而，当他们走近树时，他们就会与方框相撞，而不是树。

#### Collider objects for stairs

#### 楼梯的 Collider 对象

Stairs are a very common use-case for collider objects. In order for users to climb stairs, there must be a corresponding \_collider object that the users are able to step on.

楼梯是 Collider 对象非常常见的用例。用户爬楼梯时，必须有一个用户可以踩到的对应的 \_collider 对象。

We recommend using a ramp object for your stair colliders, this provides a much better experience when walking up or down. When they climb up your stairs, it will appear as a smooth ascent or descent, instead of requiring them to “jump” up each individual step.

我们建议您将楼梯 Collider 设为斜坡物体，这样可以在上下行走时提供更好的体验。当他们爬上你的楼梯时，它会显得平滑上升或下降，而不是要他们毎步“跳跃”着走。

Using a ramp object also avoids creating unnecessary geometry, saving room for other more complicated models. Keep in mind that collider geometry is also taken into account when calculating the [scene limitations]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-06-scene-limitations %})

使用斜坡对象还可以避免创建不必要的几何图形，从而为其他更复杂的模型节省空间。请记住，在计算[场景限制]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-06-scene-limitations %})时会考虑 Collider 几何体

1.  Create a new object in the shape of a ramp that resembles the size and proportions of the original stairs.

1. 创建一个类似于原始楼梯的大小和比例的斜坡形状的新对象。

![](/images/media/collision-stairs-both.png)

2.  Name the ramp object something similar to _stairs_collider_. It must end in \__collider_.

2. 将渐变对象命名为类似于 _stairs_collider_ 。 必须以 \__collider_ 结尾。

3.  Overlay the ramp object to the stairs so that they occupy the same space.

3. 将斜坡物体叠加到楼梯上，使它们占据相同的空间。

![](/images/media/collision-stairs-collider.png)

4.  Export both objects together as a single _glTF_ model.

4. 将两个对象一起导出为单个_glTF_模型。

![](/images/media/collision-stairs.png)

Now when users view the stairs in your scene, they’ll see the more elaborate model of the stairs, but when they climb them, they’ll collide with the ramp.

现在，当用户在你的场景中查看楼梯时，他们会看到更精细的楼梯模型，但是当他们爬楼梯时，他们会与斜坡相撞。

#### Best practices with colliders

#### collider 的最佳实践

- Always use the smallest number of triangles possible when creating colliders. Avoid making a copy of a complex object to use as a collider. Simple colliders guarantee a good user-experience in and keep your scene within the triangle limitations.

- 创建 collider 时，始终使用最少数量的三角形。避免复制复杂对象用作 collider。简单的 collider 可确保良好的用户体验并使您的场景保持在三角形限制之内。

- Collider objects shouln't have any material, as users of your scene will never see it. Colliders are invisible to users.

- 碰撞对象不能有任何材质，因为用户永远不会看到它。collider 对用户是不可见的。

> Note: Remember that each scene is limited to log2(n+1) x 10000 triangles, where n is the number of parcels in your scene.

> 注意：请记住，每个场景受限于 log2（n + 1）x 10000 个三角形，其中 n 是场景中地块的数量。

- All collider objects names must end with \__collider_. For example, _tree_collider_.

- 所有 collider 对象名称必须以 \__collider_结尾。例如，_tree_collider_。

- When duplicating collider objects, pay attention to their names. Some programs append a \__1_ to the end of the filename to avoid duplicates, for example _tree_collider_1_. Objects that are named like this will be interpreted by the Decentraland World Engine as normal objects, not colliders.

- 复制 collider 对象时，请注意其名称。有些程序会在文件名的末尾附加一个 \__1_ 以避免重复，例如_tree_collider_1_。像这样命名的物体将被 Decentraland 的虚拟世界引擎解释为普通物体，而不是 collider。

- To view the limits of all collider meshes in a Decentraland scene, launch your scene preview with `dcl start` and then click `c`. This draws blue lines that delimit all colliders in place.

- 要查看 Decentraland 场景中所有 collider 的限制网格，请使用 `dcl start` 启动场景预览，然后单击 `c`。会显示绘制的蓝色线条，用于区分所有 collider 的位置。

## Animations

## 动画

3D models can be animated in a Decentraland scene using skeletal animations. All animations of a 3D model must be embedded inside its _glTF_ file, you can't reference animations in separate files.

可以使用骨骼动画在 Decentraland 场景中对 3D 模型进行动画处理。 3D 模型的所有动画必须嵌入其 _glTF_ 文件中，您不能在单独的文件中引用动画。

Currently, other forms of animations that aren't based on armatures are not supported.

目前，不支持不基于 armatures （骨架）的其他形式的动画。

There's no specific rule about the names animations must have. You can verify the names of the animations in an exported model by opening the contents of a _.gltf_ file with a text editor. Typically, an animation name is comprised of its armature name, an underscore and its animation name. For example `myArmature_animation1`.

关于动画必须具有的名称没有具体规则。您可以通过使用文本编辑器打开 _.gltf_ 文件的内容来验证导出模型中动画的名称。通常，动画名称由其 armature 名称，下划线及其动画名称组成。例如`myArmature_animation1`。

You can include any number of animations in a _glTF model_. All animations in a _glTF_ model are dissabled by default when loading the model into a Decentraland scene. See [Scene content guide]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %}) for instructions on how to activate and handle animations in a scene.

您可以在 _glTF 模型中包含任意数量的动画。在将模型加载到 Decentraland 场景时，默认情况下，_glTF_ 模型中的所有动画都是不可用的。有关如何激活和处理场景中动画的说明，请参阅[场景内容指南]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %})。

> Note: There currently isn't a way to change the frame rate of an animation displayed in your scene, the speed is fixed to a default setting. To change an animation's speed, you must change the number of frames.

> 注意：目前无法更改场景中显示的动画的帧速率，速度固定为默认设置。要更改动画的速度，您必须更改帧数。

In a Decentraland scene, you can use `weight` to blend several animations or to make an animation more subtle.

在 Decentraland 场景中，您可以使用 `weight` 来混合多个动画或使动画更精细。

> Tip: Instead of creating your own animations, you can also download generic animations and apply them to your model. For example, for 3D characters with human-like characteristics, you can download free or paid animations from [Mixamo](https://www.mixamo.com/#/).

> 提示：您也可以下载通用动画并将其应用于模型，而不是创建自己的动画。例如，对于具有类似人类特征的 3D 角色，您可以从 [Mixamo](https://www.mixamo.com/#/) 下载免费或付费动画。

#### Creating an animation

#### 创建动画

You can use a tool like Blender to create animations for a 3D model.

您可以使用 Blender 之类的工具为 3D 模型创建动画。

1.  Create an armature, following the shape of your model and the parts you wish to move. You do this by adding an initial bone and then extruding all other bones from the vertices of that one. Bones in the armature define the points that can be articulated. The armature must be positioned overlapping the mesh.

1. 根据模型的形状和要移动的部分来创建 armature (骨架) 。你要做的是添加一个初始骨骼然后从这个骨骼顶点伸出其它骨骼。armature (骨架)中的骨头定义可以铰接的点。骨架必须与网格重叠。

![](/images/media/armature_hummingbird1.png)

2.  Make both the armature and the mesh child assets of the same object.

2. 使得骨架和网络为同一物体的子资源。

3.  Check that the mesh moves naturally when rotating its bones in the ways you plan to move it. If parts of the mesh get stretched in undesired ways, use weight paint to change what parts of the model are affected by each bone in the armature.

3. 按照您计划移动的方式旋转骨骼时，检查网格是否自然移动。如果网格的某些部分以不希望的方式拉伸，请使用 weight paint 来更改模型的哪些部分受到骨架中骨骼的影响。

![](/images/media/animations_hummingbird_wp1.png)

![](/images/media/animations_hummingbird_wp2.png)

> Note: There's a reported bug with Babylon.js that prevents some faces of a mesh from being rendered when they're not related to any bone in the armature. So if you paint some faces with weight 0 and then animate the model, you might see these faces dissappear. To solve this, we recommend making sure that each face is related to at least one bone of the armature and painted with a weight of at least 0.01.

> 注意：Babylon.js 报告了一个错误，网格的某些面没有渲染当它们与骨架中的任何骨骼无关时。因此，如果您绘制一些weight 为 0 的面，然后为模型设置动画，您可能会看到这些面消失。为了解决这个问题，我们建议确保每个面至少与骨架的一个骨骼相关，并且 weight 至少为 0.01。

4.  Move the armature to a desired pose, all bones can be rotated or scaled. Then lock the rotation and scale of the bones you wish to control with this animation.

4. 将骨架移动到所需姿势，所有骨骼都可以旋转或缩放。然后锁定你要控制的动画的骨骼的旋转和比例。

![](/images/media/armature_hummingbird2.png)

5.  Switch to a different frame in the animation, position the armature into a new pose and lock it again. Repeat this process for all the key frames you want to set to describe the animation.

5. 切换到动画中的其他帧，将骨架定位到新姿势并再次锁定。对要设置的所有关键帧重复此过程以描述动画。

![](/images/media/armature_hummingbird_animation.png)

6.  By default all frames in between the ones you defined will transition linearly from one pose to the next. You can also configure these transitions to behave exponentially, ease-in, bounce, etc.

6. 默认情况下，您定义的所有帧之间将从一个姿势线性转换到下一个姿势。您还可以将这些过渡配置为 exponentially, ease-in, bounce 等。

To create several animations for the same model in Blender, you must select the Dope-Sheet view, and open the Action Editor. You can also edit the animation from the Dope-Sheet view, for example you can adjusting the distance between two key frames.

要在 Blender 中为同一模型创建多个动画，必须选择 “Dope-Sheet” 视图，然后打开“动作编辑器”。您也可以从“Dope-Sheet”视图编辑动画，例如，您可以调整两个关键帧之间的距离。

When adding the model to your Decentraland scene, you must activate animations by configuring the _gltf-model_ entity. See [Scene content guide]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %}) for instructions.

将模型添加到 Decentraland 场景时，必须通过配置 _gltf-model_ 实体来激活动画。有关说明，请参阅[场景内容指南]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %})。

#### Best practices for animations

#### 动画的最佳实践

- Keep the armature simple, only create bones for the parts of the model that you intend to animate.

- 保持 armature 骨架简单，仅为要制作动画的模型部分创建骨骼。

- If the animation will be looped in your scene, make sure the final pose is identical to the starting pose to avoid jumps.

- 如果动画将在场景中循环，请确保最终姿势与起始姿势相同，以避免引起跳跃感。

- Sometimes in an animation you might want to only control the movements of parts of the armature, and leave other bones undefined. This can make it easier to combine animations together.

- 有时在动画中你可能只想控制骨架部分的运动，并保留其他骨骼未定义。这可以更容易地将动画组合在一起。

- Animated characters in your scene shouldn't ever stay completely still, even when they aren't doing anything. It's best to create an "idle" animation to use for when the character is still. The idle animation can include subtle movements like breathing and perhaps occasional glances.

- 场景中的动画角色不应该完全静止，即使他们没有做任何事情。最好创建一个“空闲”动画，以便于角色静止时使用。“空闲”动画可以包括一些细微的动作，比如呼吸和偶尔的一瞥。

- Make sure your model only has one armature when you export it. Sometimes, when importing another animation to the program where you're editing your model, it will bring in a copy of the armature. You want all animations of the model to be performed by the same base armature.

- 确保模型在导出时只有一个骨架。有时，当您将另一个动画导入到您正在编辑模型的程序时，它会引入一个骨架的副本。您希望模型的所有动画都由相同的基础骨架执行。

- When exporting the _glTF_ model, make sure you're exporting all the objects and animations. Some exporters will only export the _currently selected_ by default.

- 导出 _glTF_ 模型时，请确保导出所有对象和动画。某些导出器默认只导出 _当前选择的物体_。

<!--

## Materials


textures can be embedded in the gltf or referenced from a file


albedo textures: don't use light

transparent materials, use alpha


- materials (todo lo que hay en content guide es lo que seteas en los entities)

Not all materials are supported by Decentraland.


hay dos tipos de matierol, “standard materials” que es lo mismo que “blender render”

- diffuse / specular
  y dps los PBR (phisically based rendering)

- textures
  aclarar tamaños
  capaz extensiones de archivos
  capaz alpha
  capas especiales para pbr

animations y materials son “assets” para unity
un collider es ponele un “component”

#### Best practices for materials

- If your scene includes several models that use the same texture, it's useful to reference the texture as an external file. If the texture was embedded, it would be duplicated and add to the scene's weight.




-->
