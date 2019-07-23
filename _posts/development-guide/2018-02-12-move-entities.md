---
date: 2018-02-12
title: 移动实体
description: 如何随着时间的推移逐渐移动、旋转和缩放实体
categories:
  - development-guide
type: Document
set: development-guide
set_order: 12
---

要在场景中移动、旋转或调整实体大小，可以逐帧逐渐更改实体的 `Transform` 组件中存储的 _position_, _rotation_ 和 _scale_ 值。这种方法可用在基本形状（立方体，球体，平面等）以及 3D 模型（glTF）中。

可以在 [system]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %}) 的 `update()` 函数调用时，通过毎次少量移动实体来轻松地执行这些增量更改。

## 移动

移动实体的最简单方法是使用 `translate()` 函数来更改存储在 `Transform` 组件中的_ position_ 值。

```ts
export class SimpleMove implements ISystem {
  update() {
    let transform = myEntity.getComponent(Transform)
    let distance = Vector3.Forward().scale(0.1)
    transform.translate(distance)
  }
}

engine.addSystem(new SimpleMove())

const myEntity = new Entity()
myEntity.addComponent(new Transform())
myEntity.addComponent(new BoxShape())

engine.addEntity(myEntity)
```

在这个例子中，我们每帧移动实体 0.1 米。

`Vector3.Forward()` 返回面向前方长度为 1 米的向量。这个例子中，我们然后用 `scale()` 将这个向量缩小到它的长度的 1/10。如果我们的场景每秒有 30 帧，则实体会以每秒 3 米的速度移动。

<img src="/images/media/gifs/move.gif" alt="Move entity" width="300"/>

## 旋转

旋转实体的最简单方法是使用 `rotate()` 函数逐步更改 Transform 组件中的值，并将其作为系统 `update()` 函数的一部分运行。

