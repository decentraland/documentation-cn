---
date: 2018-02-6
title: 形状组件
description: 为实体提供 3D 形状和碰撞的不同组件说明
categories:
  - development-guide
type: Document
set: development-guide
set_order: 6
---

Decentraland 中的三维场景是基于[实体 - 组件](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system)模型开发的，场景中的所有内容都是 _entity（实例）_，每个实体都可以包含形成其特征和功能的_components（组件）_。

实体渲染的形状由它使用的组件来决定。每个实体只能分配一个形状组件。

<img src="/images/media/ecs-simple-components.png" alt="nested entities" width="400"/>

## 基本形状

有几个基本形状可以将添加到实体中。

有以下几种基本形状组件：

- `BoxShape`
- `SphereShape`
- `PlaneShape`
- `CylinderShape`
- `ConeShape`

这些组件中的每一个都具有特定于该形状的某些属性，例如 _cylinder_ 圆柱形状具有`arc`, `radiusTop`, `radiusBottom` 等属性。

有关组件属性的更多详细信息，请参阅[组件参考](https://github.com/decentraland/ecs-reference)。

要将组件应用于实体，可以在一个操作中实例化一个新组件并将其分配到实例：

```ts
myEntity.addComponent(new SphereShape())
```

也可以先创建组件实例，然后将其分配给实体。


```ts
let shpere = new SphereShape()
myEntity.addComponent(sphere)
```

基本形状不包括材质。要为其指定颜色或纹理，可以将[材质组件]({{ site.baseurl }}{% post_url /development-guide/2018-02-7-materials %}) 添加给同一实体。

## 3D 模型

对于更复杂的形状，您可以使用 Blender 等外部软件工具构建 3D 模型，然后使用 _.glTF_ 或 _.glb_ (binary _.glTF_)导入。 [glTF](https://www.khronos.org/gltf)(GL 传输格式）是 Khronos 的一个开源项目，为 3D 素材提供了一种通用、可扩展的格式，它既高效又与现代网络技术具有高度的可互操作性。

将外部模型添加到场景中，可以使用 `GLTFShape` 组件，将其 `src` 设置为包含模型的 glTF 文件的路径。

```ts
myEntity.addComponent(new GLTFShape("models/House.gltf"))
```

由于 `src` 字段是必需的，因此在构造组件时必须赋值。

在上面的示例中，模型位于场景项目根目录下的 `models` 文件夹中。

> 提示：我们建议您将模型与其它文件分开，放在场景中的 `/models` 文件夹中。

在 glTF 模型可以嵌入纹理、材质、碰撞器和动画。有关详细信息，请参阅[3D 模型]({{ site.baseurl }}{% post_url /3d-modeling/2018-01-09-3d-models %})。

请记住，所有模型、着色器和纹理都必须在[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})的参数范围内。

> 注意：还支持 _.obj_ 格式的 3D 模型，但不鼓励使用。_.obj_ 格式模型可使用 `OBJShape` 组件导入。请注意 _.obj_ 模型不支持动画和其他功能。

#### 免费的 3D 模型库

除了自己构建 3D 模型，您也可以从多个免费或付费库下载。

以下是免费或相对便宜的 3D 内容库：

- [Assets from the Builder](https://github.com/decentraland/builder-assets/tree/master/assets)
- [Google Poly](https://poly.google.com)
- [SketchFab](https://sketchfab.com/)
- [Clara.io](https://clara.io/)
- [Archive3D](https://archive3d.net/)
- [SketchUp 3D Warehouse](https://3dwarehouse.sketchup.com/)
- [Thingiverse](https://www.thingiverse.com/) (主要用于 3D 打印，但适用于虚拟世界)
- [ShareCG](https://www.sharecg.com/)

> 注意：请注意您下载的内容许可。

请注意，在其中一些站点中，您可以选择下载模型的格式。如果有的话，请选择 _.glTF_ 格式。如果没有，则必须先将它们转换为 _glTF_，然后才能在场景中使用它们。因此，我们建议将它们先导入 Blender ，然后用 _glTF_ 导出插件导出到 _.glTF_ 格式。

## Collisions 碰撞

启用 Collisions 的实体会占用空间且阻挡用户前进，如果实体没有启用碰撞，那么用户可以从实体上直接穿越过去。

当前碰撞的设置并不会影响其他实体彼此之间的交互，实体可以重叠。碰撞设置仅影响实体与用户的交互方式。

Decentraland 目前没有物理引擎，因此如果实体需要掉落、碰撞或反弹功能，则必须在场景代码中编写相应代码。

> 提示：要查看场景中所有碰撞网格的限制，请使用 `dcl start` 启动场景预览，然后单击 `c`。预览会用蓝线绘制出相应的碰撞网格。

默认情况下，实体不启用碰撞。根据形状组件的类型，可以按如下方式启用碰撞：

- 对于_基本_形状（如长方体，球体，平面等），可以将形状组件的 `withCollisions` 字段设置为 _true_ 来启用碰撞。

此示例定义了一个无法通过的长方体实体。

  ```ts
  let box = new BoxShape()
  box.withCollisions = true
  myEntity.addComponent(box)
  ```

> 注意：Planes 仅阻止一个方向的移动。

- 要在 _glTF_ 形状中启用碰撞，您可以：

  - 外面套一个基本形状的不可见实体，并将其 `withCollisions` 字段设置为 _true_。
  - 使用 Blender 等外部工具编辑模型，加一个 _collider 对象_。collider 必须命名为 _x_collider_，其中_x_是模型的名称。因此，对于名为 _house_ 的模型，collider 必须命名为 _house_collider_。

_collider_ 可以是一组几何形状或平面，用来定义模型的哪些部分是碰撞的。这样可以有更多的控制并且对系统的要求也低得多，因为碰撞对象通常比原始模型更简单（具有更少的顶点）。

有关如何将 colliders 添加到 3D 模型的更多详细信息，请参阅[3D模型]({{ site.baseurl }}{% post_url /3d-modeling/2018-01-09-3d-models %})。

## 不可见实体

可以在其形状组件中设置 `visible` 字段来使实体不可见。在将形状作为碰撞使用时，这个操作尤其有用。

默认情况下，基本形状和 3D 模型的所有组件都是可见的。

```ts
const myEntity = new Entity()
myEntity.addComponent(new BoxShape())
myEntity.getComponent(BoxShape).visible = false
```

## 优化 3D 模型

要确保场景中的 3D 模型加载速度更快，占用的内存更少，请遵循以下最佳实践：

- 以 _.glb_ 格式保存模型，这是 _.gltf_ 格式的轻量版本。
- 如果您有多个共享相同纹理的模型，请将纹理导出到一个单独的文件中。这样多个模型可以使用仅需要加载一次的单个纹理文件。
- 如果有多个实体使用相同 3D 模型，可以仅实例化一个 `GLTFShape` 组件，然后将相同的组件分配给需要使用它的实体。
- 如果您的场景中有实体经常显示和消失，最好将这些实体集中在一起，保持已定义，然后在需要时从引擎中删除。这有助于更快显示，缺点是在不使用时它们也会占用内存。请参阅[实体和组件]({{ site.baseurl }}{% post_url /development-guide/2018-02-1-entities-components %}#pooling-entities-and-components)

## 复用形状

如果场景中的多个实体使用相同的基本形状或 3D 模型，则无需为每个实体分别创建形状组件的实例。所有实体可以共享一个相同的实例。

这样做可以让场景显得更轻量，并且可以防止您超出每个场景最多 _bodies 物体_ 的[限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})。

> 注意：复用的形状将添加到场景的 _triangle 三角形_ 计数中。因此可以通过复用形状来避免最多三角形数的限制。

```ts
// Create shape component
const house = new GLTFShape("models/House.gltf")

// Create entities
const myEntity = new Entity()
const mySecondEntity = new Entity()
const myThirdEntity = new Entity()

// Assign shape component to entities
myEntity.addComponent(house)
mySecondEntity.addComponent(house)
myThirdEntity.addComponent(house)
```

共享形状的每个实体可以应用不同的比例、旋转或甚至材质（在基本形状的情况下），而不影响其他实体的呈现方式。

共享使用同一个 3D 模型实例的实体也可以有独立运行的动画。每个组件必须有一个单独的 `Animator` 组件，以及单独的 `AnimationState` 对象，以跟踪当前正在播放的动画的哪个部分。参见[3D 模型动画]({{ site.baseurl }}{% post_url /development-guide/2018-02-13-3d-model-animations %})