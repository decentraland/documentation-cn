---
date: 2018-06-12
title: 场景内容指南
description: 场景中如何导入 3D 模型
categories:
  - sdk-reference
type: Document
set: sdk-reference
set_order: 2
tag: introduction
---

Decentraland 中的三维场景是基于 [Entity-Component](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system) 模型的，场景中的所有内容都是由 *entity* 组成，每个 entity （实体）又可以包含 *components （组件）*，用来塑造其特性和功能。

实体可以嵌套在其他实体中，形成树结构。实际上，所有场景的输出都是嵌套的实体树。 `.xml` 场景文件显式定义了这个树，`.xlt` 场景文件定义了构建和更新这个树的 typescript 代码。

本文档介绍了如何使用场景树中的不同类型的实体和组件来实现基本对象。

## 创建简单的几何形状

几种基本形状（通常称为 *基本形体*）可以作为预定义的实体类型添加到场景中。它们具有已经定义好的组件（比如它们的形状），并可以设置其他组件（比如旋转和颜色）。

可以使用以下类型的实体：

* `<Box />`
* `<Sphere />`
* `<Plane />`
* `<Cylinder />`
* `<Cone />`

所有这些都可以添加到场景中，它们都可以包含基本组件，如位置，比例或颜色。

```tsx
<box position={vector} color="#ff00aa" scale={2} />
```

有关这些实体的更多详细信息，请参阅[实体接口]({{ site.baseurl }}{% post_url /sdk-reference/2018-06-21-entity-interfaces %}) 。

> 提示：通过源代码编辑器（如 Visual Studio Code）编辑代码时，您能看到毎种实体支持的组件列表。通常，可以把光标放在实体中并输入 *Ctrl + 空格* 来实现。

## 文本块

您可以在 Decentraland 场景中将文本作为实体添加。 您可以将这些文本实体用作场景中的标签，把它们暂时作为错误消息或任何您希望的内容显示。

{% raw %}

```tsx
<text
  value="Users will see this text floating in space in your scene."
  hAlign="left"
  position={{ x: 5, y: 1, z: 5 }}
/>
```

{% endraw %}

要在文本实体中显示非 _string_ 类型的值，请使用 `toString()` 函数将其类型转换为 _string_。

{% raw %}

```tsx
<text value={this.state.gameScore.toString()} />
```

{% endraw %}

## 颜色

颜色以十六进制值设置。要设置实体的颜色，只需将其 `color` 组件设置为相应的十六进制值即可。


{% raw %}

```tsx
<sphere position={{ x: 0.5, y: 1, z: 0 }} color="#EF2D5E" />
```

{% endraw %}

