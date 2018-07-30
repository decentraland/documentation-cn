---
date: 2018-06-12
title: Scene content guide
description: Learn how to import 3D models to use in a scene
categories:
  - sdk-reference
type: Document
set: sdk-reference
set_order: 2
tag: introduction
---

Three dimensional scenes in Decentraland are based on the [Entity-Component](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system) model, where everything in a scene is an *entity*, and each entity can include *components* that shape its characteristics and functionality. 

Decentraland 中的三维场景是基于 [Entity-Component](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system) 模型的，场景中的所有内容都是由 *entity* 组成，每个 entity （实体）又可以包含 *components （组件）*，用来塑造其特性和功能。


Entities can be nested inside others to create a tree structure. In fact, all scenes must output a tree of nested entities. `.xml` scenes explicitly define this tree, `.xlt` scenes define typescript code that builds and updates this tree.

实体可以嵌套在其他实体中，形成树结构。实际上，所有场景的输出都是嵌套的实体树。 `.xml` 场景文件显式定义了这个树，`.xlt` 场景文件定义了构建和更新这个树的 typescript 代码。


This document covers how to achieve common objectives by using different types of entities and components in a scene's tree.

本文档介绍了如何使用场景树中的不同类型的实体和组件来实现基本对象。


## Create simple geometric shapes

## 创建简单的几何形状

Several basic shapes, often called *primitives*, can be added to a scene as predefined entity types. These already have certain components defined (like their shape) and let you set others (like their rotation and color).

几种基本形状（通常称为 *基本形体*）可以作为预定义的实体类型添加到场景中。它们具有已经定义好的组件（比如它们的形状），并可以设置其他组件（比如旋转和颜色）。


The following types of entities are available:

可以使用以下类型的实体：

* `<Box />`
* `<Sphere />`
* `<Plane />`
* `<Cylinder />`
* `<Cone />`

Any of these can be added to your scene, they can all include basic components like position, scale or color.

所有这些都可以添加到场景中，它们都可以包含基本组件，如位置，比例或颜色。


```tsx
<box position={vector} color="#ff00aa" scale={2} />
```

See [Entity interfaces]({{ site.baseurl }}{% post_url /sdk-reference/2018-06-21-entity-interfaces %}) for more details on these types of entities.

有关这些实体的更多详细信息，请参阅[实体接口]({{ site.baseurl }}{% post_url /sdk-reference/2018-06-21-entity-interfaces %}) 。


> Tip: When editing the code via a source code editor (like Visual Studio Code), you can see the list of components supported by a type of entity. Typically, this is done by placing the cursor in the entity and typing *Ctrl + Space bar*.

> 提示：通过源代码编辑器（如 Visual Studio Code）编辑代码时，您能看到毎种实体支持的组件列表。通常，可以把光标放在实体中并输入 *Ctrl + 空格* 来实现。


## Entity positioning
## 实体定位


All entities can have a position, a rotation and a scale. These can be easily set as components, as shown below:

所有实体都可以有一个位置，旋转和比例。这些可以很容易地设置为组件，如下所示：


{% raw %}
```tsx
<box
    position={{ x: 5, y: 3, z: 5 }}
    rotation={{ x: 180, y: 90, z: 0 }}
    scale={0.5}
  />
```
{% endraw %}

* `position` is a *3D vector*, it sets the position on all three axes. 
* `rotation` is a *3D vector* too, but where each component represents the rotation in that axis.
* `scale` can either be a *number* or a *3D vector*, in case you want to scale the axis in different proportions. 

* `position` *3D 矢量*，用来设置三个轴上的位置。
* `rotation` 也是 *3D 矢量*，但每个组件代表该轴的旋转。
* `scale` 可以是 *数字* 或 *3D矢量*，用于轴的不同缩放比例。


When an entity is nested inside another, the child entities inherit components from the parents. This means that if a parent entity is positioned, scaled or rotated, its children are also affected. The position, rotation and scale values of children entities don't override those of the parents, instead these are compounded.

当实体嵌套在另一个实体中时，子实体从父项继承组件。这意味着如果父实体被定位，缩放或旋转，其子项也会受到影响。子实体的位置，旋转和比例值不会覆盖父母的位置，旋转和比例值，而是由两者组合而成。

You can include an invisible base entity to wrap a set of other entities and define their positioning as a group.

