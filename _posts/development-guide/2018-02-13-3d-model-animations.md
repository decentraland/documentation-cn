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

在 _.glTF_ 和 _.glb_ 格式的 3D 模型中可以包含任意多的动画。动画告诉网格如何移动，通过指定一系列随时间推移而设置的 _keyframes_，网格就可以从一个造型转变到另一个造型，从而模拟连续的运动。

大多数3D模型动画都是[_骨骼动画_](https://en.wikipedia.org/wiki/Skeletal_animation)。 这些动画将具有复杂几何形状的模型简化为“简笔画”，将网格中的每个顶点链接到最接近的_骨骼_中的骨头上，建模者调整骨架形成不同的造型，网格随着这些运动伸展和弯曲。

作为一种替代方法，_顶点动画_不需要骨架就可以对模型进行动画。这些动画直接指定模型中每个顶点的位置。Decentraland 也支持这些动画。

有关如何创建模型和动画的详细信息，请参阅[动画]({{ site.baseurl }}{% post_url /3d-modeling/2018-01-13-animations %})。阅读[形状组件]({{ site.baseurl }}{% post_url /development-guide/2018-02-6-shape-components %})，了解有关如何将 3D 模型导入场景的说明。

> 提示：动画通常更适合移动某个位置，而不是更改实体的位置。 例如，您可以设置动画以将角色的脚移动到位，但是要更改实体的位置，最好使用 “Transform” 组件。 有关详细信息，请参阅[定位实体]（{{site.baseurl}} {％post_url / development-guide / 2018-02-12-move-entities％}）。

## 检查 3D 模型

并不是所有 _glTF_ 文件都包含动画。要查看其中是否有动画，您可以执行以下操作：

- 如果使用 [VS Code](https://code.visualstudio.com/)（推荐），请安装 _GLTF Tools_ 扩展，然后在那里查看 glTF 文件的内容。
- 打开 [Babylon Sandbox](https://sandbox.babylonjs.com/)，然后拖放 glTF 文件（及所有依赖的 _.jpg_ 或 _.bin_）。
- 使用文本编辑器打开 _.glTF_ 文件，向下滚动直到找到 _"animations":_。

> 提示：在骨骼动画中，动画名称一般由其骨骼支架名称，下划线及其动画名称组成。例如 `myArmature_animation1`。

## 添加动画

`Animator` 组件管理实体的所有动画。 每个动画都由 `AnimationState` 对象处理。

![](/images/media/ecs-animations.png)

```ts
// Create entity
let shark = new Entity()

// Add a 3D model to it
shark.addComponent(new GLTFShape("models/shark.gltf"))

// Create animator component
let animator = new Animator()

// Add animator component to the entity
shark.addComponent(animator)

// Instance animation clip object
const clipSwim = new AnimationState("swim")

// Add animation clip to Animator component
animator.addClip(clipSwim)
```

也可以使用较少的语句实现：

```ts
// Create and add animator component
shark.addComponent(new Animator())

// Instance and add a clip
shark.getComponent(Animator).addClip(new AnimationState("swim"))
```

可以使用 `getClip()` 函数从 `Animator` 组件中得到 `AnimationState` 对象。

```ts
// Create and get a clip
let clipSwim = animator.getClip("swim")
```

`AnimationState` 对象不存储进入动画的实际转换，这些都在 .glTF 文件中。 相反，`AnimationState` 对象具有一个状态来跟踪它沿动画前进的程度。

## 获取动画

如果没有剪辑对象的引用，可以使用名称调用 `getClip()` 函数从 `Animator` 中获取剪辑。

```ts
// Create and add a clip
shark.getComponent(Animator).addClip(new AnimationState("swim"))

// Fetch the clip
shark.getComponent(Animator).getClip("swim")
```

<!--
... which one is true?

> Note: If you attempt to use `getClip()` to fetch a clip that doesn't exist in the Animator component, it returns `null`.

If you try to get an `AnimationState` that was never added to the `Animator` component, the clip is created and added automatically.
-->

## 播放动画

一旦 `AnimationState` 创建，默认状态为暂停。

播放或暂停最简单的方式是使用 `AnimationState` 的 `play()` 和 `pause()` 方法。

```ts
// Create animation clip
const clipSwim = new AnimationState("swim")

// Start playing the clip
clipSwim.play()

// Pause the playing of the clip
clipSwim.pause()
```

`AnimationState` 对象也有一个 `playing` 布尔参数。您可以通过更改此参数的值来启动或停止动画。

```ts
clipSwim.playing = true
```

如果动画当前处于暂停状态，则 `play()` 函数将恢复之前播放的最后一帧的动画。如果您想从头开始播放动画，请使用 `restart()` 函数。

```ts
clipSwim.restart()
```

## 动画循环

默认情况下，动画是在一个循环中播放的，这个循环会一直重复播放动画。

可以用 `AnimationState` 对象的 `looping` 属性来更改此设置。

```ts
// Create animation clip
const clipSwim = new AnimationState("swim")

// Set loop to false
clipSwim.looping = false

// Start playing the clip
clipSwim.play()
```

如果 `looping` 设置为 _false_，则动画播放一次后即停止。

> 注意：非循环动画播放完毕后，即使 3D 模型保持静止，它仍将继续处于 `playing = true` 状态。 如果您尝试播放其他动画，这可能会给您带来麻烦。 在播放动画之前，请确保已停止所有其他动画，包括已完成的非循环动画。

## 重置动画

当动画完成播放或暂停时，3D 模型将停留在其最后的姿势上。

要停止动画并回到动画中的第一帧，请使用 `AnimationState` 对象的 `stop()` 函数。

```ts
clipSwim.stop()
```

无论动画当前在哪个帧中，如果要从开始处播放动画，都要使用 `AnimationState` 对象的 `restart()` 函数。

```ts
clipSwim.restart()
```

> 注意：重置动画是一种突然的变化。如果您想让模型顺利过渡，您可以：
    - 设置动画的 `weight` 属性为 0，然后逐渐增加 `weight`
    - 创建一个动画片段，用来描述两个造型姿势间的移动。

## 具有多个动画的模型

动画中的骨骼一次只受一个动画的影响，除非这些动画的 `weight` 加起来等于或小于1。

如果一个动画只影响角色的腿，而另一个动画只影响角色的头，那么它们可以同时播放，没有任何问题。但是如果它们都影响角色的腿，那么你必须一次只播放一个，或者用更低的 `weight` 值来播放它们。

> 注意：非循环动画播放完毕后，即使 3D 模型保持静止，它仍将继续处于 `playing = true` 状态。 如果您尝试播放其他动画，这可能会给您带来麻烦。 在播放动画之前，请确保已停止所有其他动画，包括已完成的非循环动画。

## 动画速度

通过更改 `speed` 属性来更改播放动画的速度。 默认情况下，速度值为 1。


```ts
// Create animation clip
const clipSwim = new AnimationState("swim")

// Set speed to twice as fast
clipSwim.speed = 2

// Start playing the clip
clipSwim.play()
```

将速度设置为低于 1 以使其慢速播放，例如设置为 0.5 便以一半的速度播放。 将其设置为高于 1 可以更快地播放，例如设置为 2 可以以两倍的速度播放。

#### 动画 weight 属性

`weight` 属性允许单个模型一次执行多个动画，计算动画中涉及的所有动作的加权平均值。 `weight` 的值决定了动画在平均值中的重要程度。

默认情况下，`weight` 等于 _1_，它不能高于 _1_。


```ts
// Create animation clip
const clipSwim = new AnimationState("swim")

// Set weight
clipSwim.weight = 0.5

// Start playing the clip
clipSwim.play()
```

The `weight` value of all active animations in an entity should add up to 1 at all times. If it adds up to less than 1, the weighted average will be using the default position of the armature for the remaining part of the calculation.

<!-- 所有活动动画的 `weight` 值合计应始终为 1。如果小于 1，其加权平均值就是剩余部分的骨骼支架的默认位置。-->

```ts
const clipSwim = new AnimationState("swim")
clipSwim.weight = 0.2

animator.addClip(clipSwim)

clipSwim.play()
```

For example, in the code example above, we're playing the _swim_ animation, that only has a `weight` of _0.2_. This swimming movement will be quite subtle: only 20% of what the animation defines. The remaining 80% of the calculation takes values from the default posture of the armature.

<!--
例如，在上面的代码示例中，我们只播放 `weight` 为 _0.2_ 的 _swim_ 动画。在这种情况下，游泳的速度很慢：只有动画设定移动的 20％。被平均的另外 80% 是骨骼支架的默认位置。
-->

可以以有趣的方式使用 `weight` 属性，例如 _swim_ 的 `weight` 属性可以与鲨鱼游动的速度成比例设置，因此您不需要为快速和慢速游泳创建多个动画。

您还可以在动画开始时或停止时逐步更改 `weight` 值，使得转换更为自然，并避免从默认位置跳转到动画中的第一个位置。

> 注意：所有作用于 3D 模型骨骼上的动画的 `weight` 值加起来不能超过 1。如果有多个动画同时影响相同的骨骼，则需要将它们的权重设置为加起来小于 1 的值。

## 批量设置剪辑参数

使用 `AnimationState` 对象的 `setParams()` 函数一次设置多个参数。

可以配置以下参数：

- `playing`：布尔值，用于确定当前是否正在播放动画。
- `looping`：布尔值，用于确定动画是否以连续循环播放。
- `speed`：一个数字，用于确定播放动画的速度。
- `weight`：用于使用加权平均值混合动画。

```ts
const clipSwim = new AnimationState("swim")

clipSwim.setParams({playing:true, looping:true, speed: 2, weight: 0.5})
```

## 共享形状组件的动画

您可以在多个实体上使用 `GLTFShape` 组件的相同实例来节省资源。 如果每个实体都有自己的 `Animator` 组件和它自己的 `AnimationState` 对象，那么它们每个都可以独立动画。

```ts
//create entities
let shark1 = new Entity()
let shark2 = new Entity()

// create reusable shape component
let sharkShape = new GLTFShape('models/shark.gltf')

// Add the same GLTFShape instance to both entities
shark1.addComponent(sharkShape)
shark2.addComponent(sharkShape)

// Create separate animator components
let animator1 = new Animator()
let animator2 = new Animator()

// Add separate animator components to the entities
shark1.addComponent(animator1)
shark2.addComponent(animator2)

// Instance separate animation clip objects
const clipSwim1 = new AnimationState("swim")
const clipSwim2 = new AnimationState("swim")

// Add animation clips to Animator components
animator1.addClip(clipSwim1)
animator2.addClip(clipSwim2)

engine.addEntity(shark1)
engine.addEntity(shark2)
```

> 注意：如果您定义一个 `AnimationState` 对象实例并将其添加到不同实体的多个 `Animator` 组件，则使用 `AnimationState` 实例的所有实体将同时进行动画处理。
