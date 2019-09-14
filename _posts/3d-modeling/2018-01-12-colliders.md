---
date: 2018-01-11
title: 碰撞
description: 如何将碰撞添加到导入 Decentraland 的 3D 模型中。

categories:
  - 3d-modeling
type: Document
set: 3d-modeling
set_order: 10
---

要启用 3D 模型与场景用户之间的碰撞，必须创建一个新对象作为 collider 。如果没有 collider，用户就可以如同模型不存在一样穿越过模型。出于性能原因，Colliders 通常比模型本身的几何形状简单得多。

Colliders 目前不会影响模型和实体之间的相互作用，它们可以重叠。Colliders 仅影响模型与用户游戏化身的交互方式。

要被 Decentraland 场景识别为 collider 对象，只需要以某种方式命名。即对象的名称必须在末尾包含后缀 “\_collider”。

例如，要为树创建 collider ，可以在其主干周围创建一个简单的方框对象。场景的用户不会看到此框，但系统会阻止他们的前进。

<img src="/images/media/collision-tree.png" alt="Entity tree" width="500"/>

在这种情况下，我们可以将方框命名为“Box*Tree_collider”，并将树和方框导出为单个 \_gltf* 模型文件。 \_collider 标签告诉 Decentraland 的虚拟世界引擎，方框对象属于 collider 集合，使得 \__collider 网格不可见。

<img src="/images/media/collision-hierarchy.png" alt="Entity tree" width="350"/>

每当玩家在场景中查看树模型时，他们都会看到树的复杂模型。然而，当他们走近树时，他们就会与方框相撞，而不是树。

#### 如何为楼梯添加 Collider

楼梯是 Collider 对象非常常见的用例。用户爬楼梯时，必须有一个用户可以踩到的对应的 \_collider 对象。

我们建议您将楼梯 Collider 设为斜坡物体，这样可以在上下行走时提供更好的体验。当他们爬上你的楼梯时，它会显得平滑上升或下降，而不是要他们毎步“跳跃”着走。

使用斜坡对象还可以避免创建不必要的几何图形，从而为其他更复杂的模型节省空间。请记住，在计算[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})时会考虑 Collider 几何体

1. 创建一个类似于原始楼梯的大小和比例的斜坡形状的新对象。

   <img src="/images/media/collision-stairs-both.png" alt="Staircase mesh and collider side by side" width="300"/>

2. 将渐变对象命名为类似于 _stairs_collider_ 。 必须以 \__collider_ 结尾。

3. 将斜坡物体叠加到楼梯上，使它们占据相同的空间。

   <img src="/images/media/collision-stairs-collider.png" alt="Overlaid mesh and collider" width="300"/>

4. 将两个对象一起导出为单个_glTF_模型。

   <img src="/images/media/collision-stairs.png" alt="Exported 3D model with invisible collider" width="300"/>

现在，当用户在你的场景中查看楼梯时，他们会看到更精细的楼梯模型，但是当他们爬楼梯时，他们会与斜坡相撞。

#### 碰撞的最佳实践

- 创建 collider 时，始终使用最少数量的三角形。避免复制复杂对象用作 collider。简单的 collider 可确保良好的用户体验并使您的场景保持在三角形限制之内。
- 碰撞对象不能有任何材质，因为用户永远不会看到它。collider 对用户是不可见的。
  > 注意：请记住，每个场景受限于 log2（n + 1）x 10000 个三角形，其中 n 是场景中地块的数量。
- 所有 collider 对象名称必须以 \__collider_结尾。例如，_tree_collider_。
- 如果使用 _plane_ 作为 collider，它只能在一个方向上阻挡用户。 如果您希望 collider 可以在两侧阻挡，例如墙壁，则需要创建两个 _plane_，其法线朝向相反的方向。

- 复制 collider 对象时，请注意其名称。有些程序会在文件名的末尾附加一个 \__1_ 以避免重复，例如_tree_collider_1_。像这样命名的物体将被 Decentraland 的虚拟世界引擎解释为普通物体，而不是 collider。
- 要查看 Decentraland 场景中所有 collider 的网格限制，请使用 `dcl start` 启动场景预览，然后单击 `c`。会显示绘制的蓝色线条，用于区分所有 collider 的位置。

- 如果添加与场景中的 3D 模型重叠的不可见基本形状，则可以不用添加 collider 网格。 默认情况下，原始形状使用了碰撞。 例如，具有 BoxShape() 组件的实体，也可将其 `visible` 属性设置为 false 来解决这个问题。