您可以用一个不可见的基本实体来组合其他实体，并将它们作为一个组来定义位置。


{% raw %}
```tsx
  <entity
      position={{ x: 0, y: 0, z: 1 }}
      rotation={{ x: 45, y: 0, z: 0 }}
  >
    <box position={{ x: 10, y: 0, z: 0 }} scale={2} />
    <box position={{ x: 10, y: 10, z: 0 }} scale={1} />
    <box position={{ x: 0, y: 10, z: 0 }} scale={2} />
  </entity>
```
{% endraw %}

You can also set a position, rotation and scale for the entire <scene/> entity and affect everything in the scene.

您还可以为整个 <scene/> 实体设置位置，旋转和缩放，这将影响场景中的所有内容。



### Transitions
### 过渡


In dynamic scenes, you can configure an entity to affect the way in which it moves. By default, all changes to an entity are rendered as a sudden shift from one state to another. By adding a transition component, you can make the change be gradual and more natural.

在动态场景中，您可以配置实体以影响其移动方式。默认情况下，对实体的所有更改都会表现为从一个状态突然转换到另一个状态。添加过渡组件，可以使更改变得更加渐进和自然。

The example below shows a box entity that is configured to rotate smoothly. 

下面的示例展示了一个平滑旋转的 box 实体。


{% raw %}
```tsx
 <box 
    rotation={currentRotation}
    transition={{ rorotation: { duration: 1000, timing: "ease-in" }}}
  />
```
{% endraw %}

> Note: The transition component doesn't make the box rotate, it just sets the way it rotates whenever the value of the entity's rotation changes, usually as the result of an event.

> 注意：过渡组件并不会使 box 旋转，它只是设置作为事件的结果实体的旋转值发生变化时的旋转方式。

The transition component can be added to affect the following properties of an entity:

可以添加 transition 组件以影响实体的以下属性：

* position
* rotation
* scale
* color

Note that the transition for each of these properties is configured separately.

请注意，每个过渡属性都是分开配置的。

{% raw %}
```tsx
 <box 
    rotation={currentRotation}
    color={currentColor}
    scale={currentScale}
    transition={ 
        { rotation: { duration: 1000, timing: "ease-in" }}
        { color: { duration: 3000, timing: "exponential-in" }}
        { scale: { duration: 300, timing: "bounce-in" }}
        }
  />
```
{% endraw %}

The transition component allows you to set:

过渡组件允许您设置：

* A delay: milliseconds to wait before the change begins occuring.
* A duration: milliseconds from when the change begins to when it ends.
* Timing: select a function to shape the transition. For example, the transition could be `linear`, `ease-in`, `ease-out`, `exponential-in` or `bounce-in`, among other options.

* delay：在更改开始发生之前等待的毫秒数。
* duration：从更改开始到结束时的毫秒数。
* Timing：选择一个过渡函数。例如，可以用 `linear`, `ease-in`, `ease-out`, `exponential-in` 或 `bounce-in` 等选项。


In the example below, a transition is applied to the rotation of an invisible entity that wraps a box. As the box is off-center from the parent entity, the box pivots like an opening door.

在下面的示例中，在组合了 box 的不可见实体的旋转上设置了过渡。由于 box 不在父实体的中心，使得 box 的旋转看起来如同开门一样。

{% raw %}
```tsx

<entity 
    rotation={currentRotation}  
    transition={{ rotation: { duration: 1000, timing: "ease-in" }}}>
        <box 
          id="door" 
          scale={{ x: 1, y: 2, z: 0.05 }} 
          position={{ x: 0.5, y: 1, z: 0 }} 
        />
</entity>
```
{% endraw %}

## Color
## 颜色


Color is set in hexadecimal values. To set an entity's color, simply set its `color` component to the corresponding hexadecimal value.

颜色以十六进制值设置。要设置实体的颜色，只需将其 `color` 组件设置为相应的十六进制值即可。


{% raw %}
```tsx
  <sphere 
    position={{ x: 0.5, y: 1, z: 0 }}   
    color="#EF2D5E"
  />
```
{% endraw %}