> 提示：有许多在线颜色选择器可以帮你找到特定的颜色。如，您可以尝试[W 3 Schools](https://www.w3schools.com/colors/colors_picker.asp)上的颜色选择器。

## 材质

材质被定义为场景中的单独实体，这可以预防重复定义材质，简化场景中的代码。

只需设置 `material` 组件，即可将材质应用于基本实体和平面。


{% raw %}

```tsx
<material
  id="reusable_material"
  albedoTexture="materials/wood.png"
  roughness={0.5}
/>
<sphere material="#reusable_material" />
```

{% endraw %}

在上面的示例中，材质的图像位于 `materials` 文件夹中，该文件夹位于场景项目文件夹的根目录。

导入包含嵌入材质的 glTF 模型时，会隐式地将材质导入到场景中。在这种情况下，场景不需要声明 `<material/>` 实体。

#### 纹理贴图

使用材质的实体可以设置将特定区域的纹理贴到其表面。通过在纹理的二维图像上设置 _u_ 和 _v_ 坐标对应实体的顶点来实现。实体拥有的顶点越多，就需要在纹理上定义更多的 _uv_ 坐标，例如，plane 需要定义 8 个 _uv_ 点，两个面中的每面都需要 4 个点。

{% raw %}

```tsx
async render() {
  return (
    <scene position={{ x: 5, y: 1.5, z: 5 }}>
      <basic-material
        id="sprite001"
        texture="materials/atlas.png"
        samplingMode={DCL.TextureSamplingMode.NEAREST}
      />
      <plane
        material="#sprite001"
        uvs={[
          0,
          0.75,
          0.25,
          0.75,
          0.25,
          1,
          0,
          1,
          0,
          0.75,
          0.25,
          0.75,
          0.25,
          1,
          0,
          1
        ]}
      />
    </scene>
  )
}
```

{% endraw %}

通过纹理贴图，更改所有帧中相同纹理的选定区域。可以创建动画精灵 Animated Sprite 效果。

#### 透明材质

要使材质透明，必须将 Alpha 通道添加到用于纹理的图像中。 `material` 实体默认忽略纹理图像的 alpha 通道，所以需要设置以下一种：

- 将 `hasAlpha` 设置为 true。
- 在 `alphaTexture` 中设置图像，可以是相同或不同的图像。

{% raw %}

```tsx
<material
  albedoTexture="materials/semiTransparentTexture.png"
  hasAlpha
/>
// or
<material
  albedoTexture="materials/semiTransparentTexture.png"
  alphaTexture="materials/semiTransparentTexture.png"
/>
```

{% endraw %}

#### 基本材质

您可以使用 `<basic-material />` 而不是 `<material />` 实体定义材质。 这样就可以制成无阴影且不受光影响的材质。 这对于创建应该保持一致的用户界面非常有用，它还可以用于为场景提供更简约的外观。

{% raw %}

```tsx
<basic-material
  id="basic_material"
  texture="materials/profile_avatar.png"
/>
<sphere
  material="#basic_material"
/>
```

{% endraw %}

导入包含嵌入材质的 glTF 模型时，也会将材质隐式导入到场景中。 在这种情况下，场景不需要声明 `<material />` 实体。

## 导入 3D 模型

对于更复杂的形状，您可以在 Blender 等外部工具中构建 3D 模型，然后以 glTF 格式导入它们。 [glTF](https://www.khronos.org/gltf) (GL 传输格式) 是 Khronos 的一个开放项目，为 3D 资产提供了一种通用的，可扩展的格式，既高效又跟现代 web 技术具有高度交互性。

要将外部模型添加到场景中，请添加 `<gltf-model>` 元素并将其 `src` 组件设置为包含模型的 glTF 文件的路径。


{% raw %}
```tsx
<gltf-model
    position={{ x: 5, y: 3, z: 5 }}
    scale={0.5}
    src="models/myModel.gltf"
  />
```
{% endraw %}

在上面的示例中，材质的图像位于 `materials` 文件夹中，该文件夹位于场景项目文件夹的根目录。

> 提示：我们建议您将模型分开放在场景中的 `/models` 文件夹中。

glTF 模型还可以包括自己的纹理，材质，collider（碰撞）和动画。 有关详细信息，请参阅[3D 模型注意事项]({{ site.baseurl }}{% post_url /getting-started/2018-01-09-external-3d-models %})。

请记住，所有模型、它们的着色器和它们的纹理都必须在[场景限制]({{ site.baseurl }}{% post_url /getting-started/2018-01-06-scene-limitations %})的范围内。

> 注意：obj 模型作为遗留功能也支持，但可能不会支持很长时间。请使用 `<obj-model>` 添加实体。

#### 动画

可以使用文本编辑器打开具有 .gltf 扩展名的文件以查看其内容。在那里，您可以找到模型中包含的动画列表以及它们的命名方式。通常，动画名称由其骨架名称，下划线及其动画名称组成。 例如 `myArmature_animation1`。

在导入 Decentraland 场景前，有关如何为 3D 模型创建动画的信息，请参阅[3D 模型注意事项]({{ site.baseurl }}{% post_url /getting-started/2018-01-09-external-3d-models %})。

通过将 _skeletalAnimation_ 设置添加到 gltf 模型并设置它的一个剪辑的 `playing` 属性为 `true` 来激活动画。

以下示例导入包含动画的模型并对其进行配置：

{% raw %}

```tsx
<gltf-model
  position={{ x: 5, y: 3, z: 5 }}
  scale={0.5}
  src="models/shark_anim.gltf"
  skeletalAnimation={[
    { clip: "shark_skeleton_bite", playing: false },
    {
      clip: "shark_skeleton_swim",
      weight: 0.2,
      playing: true
    }
  ]}
/>
```

{% endraw %}

在这个例子中，骨架被命名为 `shark_skeleton` ，其中包含的两个动画被命名为 `bite` 和 `swim`。

通过设置 `loop` 属性，可以将动画设置为连续循环。如果设置 `loop:false` 那么动画只会在激活时被调用一次。

`weight` 属性允许单个模型一次执行多个动画，计算动画中所有运动的加权平均值。 `weight` 的值决定了给定动画的重要程度。

#### 免费 3D 模型库

您也可以从多个免费或付费库中下载，而不是自己构建 3D 模型。

为了帮助您入门，下面列出了免费或相对便宜的内容库：


* [Google Poly](https://poly.google.com)
* [SketchFab](https://sketchfab.com/)
* [Clara.io](https://clara.io/)
* [Archive3D](https://archive3d.net/)
* [SketchUp 3D Warehouse](https://3dwarehouse.sketchup.com/)
* [Thingiverse](https://www.thingiverse.com/) (3D models made primarily for 3D printing, but adaptable to Virtual Worlds)
* [ShareCG](https://www.sharecg.com/)

> 注意：请注意您下载的内容的许可。

请注意，您可以从这些站点下载的大多数模型都不是 glTF 格式。如果是这种，您必须先将它们转换为 glTF，然后才能在场景中使用它们。我们建议将它们导入 Blender 并用可用的 glTF 导出附加组件导出它们。

## 声音

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

`src` 属性指向声音文件的位置。

在上面的示例中，音频文件位于 `sounds` 文件夹中，此文件夹位于场景项目文件夹的根目录中。

> 提示：我们建议您将声音文件保存在场景内的 `/sounds` 文件夹中。

支持的声音格式因浏览器而异，但使用 `.mp3`，`.accc` 和 `.ogg` 是安全的。 `.wav` 文件也受支持，但通常不推荐，因为它们要大得多。

每个实体只能播放一个声音文件。通过包含多个不可见实体可以轻松克服此限制，每个实体都可有自己的声音文件。

声音组件的 `distanceModel` 属性指定用户与声音源的距离如何影响其音量。可以是 `linear`, `exponential` 或 `inverse`。使用线性或指数模型时，您还可以设置 `rolloffFactor` 属性来设置曲线的陡度。

<!---

Setting loop to false stops the audio, it doesn't pause it. So when setting loop to true the audio will start from the beginning.

Setting playing to false pauses??????

same for video??

-->

## Video

您可以通过添加 `video` 实体将视频添加到场景中。

{% raw %}

```tsx
<video
  id="myVideo"
  position={{ x: 2, y: 3, z: 1 }}
  width={4}
  height={2.5}
  volume={currentVolume}
  src="video/myVideo.mp4"
  play={true}
/>
```

{% endraw %}

_video_ 实体需要在 `src` 中选择一个视频，可以是本地文件，也可以是指向远程视频的 URL。

在上面的示例中，视频位于 `video` 文件夹中，该文件夹位于场景项目文件夹的根目录。

支持的视频格式因浏览器而异，但使用 `.mp4` 和 `.avi` 是安全的。

可以使用 `volume` ，将音量设置为 0 到 100。

## 实体 collision (碰撞)

启用了 collisions 的实体占用空间并且会阻档用户通过，没有设置 collision 的对象，玩家可以穿越而过。

{% raw %}

```tsx
<box position={{ x: 10, y: 0, z: 0 }} scale={2} withCollisions={true} />
```

{% endraw %}

上面的示例定义了一个无法通过的 box 实体。

默认情况下，所有实体都禁用了碰撞。根据实体的类型，碰撞的启用方式如下：

- 对于大多数实体，包括*基元*（盒子，球体等），平面和基本实体，可以通过将 `withCollisions` 组件设置为 `true` 来启用。
- 要在 *glTF模型* 中启用碰撞，您可以：
  - 覆盖一个不可见的实体，将`withCollisions`组件设置为 `true`。
  - 在 Blender 等外部工具中编辑模型以包含 _collider 对象_。 collider 必须命名为 _x_collider_，其中 _x_ 是模型的名称。 因此，对于名为 house 的模型，必须将 collider 命名为_house_collider_。

_collider_ 是一组平面或几何形状，用于定义模型的哪些部分发生碰撞。这样可以有更多的控制并降低对系统的要求，因为碰撞网格通常比原始模型更简单（具有更少的顶点）。

有关 collider 是什么以及如何添加的更多详细信息，请参阅[3D 模型注意事项]({{ site.baseurl }}{% post_url /getting-started/2018-01-09-external-3d-models %})。

当前碰撞设置不会影响与其他实体间的交互，实体始终可以重叠。碰撞设置仅影响实体与游戏中的玩家的交互方式。

Decentraland 目前没有物理引擎，因此如果您希望实体掉落，相撞或反弹等效果，则必须将此行为编码到场景中。

>提示：要查看场景中所有碰撞网格的限制，请使用 `dcl start` 启动场景预览，然后单击 `c`。 会用蓝线显示所有 collider 碰撞区域的位置。

## 将 XML 迁移到 typpescript

如果您使用静态 XML 场景并希望向其添加动态功能，则必须将其迁移到 TSX 格式。这意味着要对实体语法进行一些小的更改。

#### 数据类型

> **TL;DR**  
> in XML: `position="10 10 10"`  
> in TSX: `position={ { x:10, y: 10, z: 10 } }`

`text/xml` 表示与场景的 TSX 表示之间存在细微差别。我们的方法是 TSX 优先，场景的 XML 表示只是兼容性视图。因此，TSX 中的属性必须是对象，而不是文本。


```xml
<scene>
  <box position="10 10 10" />
</scene>
```

上面的静态场景迁移到 TSX 时会形成如下动态场景：

{% raw %}

```tsx
class Scene extends ScriptableScene {
  async render() {
    return (
      <scene>
        <box position={{ x: 10, y: 10, z: 10 }} />
      </scene>
    )
  }
}
```

{% endraw %}

#### 属性命名

> **TL;DR**  
> in XML: `albedo-color="#ffeeaa"` (kebab-case)  
> in TSX: `albedoColor="#ffeeaa"` (camelCase)

HTML 和 XHTML 对属性不区分大小写，这会与某些属性（如`albedoColor`）的实现冲突。因为 `albedocolor` 令人困惑，并且在代码中使用连字符硬编码显得不太干净，我们决定遵循 React 惯例，即在代码中对每个属性采用驼峰式命名，在 HTML/XML 中使用连字符。

{% raw %}

```xml
<scene>
  <!-- XML -->
  <material id="test" albedo-color="#ffeeaa" />
</scene>
```

{% endraw %}

将上面的静态场景迁移到 TSX 时会形成如下代码：

{% raw %}

```tsx
// TSX
class Scene extends ScriptableScene {
  async render() {
    return (
      <scene>
        <material id="test" albedoColor="#ffeeaa" />
      </scene>
    )
  }
}
```

{% endraw %}