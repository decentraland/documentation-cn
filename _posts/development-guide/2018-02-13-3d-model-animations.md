---
date: 2018-02-13
title: 3D 动画模型
description: 如何移动场景中的实体实现实体动画
categories:
  - development-guide
type: Document
set: development-guide
set_order: 13
---

在 _.glTF_ 和 _.glb_ 格式的 3D 模型中可以包含任意数量的动画。动画通过指定一系列随时间推移的不同位置，将由网格形成的造型从一个造型转变到另一个造型来模拟连续运动。动画指定了网格如何移动以及移动的方式。

有关如何为其创建模型和动画的详细信息，请参阅[3D模型注意事项]({{ site.baseurl }}{% post_url /development-guide/2018-01-09-external-3d-models %})。阅读[形状组件]({{ site.baseurl }}{% post_url /development-guide/2018-02-6-shape-components %})，了解有关如何将 3D 模型导入场景的说明。

> 提示：动画更适合于原地移动，而不是用于更改场景中实体的位置。例如，可以设置动画原地移动角色的脚，但是要更改实体的位置，请使用Transform 组件。有关详细信息，请参阅[实体定位]({{ site.baseurl }}{% post_url /development-guide/2018-01-12-entity-positioning %})。

## 检查 3D 模型

并不是所有 _glTF_ 文件都包含动画。要查看其中是否有动画，您可以执行以下操作：

- 如果使用 [VS Code](https://code.visualstudio.com/)（推荐），请安装 _GLTF Tools_ 扩展，然后在那里查看 glTF 文件的内容。
- 打开 [Babylon Sandbox](https://sandbox.babylonjs.com/)，然后拖放 glTF 文件（及所有依赖的 _.jpg_ 或 _.bin_）。
- 使用文本编辑器打开 _.glTF_ 文件，向下滚动直到找到 _"animations":_。

通常，动画名称由其骨骼支架名称，下划线及其动画名称组成。例如 `myArmature_animation1`。

## 实例及添加动画

要在 3D 模型中使用动画，必须创建一个 `AnimationClip` 对象并将其添加到 `GLTFShape` 组件中。

![](/images/media/ecs-animations.png)

```ts
let shark = new Entity()
shark.add(new GLTFShape("models/shark.gltf"))

// Instance animation clip
const clipSwim = new AnimationClip("swim")

// Add animation clip to GLTFShape component
shark.get(GLTFShape).addClip(clipSwim)
```

还可以在单​​个语句中创建和添加 `AnimationClip`：

```ts
// Instance and add a clip
shark.get(GLTFShape).addClip(new AnimationClip("swim"))

// Retrieve a clip that was added to a component
let swim = swim.get(GLTFShape).getClip("swim")
```

创建和添加 `AnimationClip` 的步骤也可以省略。如果您尝试获取从未添加到组件的 `AnimationClip`，则会自动创建并添加：


```ts
// Create and get a clip
let swim = swim.get(GLTFShape).getClip("swim")
```

`AnimationClip` 对象的每个实例都有自己的状态，用以记录它在动画中前进了多少。如果将同一个 `AnimationClip` 实例添加到来自不同实体的多个 `GLTFShape` 组件，它们都将引用相同的状态。这意味着如果您播放这个动画，则使用该实例的所有实体将同时播放动画。

如果要使用相同的动画独立地为多个实体设置动画，则必须实例多个 `AnimationClip` 对象，每个实体使用一个对象。

```ts
// Create an entity
let shark1 = new Entity()
shark1.add(new GLTFShape("models/shark.gltf"))

// Instance and add a clip
shark1.get(GLTFShape).addClip(new AnimationClip("swim"))

// Create a second entity
let shark2 = new Entity()
shark2.add(new GLTFShape("models/shark.gltf"))

// Instance and add a new clip
shark2.get(GLTFShape).addClip(new AnimationClip("swim"))
```

## 播放动画

一旦 `AnimationClip` 被添加到 `GLTFShape` 组件，它默认开始状态为暂停。

播放或暂停最简单的方式是使用 `AnimationClip` 的 `play()` 和 `pause()` 方法。


```ts
// Create animation clip
const clipSwim = new AnimationClip("swim")

// Start playing the clip
clipSwim.play()

// Pause the playing of the clip
clipSwim.pause()
```

如果你的场景代码没有直接引用 clip 对象的指针，可以使用 `getClip()` 从 `GLTFShape` 中获取。


```ts
// Create and add clip
shark.get(GLTFShape).addClip(new AnimationClip("swim"))

// Start playing the clip
shark
  .get(GLTFShape)
  .getClip("swim")
  .play()

// Pause the playing of the clip
shark
  .get(GLTFShape)
  .getClip("swim")
  .pause()
```

> 注意：如果您尝试使用 `getClip()` 来获取组件中不存在的动画剪辑，则返回`null`。

`AnimationClip` 对象也有一个 `playing` 布尔参数。可以通过更改此参数的值来启动或停止动画。

```ts
clipSwim.playing = true
```

## Set clip parameters

## 设置动画剪辑参数

您可以为 `AnimationClip` 配置以下参数：

- `loop`：布尔值，用于确定动画是否以连续循环播放。如果设置为 _false_，则动画只播放一次然后停止播放。
- `playing`：布尔值，用于确定当前是否正在播放动画。
- `speed`：一个确定动画播放速度的数字，默认等于 _1_。可以将其设置得更高或更低，以使动画播放更快或更慢。
- `weight`：允许单个模型一次执行多个动画，计算动画中涉及的所有动作的加权平均值。 `weight` 的值决定了动画在平均值中的重要程度。默认情况下等于 _1_，但是不能高于 _1_。

在创建 `AnimationClip` 时，构造函数的第二个可选参数，可以将动画剪辑的所有参数值作为对象传递：


```ts
const clipSwim = new AnimationClip("swim", {
  loop: true,
  speed: 3,
  weight: 0.2
})
```

您还可以使用 `setParams()` 函数修改现有 `AnimationClip` 参数，使用一个包含您想要更改的参数值的对象：


```ts
clipSwim.setParams({ loop: true, speed: 3, weight: 0.2 })
```

#### 关于 weight 参数


The `weight` value of all active animations should add up to 1 at all times. If it adds up to less than 1, the weighted average will be referencing the default position of the armature for the remaining part of the calculation.

<!-- 所有活动动画的 `weight` 值合计应始终为 1。如果小于 1，其加权平均值就是剩余部分的骨骼支架的默认位置。-->

```ts
const clipSwim = new AnimationClip("swim", {
  weight: 0.2
})
const clipBite = new AnimationClip("bite", {
  weight: 0.8
})
shark.get(GLTFShape).addClip(clipSwim)
shark.get(GLTFShape).addClip(clipBite)

clipSwim.play()
```

For example, in the code example above, we're only playing the _swim_ animation, that has a `weight` of _0.2_. In this case the swimming movement is quite subtle: only 20% of what the animation says it should move. The other 80% of what's being averaged to get the final armature position is the default posture of the armature.

<!--
例如，在上面的代码示例中，我们只播放 `weight` 为 _0.2_ 的 _swim_ 动画。在这种情况下，游泳的速度很慢：只有动画设定移动的 20％。被平均的另外 80% 是骨骼支架的默认位置。
-->

可以以有趣的方式使用 `weight` 属性，例如 _swim_ 的 `weight` 属性可以与鲨鱼游动的速度成比例设置，因此您不需要为快速和慢速游泳创建多个动画。

您还可以在动画开始时或停止时逐步更改 `weight` 值，使得转换更为自然，并避免从默认位置跳转到动画中的第一个位置。