> Tip: There are many online color-pickers you can use to find a specific color graphically. To name one, you can try the color picker on [W 3 Schools](https://www.w3schools.com/colors/colors_picker.asp).

> 提示：有许多在线颜色选择器可以帮你找到特定的颜色。如，您可以尝试[W 3 Schools](https://www.w3schools.com/colors/colors_picker.asp)上的颜色选择器。



## Materials
## 材质

Materials are defined as separate entities in a scene, this prevents material definitions from being duplicated, keeping the scene's code lighter.

材质被定义为场景中的单独实体，这可以预防重复定义材质，简化场景中的代码。


Materials can be applied to primitive entities and to planes, simply by setting the `material` component.

只需设置 `material` 组件，即可将材质应用于基本实体和平面。


{% raw %}
```tsx
  <material 
    id="reusable_material" 
    albedoTexture="materials/wood.png" 
    roughness="0.5" 
  />
  <sphere
    material="#reusable_material" 
  />
```
{% endraw %}

Materials are also implicitly imported into a scene when you import a glTF model that includes embedded materials. When that's the case, the scene doesn't need a `<material/>` entity declared.

导入包含嵌入材质的 glTF 模型时，会隐式地将材质导入到场景中。在这种情况下，场景不需要声明 `<material/>` 实体。



## Import 3D models
## 导入3D模型
 
For more complex shapes, you can build a 3D model in an external tool like Blender and then import them in glTF format.  [glTF](https://www.khronos.org/gltf) (GL Transmission Format) is an open project by Khronos providing a common, extensible format for 3D assets that is both efficient and highly interoperable with modern web technologies.

对于更复杂的形状，您可以在 Blender 等外部工具中构建 3D 模型，然后以 glTF 格式导入它们。 [glTF](https://www.khronos.org/gltf) (GL 传输格式) 是 Khronos 的一个开放项目，为 3D 资产提供了一种通用的，可扩展的格式，既高效又跟现代 web 技术具有高度交互性。

> Note: When using Blender, you need an add-on to export glTF files. For models that don't include animations we recommend installing the add-on by [Kronos group](https://github.com/KhronosGroup/glTF-Blender-Exporter). To export glTFs that include animations, you should instead install the add-on by [Kupoman](https://github.com/Kupoman/blendergltf).

> 注意：使用 Blender 时，需要一个附加组件来导出 glTF 文件。对于不包含动画的模型，我们建议您安装[Kronos group](https://github.com/KhronosGroup/glTF-Blender-Exporter) 的附加组件。要导出包含动画的 glTF，您应该安装 [Kupoman](https://github.com/Kupoman/blendergltf) 附加组件。


To add an external model into a scene, add a `<gltf-model>` element and set its `src` component to the path of the glTF file containing the model.

要将外部模型添加到场景中，请添加 `<gltf-model>` 元素并将其 `src` 组件设置为包含模型的 glTF 文件的路径。

> Tip: We recommend keeping your models separate in a `/models` folder inside your scene.

> 提示：我们建议您将模型分开放在场景中的 `/models` 文件夹中。


{% raw %}
```tsx
<gltf-model
    position={{ x: 5, y: 3, z: 5 }}
    scale={0.5}
    src="models/myModel.gltf"
  />
```
{% endraw %}

glTF models can have either a `.gltf` or a `.glb` extension. glTF files are human-readable, you can open one in a text editor and read it like a JSON file. This is useful, for example, to verify that animations are properly attached and to check for their names. glb files are binary, so they're not readable but they are considerably smaller in size, which is good for the scene's performance. 

glTF 模型可以有 `.gltf` 或 `.glb` 扩展名。 glTF 文件是人类可读的，您可以在文本编辑器中打开它并像 JSON 文件一样读取。例如，这有助于验证动画是否已正确附加并检查其名称。 glb 文件是二进制文件，所以它们不可读，但它们的文件要小得多，有利于提高场景的性能。

> Tip: We recommend using `.gltf` while you're working on a scene, but then switching to `.glb` when uploading it.

> 提示：我们建议您在处理场景时使用 `.gltf`，然后在上传时切换使用 `.glb`。


glTF models can also include their own textures and animations. Keep in mind that all models, their shaders and their textures must be within the parameters of the [scene limitations]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-06-scene-limitations %}).

glTF 模型还可以包含自己的纹理和动画。请记住，所有模型，着色器和纹理都必须在[场景限制]({{ site.baseurl }}{% post_url /documentation/building-scenes/2018-01-06-scene-limitations %})的参数范围内。


> Note: obj models are also supported as a legacy feature, but will likely not be supported for much longer. To add one, use an `<obj-model>` entity. 

> 注意：obj 模型作为遗留功能也支持，但可能不会支持很长时间。请使用 `<obj-model>` 添加实体。


### Animations
### 动画


> Note: Keep in mind that all models and their textures must be within the parameters of the [scene limitations]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-06-scene-limitations %}).

> 注意：请记住，所有模型及其纹理必须在[scene limitations]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-06-scene-limitations %})的参数范围内。