`rotate()` 函数有两个参数：
- 旋转的方向（_Vector3_）
- 旋转量，[euler](https://en.wikipedia.org/wiki/Euler_angles) 度数（从 0 到 360）

```ts
export class SimpleRotate implements ISystem {
  update() {
    let transform = myEntity.getComponent(Transform)
    transform.rotate(Vector3.Left(), 3)
  }
}

engine.addSystem(new SimpleRotate())

const myEntity = new Entity()
myEntity.addComponent(new Transform())
myEntity.addComponent(new BoxShape())

engine.addEntity(myEntity)
```

> 提示：要使实体始终旋转以面向用户，您可以添加[`Billboard` 组件]({{ site.baseurl }}{% post_url /development-guide/2018-01-12-entity-positioning %}#face-the-user)。

<img src="/images/media/gifs/rotate.gif" alt="Move entity" width="300"/>

## 旋转轴心

旋转实体时，会始终按照实体的中心坐标旋转。要使用另一组坐标作为轴心点旋转实体，请创建第二个（不可见）实体，位置设为将要旋转的中心点，并使其成为要旋转的实体的父实体。

旋转父实体时，其子项将全部使用父项的位置作为轴心点旋转。请注意，子实体的 `position` 是引用了父实体的 `position`。

```ts

// Create entity you wish to rotate
const myEntity = new Entity()

myEntity.addComponent(new BoxShape())

// Create the pivot entity
const pivot = new Entity()

// Position the pivot entity on the pivot point of the rotation
pivot.addComponent(new Transform({
  position: new Vector3(3, 2, 3)
}))

// add pivot entity
engine.addEntity(pivot)

// Set pivot as the parent
myEntity.setParent(pivot)

// Position child in reference to parent
myEntity.addComponent(new Transform({
  position: new Vector3(0, 0.5, 0.5)
}))

// Add child entity to the engine
engine.addEntity(myEntity)


// Define a system that updates the rotation on every frame
export class PivotRotate implements ISystem {
  update() {
    let transform = pivot.getComponent(Transform)
    transform.rotate(Vector3.Left(), 3 )
  }
}

// Add the system
engine.addSystem(new PivotRotate())
```

请注意，在此示例中，系统正在旋转 `pivot` 实体，该实体是 `door` 实体的父实体。

<img src="/images/media/gifs/pivot-rotate.gif" alt="Move entity" width="300"/>

## 根据延迟时间调整移动量

如果运行场景的客户端跟不上帧速率。就有可能导致运动出现跳跃，因为并非所有帧都是均匀定时的，但每帧移动实体的量是一样的。

您可以使用 `dt` 参数来调整运动的比例来对这种不均匀的时间补偿。

```ts
export class SimpleMove implements ISystem {
  update(dt: number) {
    let transform = myEntity.get(Transform)
    let distance = Vector3.Forward.scale(dt * 3)
    transform.translate(distance)
  }
}
// (...)
```

即使帧速率下降，上述示例仍将保持与之前大致相同的速度。当以每秒 30 帧的速度运行时，`dt` 的值为 1/30。

## 两点间移动

如果要实体在两点之间平滑移动，请使用 _lerp_（线性插值）算法。这种算法在游戏开发中非常有名，因为它非常有用。

`lerp()` 函数有三个参数：

- 原点位置的向量
- 目标位置的向量
- 数值，从 0 到 1 的值，表示要做的转换的比例。

```ts
const originVector = Vector3.Zero()
const targetVector = Vector3.Forward()

let newPos = Vector3.Lerp(originVector, targetVector, 0.6)
```

线性插值算法在两个矢量之间的路径中找到与提供的量匹配的中间点。

例如，如果原始向量是 _(0, 0, 0)_ 并且目标向量是 _(10, 0, 10)_：

- 数值使用 0 将返回 _(0, 0, 0)_
- 使用 0.3 的数量将返回 _(3, 0, 3)_
- 使用 1 的数量将返回 _(10, 0, 10)_

要在场景中实现 `lerp()`，我们建议您创建一个自定义组件来存储必要的信息。您还需要定义一个在每个帧中实现渐变运动的系统。

```ts
@Component("lerpData")
export class LerpData implements ISystem {
  origin: Vector3 = Vector3.Zero()
  target: Vector3 = Vector3.Zero()
  fraction: number = 0
}

// a system to carry out the movement
export class LerpMove {
  update(dt: number) {
    let transform = myEntity.getComponent(Transform)
    let lerp = myEntity.getComponent(LerpData)
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
myEntity.addComponent(new Transform())
myEntity.addComponent(new BoxShape())

myEntity.addComponent(new LerpData())
myEntity.getComponent(LerpData).origin = new Vector3(1, 1, 1)
myEntity.getComponent(LerpData).target = new Vector3(8, 1, 3)

engine.addEntity(myEntity)
```

<img src="/images/media/gifs/lerp-move.gif" alt="Move entity" width="300"/>

## 在两个角度间旋转

要在两个角度之间平滑旋转，请使用 _slerp_（_球形_ 线性插值）算法。此算法与 _lerp_ 非常相似，但它处理四元数旋转。

`slerp()` 函数有三个参数：
- 旋转原点的[四元数](https://en.wikipedia.org/wiki/Quaternion)角度
- 旋转目标的[四元数](https://en.wikipedia.org/wiki/Quaternion)角度
- 数值，一个从 0 到 1 的值，表示要转换的比例。

> 提示：使用[euler](https://en.wikipedia.org/wiki/Euler_angles) 角度（从 0 到 360 ）中传递旋转值，可以用 `Quaternion.Euler()`


```ts
const originRotation = Quaternion.Euler(0, 90, 0)
const targetRotation = Quaternion.Euler(0, 0, 0)

let newRotation = Scalar.Lerp(originRotation, targetRotation, 0.6)
```

要在场景中实现这一点，我们建议在自定义组件中存储进入 `Slerp()` 函数的数据。您还需要定义一个在每个帧中实现渐变的系统。


```ts
@Component('slerpData')
export class SlerpData implements ISystem {
  originRot: Quaternion = Quaternion.Euler(0, 90, 0)
  targetRot: Quaternion = Quaternion.Euler(0, 0, 0)
  fraction: number = 0
}

// a system to carry out the rotation
export class SlerpRotate implements ISystem {
 
  update(dt: number) {
      let slerp = myEntity.getComponent(SlerpData)
      let transform = myEntity.getComponent(Transform)
      if (slerp.fraction < 1) {
        let rot = Quaternion.Slerp(slerp.originRot, slerp.targetRot, slerp.fraction)
        transform.rotation = rot  
        slerp.fraction += dt/5 
      }
  }
}

// Add system to engine
engine.addSystem(new SlerpRotate())

const myEntity = new Entity()
myEntity.addComponent(new Transform())
myEntity.addComponent(new BoxShape())

myEntity.addComponent(new SlerpData())
myEntity.getComponent(SlerpData).originRot = Quaternion.Euler(0, 90, 0)
myEntity.getComponent(SlerpData).targetRot = Quaternion.Euler(0, 0, 0)

engine.addEntity(myEntity)
```

> 注意：您可以使用 `Vector3` 代替 `Lerp()` 函数表示旋转，但这意味着每帧需要将 `Vector3` 转换到 `Quaternion` 。因为旋转值在 `Transform` 组件中内部存储为四元数，因此使用四元数更有效率。

<img src="/images/media/gifs/lerp-rotate.gif" alt="Move entity" width="300"/>

## 平滑缩放

如果希望实体平滑地改变大小而不改变其比例，请使用 `Scalar` 对象的 _lerp_（线性插值）算法。

否则，如果要在各轴中使用不同的比例，请使用 `Vector3` 表示原始比例和目标比例，然后使用 `Vector3` 的 _lerp_ 函数。

`Scalar` 对象的 `lerp()` 函数有三个参数：

- 原始比例值
- 目标比例值
- 数值，从 0 到 1 的值，表示要进行的缩放比例。


```ts
const originScale = 1
const targetScale = 10

let newScale = Scalar.Lerp(originScale, targetScale, 0.6)
```

要在场景中实现此 lerp ，我们建议您创建一个自定义组件来存储必要的信息。您还需要定义一个在每帧中实现渐变缩放的系统。

```ts
@Component("lerpData")
export class LerpSizeData {
  origin: number = 0.1
  target: number = 2
  fraction: number = 0
}

// a system to carry out the movement
export class LerpSize implements ISystem {
  update(dt: number) {
    let transform = myEntity.getComponent(Transform)
    let lerp = myEntity.getComponent(LerpSizeData)
    if (lerp.fraction < 1) {
      let newScale = Scalar.Lerp(
        lerp.origin,
        lerp.target,
        lerp.fraction
      )
      transform.scale.setAll(newScale)
      lerp.fraction += dt / 6
    }
  }
}

// Add system to engine
engine.addSystem(new LerpSize())


const myEntity = new Entity()
myEntity.addComponent(new Transform())
myEntity.addComponent(new BoxShape())

myEntity.addComponent(new LerpSizeData())
myEntity.getComponent(LerpSizeData).origin = 0.1
myEntity.getComponent(LerpSizeData).target = 2

engine.addEntity(myEntity)
```

 <img src="/images/media/gifs/lerp-scale.gif" alt="Move entity" width="300"/>

## 在两点之间以不规则的速度移动

使用 lerp 方法时，也可以使移动以非线性速度进行。在前面的示例中，我们将每个帧的 lerp 量增加给定量，但我们也可以使用数学函数以指数方式增加数量，或者使用其他方式来增加运动速度。

您还可以使用一个提供重复结果的函数，如正弦函数，用以描述来来去去的运动。

```ts
@Component("lerpData")
export class LerpData {
  origin: Vector3 = Vector3.Zero()
  target: Vector3 = Vector3.Zero()
  fraction: number = 0
}

export class LerpMove implements ISystem {
  update(dt: number) {
    let transform = myEntity.getComponent(Transform)
    let lerp = myEntity.getComponent(LerpData)
    lerp.fraction += (dt + lerp.fraction)/10
    transform.position = Vector3.Lerp(
      lerp.origin,
      lerp.target,
      lerp.fraction
    )
  }
}

// Add system to engine
engine.addSystem(new LerpMove())
```

上面的示例跟之前的线性 lerp 示例很相似，但是 `fraction` 字段以非线性方式增加，导致曲线在每个帧上以更大的增量移动实体。

 <img src="/images/media/gifs/lerp-speed-up.gif" alt="Move entity" width="300"/>

## 沿着线路移动

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

export class PatrolPath implements ISystem {
  update(dt: number) {
    let transform = myEntity.getComponent(Transform)
    let path = myEntity.getComponent(PathData)
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
myEntity.addComponent(new Transform())
myEntity.addComponent(new BoxShape())
myEntity.addComponent(new PathData())

engine.addEntity(myEntity)
```

上面的示例定义了由四个 3D 矢量组成的 3D 路径。我们还定义了一个自定义的 `PathData` 组件，它包含与上面 _lerp_ 示例中自定义组件使用的相同数据，但添加了一个 `nextPathIndex` 字段来跟踪路径中接下来要使用的向量。

该系统与 _lerp_ 示例中的系统非常相似，但是当一个 lerp 动作完成时，它将 `target` 和`origin` 字段设置为新值。如果我们到达路径的末尾，我们返回使用路径中的第一个值。

 <img src="/images/media/gifs/lerp-path.gif" alt="Move entity" width="300"/>


<!--

## Move along curves

... investigate

-->
