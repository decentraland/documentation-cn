---
date: 2018-02-12
title: 移动实体
description: 如何随着时间的推移进行增量更改以逐渐移动、旋转和缩放实体
categories:
  - development-guide
type: Document
set: development-guide
set_order: 12
---

To move, rotate or resize an entity in your scene, change the  _position_, _rotation_ and _scale_ values stored in an entity's `Transform` component incrementally, frame by frame. This can be used on primitive shapes (cubes, spheres, planes, etc) as well as on 3D models (glTF).

You can easily perform these incremental changes by moving entities a small amount each time the `update()` function of a [system]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %}) is called.

在场景中移动、旋转或调整实体大小，可以逐帧逐步更改实体的 `Transform` 组件中存储的 _position_, _rotation_ 和 _scale_ 值。此方法可用于基本形状（立方体，球体，平面等）以及 3D 模型（glTF）中。

可以在 [system]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %}) 的 `update()` 函数调用时，通过毎次少量移动实体来轻松地进行这些增量更改。

## Move
## 移动

The easiest way to move an entity is to use the `translate()` function to change the _position_ value stored in the `Transform` component.

移动实体的最简单方法是使用 `translate()` 函数来更改存储在 `Transform` 组件中的 _position_ 值。

```ts
export class SimpleMove {
  update() {
    let transform = myEntity.get(Transform)
    let distance = Vector3.Forward.scale(0.1)
    transform.translate(distance)
  }
}

engine.addSystem(new SimpleMove())

const myEntity = new Entity()
myEntity.set(new Transform())
myEntity.set(new BoxShape())

engine.addEntity(myEntity)
```

In this example we're moving an entity by 0.1 meters per frame. 

`Vector3.Forward()` returns a vector that faces forward and measures 1 meter in length. In this example we're then scaling this vector down to 1/10 of its length with `scale()`. If our scene has 30 frames per second, the entity is moving at 3 meters per second in speed.

在这个例子中，我们每帧移动实体 0.1 米。

`Vector3.Forward()` 返回一个面向前方长度为 1 米的向量。这个例子中，我们然后用 `scale()` 将这个向量缩小到它的长度的 1/10。如果我们的场景每秒有 30 帧，则实体会以每秒 3 米的速度移动。

## Adjust movement to delay time
## 根据延迟时间调整移动量

Suppose that the user running your scene is struggling to keep up with the pace of the frame rate. That could result in the movement appearing jumpy, as not all frames are evenly timed but each moves the entity in the same amount.

You can compensate for this uneven timing by using the `dt` parameter to adjust the scale the movement.

如果运行场景的客户端跟不上帧速率。就有可能导致运动出现跳跃，因为并非所有帧都是均匀定时的，但每个帧都以相同的量移动实体。

您可以使用 `dt` 参数来调整运动的比例来对这种不均匀的时间进行补偿。

```ts
export class SimpleMove {
  update(dt: number) {
    let transform = myEntity.get(Transform)
    let distance = Vector3.Forward.scale(dt * 3)
    transform.translate(distance)
  }
}
// (...)
```

The example above keeps movement at approximately the same speed as before, even if the frame rate drops. When running at 30 frames per second, the value of `dt` is 1/30 .

即使帧速率下降，上述示例仍将保持与之前大致相同的速度。当以每秒 30 帧的速度运行时，`dt` 的值为 1/30。

## Rotate
## 旋转

The easiest way to rotate an entity is to use the `rotate()` function to change the values in the Transform component incrementally, and run this as part of the `update()` function of a system.

旋转实体的最简单方法是使用 `rotate()` 函数逐步更改 Transform 组件中的值，并将其作为系统 `update()` 函数的一部分运行。

The `rotate()` function takes two arguments:

- The direction in which to rotate (as a _Vector3_)
- The amount to rotate, in [euler](https://en.wikipedia.org/wiki/Euler_angles) degrees (from 0 to 360)

`rotate()` 函数有两个参数：

- 旋转的方向（_Vector3_）
- 旋转量，[euler](https://en.wikipedia.org/wiki/Euler_angles) 度数（从 0 到 360）

```ts
export class SimpleRotate {
  update() {
    let transform = myEntity.get(Transform)
    let distance = dt * 3
    transform.rotate(Vector3.Left(), distance)
  }
}

engine.addSystem(new SimpleRotate())
```

> Tip: To make an entity always rotate to face the user, you can use the `billboardMode` setting. See [Set entity poision]({{ site.baseurl }}{% post_url /development-guide/2018-01-12-entity-positioning %}#face-the-user) for details.

> 提示：要使实体始终旋转以面向用户，可以使用 `billboardMode` 设置。有关详细信息，请参阅[实体位置设置]({{ site.baseurl }}{% post_url /development-guide/2018-01-12-entity-positioning %}#face-the-user)。

## Rotate over a pivot point
## 旋转轴心

When rotating an entity, the rotation is always in reference to the entity's center coordinate. To rotate an entity using another set of coordinates as a pivot point, create a second (invisible) entity with the pivot point as its position and make it a parent of the entity you with to rotate.

When rotating the parent entity, its children will be all rotated using the parent's position as a pivot point. Note that the `position` of the child entity is in reference to that of the parent entity.

旋转实体时，会始终按照实体的中心坐标旋转。要使用另一组坐标作为轴心点旋转实体，请创建第二个（不可见）实体，位置设为将要旋转的轴心点，并使其成为要旋转的实体的父实体。

旋转父实体时，其子项将全部使用父项的位置作为轴心点旋转。请注意，子实体的 `position` 是引用了父实体的 `position`。

```ts
// Create entity you wish to rotate
const door = new Entity()

// Create the pivot entity
const pivot = new Entity()

// Position the pivot entity on the pivot point of the rotation
pivot.set(new Transform({
  position: new Vector3(4, 1, 3)
}))

// Set pivot as the parent
door.setParent(pivot)

// Position child in reference to parent
door.set(new Transform({
  position: new Vector3(0.5, 0, 0)
}))

// Rotate the parent. The child rotates using the parent's location as a pivot point.
pivot.get(Transform).rotation.setEuler(0, 90, 0)

// Add both entities to the engine
engine.addEntity(door)
engine.addEntity(pivot)

// Define a system that updates the rotation on every frame
export class PivotRotate {
  update() {
    let transform = myEntity.get(pivot)
    let distance = dt * 3
    transform.rotate(Vector3.Left(), distance )
  }
}

// Add the system
engine.addSystem(new PivotRotate())
```

Note that in this example, the system is rotating the `pivot` entity, that's a parent of the `door` entity.

请注意，在此示例中，系统正在旋转 `pivot` 实体，该实体是 `door` 实体的父实体。

## Move between two points
## 两点间移动

If you want an entity to move smoothly between two points, use the _lerp_ (linear interpolation) algorithm. This algorithm is very well known in game development, as it's really useful.

如果要实体在两点之间平滑移动，请使用 _lerp_（线性插值）算法。这种算法在游戏开发中非常有名，因为它非常有用。

The `lerp()` function takes three parameters:

- The vector for the origin position
- The vector for the target position
- The amount, a value from 0 to 1 that represents what fraction of the translation to do.

`lerp()` 函数有三个参数：

- 原点位置的向量
- 目标位置的向量
- 数值，从 0 到 1 的值，表示要做的转换的比例。

```ts
const originVector = Vector3.Zero()
const targetVector = Vector3.Forward()

let newPos = Vector3.Lerp(originVector, targetVector, 0.6)
```

The linear interpolation algorithm finds an intermediate point in the path between both vectors that matches the provided amount.

线性插值算法在两个矢量之间的路径中找到与提供的量匹配的中间点。

For example, if the origin vector is _(0, 0, 0)_ and the target vector is _(10, 0, 10)_:

例如，如果原始向量是 _(0, 0, 0)_ 并且目标向量是 _(10, 0, 10)_：

- Using an amount of 0 would return _(0, 0, 0)_
- Using an amount of 0.3 would return _(3, 0, 3)_
- Using an amount of 1 would return _(10, 0, 10)_

- 0 将返回 _(0, 0, 0)_
- 使用 0.3 的数量将返回 _(3, 0, 3)_
- 使用 1 的数量将返回 _(10, 0, 10)_


To implement this `lerp()` in your scene, we recommend creating a custom component to store the necessary information. You also need to define a system that implements the gradual movement in each frame.

要在场景中实现 `lerp()`，我们建议您创建一个自定义组件来存储必要的信息。您还需要定义一个在每个帧中实现渐变运动的系统。

```ts
@Component("lerpData")
export class LerpData {
  origin: Vector3 = Vector3.Zero()
  target: Vector3 = Vector3.Zero()
  fraction: number = 0
}

// a system to carry out the movement
export class LerpMove {
  update(dt: number) {
    let transform = myEntity.get(Transform)
    let lerp = myEntity.get(LerpData)
    if (lerp.fraction < 1) {
      transform.position = Vector3.Lerp(
        lerp.origin,
        lerp.target,
        lerp.fraction
      )
      lerp.fraction += dt / 6
    }
  }
}

// Add system to engine
engine.addSystem(new LerpMove())

const myEntity = new Entity()
myEntity.set(new Transform())
myEntity.set(new BoxShape())

myEntity.set(new LerpData())
myEntity.get(LerpData).origin = new Vector3(1, 1, 1)
myEntity.get(LerpData).target = new Vector3(8, 1, 3)

engine.addEntity(myEntity)
```

## Rotate between two angles
## 在两个角度间旋转

To rotate smoothly between two angles, use the _slerp_ (_spherical_ linear interpolation) algorithm. This algorithm is very similar to a _lerp_, but it handles quaternion rotations.

要在两个角度之间平滑旋转，请使用 _slerp_（_球形_ 线性插值）算法。此算法与 _lerp_ 非常相似，但它处理四元数旋转。

The `slerp()` function takes three parameters:

- The [quaternion](https://en.wikipedia.org/wiki/Quaternion) angle for the origin rotation
- The [quaternion](https://en.wikipedia.org/wiki/Quaternion) angle for the target rotation
- The amount, a value from 0 to 1 that represents what fraction of the translation to do.

`slerp()` 函数有三个参数：

- 旋转原点的[四元数](https://en.wikipedia.org/wiki/Quaternion)角度
- 旋转目标的[四元数](https://en.wikipedia.org/wiki/Quaternion)角度
- 数值，一个从 0 到 1 的值，表示要转换的比例。


> Tip: You can pass rotation values in [euler](https://en.wikipedia.org/wiki/Euler_angles) degrees (from 0 to 360) by using `Quaternion.Euler()`.

> 提示：使用[euler](https://en.wikipedia.org/wiki/Euler_angles) 角度（从 0 到 360 ）中传递旋转值，可以用 `Quaternion.Euler()`


```ts
const originRotation = Quaternion.Euler(0, 90, 0)
const targetRotation = Quaternion.Euler(0, 0, 0)

let newRotation = Scalar.Lerp(originRotation, targetRotation, 0.6)
```

To implement this in your scene, we recommend storing the data that goes into the `Slerp()` function in a custom component. You also need to define a system that implements the gradual rotation in each frame.

要在场景中实现这一点，我们建议在自定义组件中存储进入 `Slerp()` 函数的数据。您还需要定义一个在每个帧中实现渐变的系统。


```ts
@Component('slerpData')
export class SlerpData {
  originRot: Quaternion = Quaternion.Euler(0, 90, 0)
  targetRot: Quaternion = Quaternion.Euler(0, 0, 0)
  fraction: number = 0
}

// a system to carry out the rotation
export class SlerpRotate implements ISystem {
 
  update(dt: number) {
      let slerp = myEntity.get(SlerpData)
      let transform = myEntity.get(Transform)

      slerp.fraction += dt
      let rot = Quaternion.Slerp(slerp.originRot, slerp.targetRot, slerp.fraction)
      transform.rotation = rot   
  }
}

// Add system to engine
engine.addSystem(new SlerpRotate())

const myEntity = new Entity()
myEntity.set(new Transform())
myEntity.set(new BoxShape())

myEntity.set(new SlerpData())
myEntity.get(SlerpData).originRot = Quaternion.Euler(0, 90, 0)
myEntity.get(SlerpData).targetRot = Quaternion.Euler(0, 0, 0)

engine.addEntity(myEntity)
```

> Note: You could instead represent the rotation with `Vector3` values and use a `Lerp()` function, but that would imply a conversion from `Vector3` to `Quaternion` on each frame. Rotation values are internally stored as quaternions in the `Transform` component, so it's more efficient to work with quaternions.

> 注意：您可以使用 `Vector3` 代替 `Lerp()` 函数表示旋转，但这意味着每帧需要将 `Vector3` 转换到 `Quaternion` 。因为旋转值在 `Transform` 组件中内部存储为四元数，因此使用四元数更有效率。

## Change scale between two sizes
## 两种尺寸间的比例

If you want an entity to change size smoothly and without changing its proportions, use the _lerp_ (linear interpolation) algorithm of the `Scalar` object. 

Otherwise, if you want to change the axis in different proportions, use `Vector3` to represent the origin scale and the target scale, and then use the _lerp_ function of the `Vector3`.

如果希望实体平滑地改变大小而不改变其比例，请使用 `Scalar` 对象的 _lerp_（线性插值）算法。

否则，如果要在各轴中使用不同的比例，请使用 `Vector3` 表示原始比例和目标比例，然后使用 `Vector3` 的 _lerp_ 函数。

The `lerp()` function of the `Scalar` object takes three parameters:

- A number for the origin scale
- A number for the target scale
- The amount, a value from 0 to 1 that represents what fraction of the scaling to do.

`Scalar` 对象的 `lerp()` 函数有三个参数：

- 原始比例值
- 目标比例值
- 数值，从 0 到 1 的值，表示要进行的缩放比例。

```ts
const originScale = 1
const targetScale = 10

let newScale = Scalar.Lerp(originScale, targetScale, 0.6)
```
To implement this lerp in your scene, we recommend creating a custom component to store the necessary information. You also need to define a system that implements the gradual scaling in each frame.

要在场景中实现此 lerp ，我们建议您创建一个自定义组件来存储必要的信息。您还需要定义一个在每帧中实现渐变缩放的系统。

## Move at irregular speeds between two points
## 在两点之间以不规则的速度移动

While using the lerp method, you can make the movement speed non-linear. In the previous example we increment the lerp amount by a given amount each frame, but we could also use a mathematical function to increase the number exponentially or in other measures that give you a different movement pace.

You could also use a function that gives recurring results, like a sine function, to describe a movement that comes and goes.

使用 lerp 方法时，也可以使移动以非线性速度进行。在前面的示例中，我们将每个帧的 lerp 量增加给定量，但我们也可以使用数学函数以指数方式增加数量，或者使用其他方式来增加运动速度。

您还可以使用一个提供重复结果的函数，如正弦函数，用以描述来来去去的运动。

```ts
@Component("lerpData")
export class LerpData {
  origin: Vector3 = Vector3.Zero()
  target: Vector3 = Vector3.Zero()
  fraction: number = 0
  time: number = 0
}

export class LerpMove {
  update(dt: number) {
    let transform = lerpEntity.get(Transform)
    let lerp = lerpEntity.get(LerpData)
    lerp.time += dt / 6
    lerp.fraction = Math.sin(lerp.time)
    transform.position = Vector3.Lerp(
      lerp.origin,
      lerp.target,
      lerp.fraction
    )
  }
}
```

The example above adds a `time` field to the custom component. The `time` field is incremented on every frame, and then `fraction` is set to the _sin_ of that value. Because of the nature of the _sin_ operation, the entity will lerp back and forth between both points.

上面的示例将 `time` 字段添加到自定义组件中。 每帧 `time` 值递增，然后 `fraction` 被设置为该值的 _正弦函数_。由于 _sin_ 函数的性质，实体将在两个点之间来回反复。

## Follow a path
## 沿着路径移动

A `Path3` object stores a series of vectors that describe a path. You can have an entity loop over the list of vectors, performing a lerp movement between each.

`Path3` 对象存储一系列描述路径的向量，实体可以在向量列表中循环，在每个向量之间执行 lerp 移动。

```ts
const point1 = new Vector3(1, 1, 1)
const point2 = new Vector3(8, 1, 3)
const point3 = new Vector3(8, 4, 7)
const point4 = new Vector3(1, 1, 7)

const myPath = new Path3D([point1, point2, point3, point4])

@Component("pathData")
export class PathData {
  origin: Vector3 = myPath.path[0]
  target: Vector3 = myPath.path[1]
  fraction: number = 0
  nextPathIndex: number = 1
}

export class PatrolPath {
  update(dt: number) {
    let transform = myEntity.get(Transform)
    let path = myEntity.get(PathData)
    if (path.fraction < 1) {
      transform.position = Vector3.Lerp(
        path.origin,
        path.target,
        path.fraction
      )
      path.fraction += dt / 6
    } else {
      path.nextPathIndex += 1
      if (path.nextPathIndex >= myPath.path.length) {
        path.nextPathIndex = 0
      }
      path.origin = path.target
      path.target = myPath.path[path.nextPathIndex]
      path.fraction = 0
    }
  }
}

engine.addSystem(new PatrolPath())

const myEntity = new Entity()
myEntity.set(new Transform())
myEntity.set(new BoxShape())
myEntity.set(new PathData())

engine.addEntity(myEntity)
```

The example above defines a 3D path that's made up of four 3D vectors. We also define a custom `PathData` component, that includes the same data used by the custom component in the _lerp_ example above, but adds a `nextPathIndex` field to keep track of what vector to use next from the path.

The system is very similar to the system in the _lerp_ example, but when a lerp action is completed, it sets the `target` and `origin` fields to new values. If we reach the end of the path, we return to the first value in the path.

上面的示例定义了由四个 3D 矢量组成的 3D 路径。我们还定义了一个自定义的 `PathData` 组件，它包含与上面 _lerp_ 示例中自定义组件使用的相同数据，但添加了一个 `nextPathIndex` 字段来跟踪路径中接下来要使用的向量。

该系统与 _lerp_ 示例中的系统非常相似，但是当一个 lerp 动作完成时，它将 `target` 和`origin` 字段设置为新值。如果我们到达路径的末尾，我们返回使用路径中的第一个值。

<!--

## Move along curves

... investigate

-->