Files with .gltf extensions can be opened with a text editor to view their contents. There you can find the list of animations included in the model and how they're named.

可以使用文本编辑器打开具有 .gltf 扩展名的文件以查看其内容。在那里，您可以找到模型中包含的动画列表以及它们的命名方式。


In a dynamic scene, you reference an animation by its armature name, an underscore and its animation name. For example `myArmature_animation1`. You activate an animation by setting its `playing` property to `true`.

在动态场景中，您可以通过其骨架名称，下划线及其动画名称来引用动画。例如`myArmature_animation1`。您可以通过将其 `playing` 属性设置为 `true` 来激活动画。


The example below imports a model that includes animations and configures them:

以下示例导入包含动画的模型并对其进行配置：


{% raw %}
```tsx
<gltf-model
    position={{ x: 5, y: 3, z: 5 }}
    scale={0.5}
    src="models/shark_anim.gltf"
    skeletalAnimation={[
      { clip: 'shark_skeleton_bite', playing: false },
      { clip: 'shark_skeleton_swim', weight: 0.2, playing: true }
    ]}
  />
```
{% endraw %}

In this example, the armature is named `shark_skeleton` and the two animations contained in it are named `bite` and `swim`.

在这个例子中，骨架被命名为 `shark_skeleton` ，其中包含的两个动画被命名为 `bite` 和 `swim`。


An animation can be set to loop continuously by setting its `loop` property. If `loop:false` then the animation will be called only once when activated.

通过设置 `loop` 属性，可以将动画设置为连续循环。如果设置 `loop:false` 那么动画只会在激活时被调用一次。


The `weight` property allows a single model to carry out multiple animations at once, calculating a weighted average of all the movements involved in the animation. The value of `weight` determines how much importance the given animation will be given.

`weight` 属性允许单个模型一次执行多个动画，计算动画中所有运动的加权平均值。 `weight` 的值决定了给定动画的重要程度。


### Free libraries for 3D models

### 免费 3D 模型库


Instead of building your own 3d models, you can also download them from several free or paid libraries.

您也可以从多个免费或付费库中下载，而不是自己构建 3D 模型。


To get you started, below is a list of libraries that have free or relatively inexpensive content:

为了帮助您入门，下面列出了免费或相对便宜的内容库：


* [Google Poly](https://poly.google.com)
* [SketchFab](https://sketchfab.com/)
* [Clara.io](https://clara.io/)
* [Archive3D](https://archive3d.net/)
* [SketchUp 3D Warehouse](https://3dwarehouse.sketchup.com/)
* [Thingiverse](https://www.thingiverse.com/) (3D models made primarily for 3D printing, but adaptable to Virtual Worlds)
* [ShareCG](https://www.sharecg.com/)

> Note: Pay attention to the licence restrictions that the content you download has.

> 注意：请注意您下载的内容的许可。


Note that most of the models that you can download from these sites won't be in glTF. If that's the case, you must convert them to glTF before you can use them in a scene. We recommend importing them into Blender and exporting them with one of the available glTF export add-ons.

请注意，您可以从这些站点下载的大多数模型都不是 glTF 格式。如果是这种，您必须先将它们转换为 glTF，然后才能在场景中使用它们。我们建议将它们导入 Blender 并用可用的 glTF 导出附加组件导出它们。



### Why we use glTF?
###为什么我们使用glTF？


Compared to the older *OBJ format*, which supports only vertices, normals, texture coordinates, and basic materials,
glTF provides a more powerful set of features. In addition to all of the features we just named, glTF also offers:

与旧的只支持顶点，法线，纹理坐标和基本材质的 *OBJ 格式* 相比，glTF 提供了一组更强大的功能。除了我们刚刚命名的所有功能外，glTF 还提供：

- Hierarchical objects
- Scene information (light sources, cameras)
- Skeletal structure and animation
- More robust materials and shaders

- 分层对象
- 场景信息（光源，相机）
- 骨骼结构和动画
- 更强大的材质和着色器


OBJ can currently be used for simple models that have no animations, but we will probably stop supporting it in the future.

OBJ 目前可用于没有动画的简单模型，但我们可能会在将来停止支持。

Compared to *COLLADA*, the supported features are very similar. However, because glTF focuses on providing a "transmission format" rather than an editor format, it is more interoperable with web technologies.

与 *COLLADA* 相比，支持的功能非常相似。但是，因为 glTF 专注于提供“传输格式”而不是编辑器格式，它与 Web 技术更具互操作性。


Consider this analogy: the .PSD (Adobe Photoshop) format is helpful for editing 2D images, but images must then be converted to .JPG for use
on the web. In the same way, COLLADA may be used to edit a 3D asset, but glTF is a simpler way of transmitting it while rendering the same result.

考虑这个类比：.PSD（Adobe Photoshop）格式有助于编辑 2D 图像，但必须将图像转换为 .JPG 才能使用，同样，在网上，COLLADA 可用于编辑 3D 资源，但 glTF 在渲染同样的结果时是一种更简单的传输方式。


## Sound
## 声音


You can add sound to your scene by including a sound component in any entity.

您可以通过在任何实体中包含 sound 组件来为场景添加声音。



{% raw %}
```tsx
  <sphere 
    position={{ x: 5, y: 3, z: 5 }}
    sound={{
      src: "sounds/carnivalrides.ogg", 
      loop: true, 
      playing: true,
      volume: 0.5
      }}
  />
```

{% endraw %}

The `src` property points to the location of the sound file.

`src` 属性指向声音文件的位置。


> Tip: We recommend keeping your sound files separate in a `/sounds` folder inside your scene.

> 提示：我们建议您将声音文件保存在场景内的 `/sounds` 文件夹中。


Supported sound formats vary depending on the browser, but it's safe to use `.mp3`, `.accc` and  `.ogg`. `.wav` files are also supported but not generally recommended as they are significantly larger.

支持的声音格式因浏览器而异，但使用 `.mp3`，`.accc` 和 `.ogg` 是安全的。 `.wav` 文件也受支持，但通常不推荐，因为它们要大得多。


Each entity can only play a single sound file. This limitation can easily be overcome by including multiple invisible entities, each with their own sound file.

每个实体只能播放一个声音文件。通过包含多个不可见实体可以轻松克服此限制，每个实体都可有自己的声音文件。


The `distanceModel` property of the sound component conditions how the user's distance to the sound's source affects its volume. The model can be `linear`, `exponential` or `inverse`. When using the linear or exponential model, you can also set the `rolloffFactor` property to set the steepness of the curve. 

声音组件的 `distanceModel` 属性指定用户与声音源的距离如何影响其音量。可以是 `linear`, `exponential` 或 `inverse`。使用线性或指数模型时，您还可以设置 `rolloffFactor` 属性来设置曲线的陡度。


<!---

Setting loop to false stops the audio, it doesn't pause it. So when setting loop to true the audio will start from the beginning.

Setting playing to false pauses??????


### How to use Blender with the SDK


how to add collider meshes into GLTF models

-->

## Entity collision

## 实体 collision （碰撞）

Entities that have collision disabled can be walked through by a user`s avatar, entities that do have collisions enabled occupy space and block a user's path.

已禁用 collision 的实体玩家可以穿越而过，启用了 collisions 的实体占用空间并且会阻档用户通过。


{% raw %}
```tsx
<box 
  position={{ x: 10, y: 0, z: 0 }} 
  scale={2} 
  ignoreCollisions={false}
/>

```
{% endraw %}

The example above defines a box entity that can't be walked through.

上面的示例定义了一个无法通过的 box 实体。

All entities have collisions disabled by default. Depending on the type of entity, collisions are enabled as follows:

默认情况下，所有实体都禁用了碰撞。根据实体的类型，碰撞的启用方式如下：


* For most entities, including *primitives* (boxes, spheres, etc), planes and base entities, you enable collisions by setting the `ignoreCollisions` component to `false`. 
* To enable collisions in *glTF models*, you can either:

  *   Edit them in an external tool like Blender to include a *collission mesh*.
  *   Overlay an invisible entity with the `ignoreCollisions` component set to `true`.

* 对于大多数实体，包括*基元*（盒子，球体等），平面和基本实体，可以通过将 `ignoreCollisions` 组件设置为 `false` 来启用。

* 要在 *glTF模型* 中启用碰撞，您可以：
  * 在像 Blender 这样的外部工具中编辑它们以包含 *碰撞网格*。
  * 覆盖一个不可见的实体，将`ignoreCollisions`组件设置为 `true`。


A *collision mesh* is a set of planes or geometric shapes that define which parts of the model are collided with. This allows for much greater control and is a lot less demanding on the system, as the collision mesh is usually a lot simpler (with less vertices) than the original model.

A *碰撞网格* 是一组平面或几何形状，用于定义模型的哪些部分发生碰撞。这样可以有更多的控制并降低对系统的要求，因为碰撞网格通常比原始模型更简单（具有更少的顶点）。


Collision settings currently don't affect how other entities interact with each other, entities can always overlap. Collision settings only affect how the entity interacts with the avatar.

当前碰撞设置不会影响与其他实体间的交互，实体始终可以重叠。碰撞设置仅影响实体与玩家的交互方式。


Decentraland currently doesn't have a physics engine, so if you want entities to fall, crash or bounce, you must code this behavior into the scene.

Decentraland 目前没有物理引擎，因此如果您希望实体掉落，相撞或反弹等效果，则必须将此行为编码到场景中。






## Migrating XML to type script
## 将 XML 迁移到 typpescript

If you have a static XML scene and want to add dynamic capabilities to it, you must migrate it to TSX format. This implies making some minor changes to the entity syntax.

如果您使用静态 XML 场景并希望向其添加动态功能，则必须将其迁移到 TSX 格式。这意味着要对实体语法进行一些小的更改。


### Data types
### 数据类型

> **TL;DR**  
> in XML: `position="10 10 10"`  
> in TSX: `position={ { x:10, y: 10, z: 10 } }`

There are subtle differences between the `text/xml` representation and the TSX representation of a scene. Our approach is TSX-first, and the XML representation of the scene is only a compatibility view. Because of this, attributes in TSX must be objects, not plain text.

`text/xml` 表示与场景的 TSX 表示之间存在细微差别。我们的方法是 TSX 优先，场景的 XML 表示只是兼容性视图。因此，TSX 中的属性必须是对象，而不是文本。


```xml
<scene>
  <box position="10 10 10" />
</scene>
```

The static scene above becomes the following dynamic schen when migrating it to TSX:

上面的静态场景迁移到 TSX 时会形成如下动态场景：

{% raw %}
```tsx
class Scene extends ScriptableScene {
  async render() {
    return (
      <scene>
        <box position={{ x: 10, y: 10, z: 10 }} />
      </scene>
    );
  }
}
```
{% endraw %}

#### Attribute naming
### 属性命名

> **TL;DR**  
> in XML: `albedo-color="#ffeeaa"` (kebab-case)  
> in TSX: `albedoColor="#ffeeaa"` (camelCase)

HTML and XHTML are case insensitive for attributes, this generates conflicts with the implementation of certain attributes like `albedoColor`. Because reading `albedocolor` was confusing, and having hardcoded keys with hyphens in the code was so dirty, we decided to follow the React convention of having every property camel cased in code and hyphenated in the HTML/XML representation. 

HTML 和 XHTML 对属性不区分大小写，这会与某些属性（如`albedoColor`）的实现冲突。因为 `albedocolor` 令人困惑，并且在代码中使用连字符硬编码显得不太干净，我们决定遵循 React 惯例，即在代码中对每个属性采用驼峰式命名，在 HTML/XML 中使用连字符。


{% raw %}
```xml
<scene>
  <!-- XML -->
  <material id="test" albedo-color="#ffeeaa" />
</scene>
```
{% endraw %}

The static scene above becomes the following dynamic schen when migrating it to TSX:

将上面的静态场景迁移到 TSX 时会形成如下代码：


{% raw %}
```tsx
<!-- TSX -->
class Scene extends ScriptableScene {
  async render() {
    return (
      <scene>
        <material id="test" albedoColor="#ffeeaa" />
      </scene>
    );
  }
}
```
{% endraw %}
