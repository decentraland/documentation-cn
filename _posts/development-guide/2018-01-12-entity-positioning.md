---
date: 2018-02-5
title: 实体定位
description: 设置场景中实体的位置、旋转和缩放
categories:
  - development-guide
redirect_from:
  - /documentation/entity-positioning/
type: Document
set: development-guide
set_order: 5
---

可以使用 `Transform` 组件设置实体的 _position_，_rotation_ 和 _scale_。并可在任何实体、或基本形状组件（立方体，球体，平面等）或 3D 模型组件（glTF，Obj）上使用。

<img src="/images/media/ecs-simple-components.png" alt="nested entities" width="400"/>

```ts
// Create a new entity
const ball = new Entity()

// Add a transform component to the entity
ball.addComponent(new Transform())
ball.getComponent(Transform).position.set(5, 1, 5)
ball.getComponent(Transform).scale.set(2, 2, 2)
```

为简洁起见，还可以创建一个 `Transform` 实体，并在一条语句中提供初始值，参数为可选的包含 _position_，_rotation_ 和 _scale_ 属性的对象。

```ts
myEntity.ddComponent(new Transform({ 
    position: new Vector3(5, 1, 5), 
    rotation: new Quaternion(0, 0, 0, 0),
    scale: new Vector3(2, 2, 2)
    }))
```

要在场景中移动、旋转或调整实体大小，需要逐帧逐步更改此组件上的值。有关详细信息和最佳做法，请参阅[移动实体]({{ site.baseurl }}{% post_url /development-guide/2018-02-12-move-entities %})。您还可以使用[utils 库](https://www.npmjs.com/package/decentraland-ecs-utils)中的辅助函数来更轻松地实现此目的。

## 定位

`position` 是一个_3D 向量_，是实体中心在所有三个轴上的位置，存储为 `Vector3` 对象。


```ts
// Create transform with a predefined position
let myTransform = new Transform({position: new Vector3(1, 0, 1)})

// Set each axis individually
myTransform.position.x = 3
myTransform.position.y = 1
myTransform.position.z = 3

// Set the position with three numbers (x, y, z)
myTransform.position.set(3, 1, 3)

// Set the position with an object
myTransform.position = new Vector3(5, 1, 5)
```

> 注意：使用对象来设置位置时，可以使用 `Vector3` 对象，也可以使用带有 _x_，_y_ 和 _z_ 字段的任何其他对象。

设置位置时，请注意以下事项：

- 位置向量中的数字单位为_米_(除非该实体是缩放实体的子实体)。

  > 注意：如果您正在定位一个缩放值不为 1 的父实体的子项，position 向量也会相应缩放。

- 一块土地组成的场景长 16 米 x 16 米。场景的中心(地面)位于`x:8, y:0, z:8`。如果场景由多块土地组成，那么中心将根据土地的排列而变化。

- `x:0, y:0, z:0` 位于场景基块的西南角，位于地面。

  > 提示：查看场景预览时，场景的（0,0,0）点会有一个坐标，并且每轴都设有标签以供参考。

  > 提示：用你的左手，食指(指向前方)是 _z_ 轴，中指(指向侧面)是 _x_ 轴，大拇指(指向上方)是 _y_ 轴。

  > 注意：您可以通过编辑 _scene.json_ 的 `base` 属性来更改场景的基块。


- 如果实体是另一个实体的子节点，那么 `x：0，y：0，z：0` 指的是其父实体的中心，无论其在场景中的哪个位置。

- 场景中的每个实体必须始终位于其地块范围内。如果实体超出边界，则会引发错误。

  > 提示：在预览模式下查看场景时，超出边界范围的实体会以_红色_突出显示。

- 场景的高度也有限制。场景使用的地块越多，所允许的高度越高。详细信息，请参阅[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})。

## 旋转

`rotation` 存储为[_四元数_](https://en.wikipedia.org/wiki/Quaternion)，由四个数字组成，_x_，_y_，_z_ 和 _w_。

```ts
// Create transform with a predefined rotation in Quaternions
let myTransform = new Transform({rotation: new Quaternion(0, 0, 0, 1)})

// Set rotation with four numbers (x, y, z, w)
myTransform.rotation.set(0, 0, 1, 0)

// Set rotation with a quaternion
myTransform.rotation = new Quaternion(1, 0, 0, 0)
```

也可以使用[欧拉角度](https://en.wikipedia.org/wiki/Euler_angles)设置旋转，用一个更为常见的 _x_，_y_ 和 _z_ 来表示，数字使用大多数人都熟悉的从 0 到360。可以使用以下方式使用欧拉角度：


```ts
// Create transform with a predefined rotation in Euler angles
let myTransform = new Transform({rotation: Quaternion.Euler(0, 90, 0)})

// Use the .setEuler() function
myTransform.rotation.setEuler(0, 90, 180)

// Set the `eulerAngles` field
myTransform.rotation.eulerAngles = new Vector3(0, 90, 0)
```

当使用 _3D vector_ 表示欧拉角时，_x_，_y_ 和 _z_ 表示该轴的旋转，以度为单位。 360 度为一整圈。

> 注意：如果使用 _Euler_ angles 欧拉角度设置旋转，则内部存储的旋转值仍为四元数。

查询实体的旋转度时，默认情况下返回四元数。要获得以欧拉角表示的旋转，使用 `.eulerAngles` 字段：

```ts
myEntity.getComponent(Transform).rotation.eulerAngles
```


## 面向用户旋转

将 _billboard_ 组件添加到实体，这样它会始终旋转实体来面对玩家。

Billboards 是 90 年代 3D 游戏中常用的技术，大部分实体是始终面向玩家的 2D 平面，但同样也可用于旋转 3D 模型。

```ts
let box = new Entity()
box.addComponent(new BoxShape())
box.addComponent(new Transform({
  position: new Vector3(5, 1, 5)
}))
box.addComponent(new Billboard())
engine.addEntity(box)
```

您可以选择旋转哪个轴。例如，如果一个立方体的 Billboard 只在 Y 轴上旋转，则用户在地面移动时它会转向玩家，但玩家将能够从上方或从下方查看。

创建 `Billboard` 组件时，三个可选参数 _x_、_y_ 和 _z_ 轴的布尔值。默认情况下，它们都是 `true`。

```ts
// rotate on all three axis
let FullBillboard = new Billboard())

// rotate only in the X axis
let XBillboard = new Billboard(true, false ,false)

// rotate only in theY axis
let YBillboard = new Billboard(false, true ,false)

// rotate only in the Z axis
let ZBillboard = new Billboard(false, false ,true)
```

提示：如果想要实体在地平面上跟随用户旋转，可设为沿 _Y_ 轴旋转。

可以将 _text_ 实体设为 Billboards ，能让用户能始终清晰看到内容。

实体 `Transform` 组件的 `rotation` 值不会因为跟随用户旋转而改变。

如果一个实体同时具有 `Billboard` 组件和带有 `rotation` 值 `Transform` 组件，玩家将看到该实体按 billboard 旋转。 如果 billboard 设置不影响所有轴，则剩余的轴将根据 `Transform` 组件旋转。

> 注意：如果同时有多个玩家在场，则每个玩家都会看到设有 billboard 的实体面向自己。

## 面向坐标点

您可以在 Transform 组件上使用 `lookAt()`，可以简单地设置面向空间特定点的坐标。这样可以避免角度计算。

```ts
// Create a transform
let myTransform = new Transform()

// Rotate to face the coordinates (4, 1, 2)
myTransform.lookAt(new Vector3(4, 1, 2))
```

在此使用 Vector3 对象，或者具有 x，y 和 z 属性的任何对象。此向量表示要朝向的场景中点的位置坐标。

`lookAt()` 函数还有第二个可选的参数，用于设置 _up_ 的全局方向引用。在大多数情况下，您无需设置此值。

## 缩放

`scale` 也是一个_3D vector_，存储为 `Vector3` 对象，包括 _x_, _y_ 和 _z_ 轴上的缩放比例。无论是基本模型还是 3D 模型，实体的形状都会被相应地放大和缩小。

您可以使用 `set()` 为三个轴分别设置值，或者使用 `setAll()` 设置相同的缩放比例，以在缩放时保持实体的比例。

默认 scale 比例为 1，因此大于 1 的值会放大实体，而小于 1 会缩小实体。

您可以单独对每个维度进行设置，也可以使用 `set` 设置所有维度。

```ts
// Create a transform with a predefined scale
let myTransform = new Transform({scale: new Vector3(2, 2, 2)})

// Set each dimension individually
myTransform.scale.x = 1
myTransform.scale.y = 5
myTransform.scale.z = 1

// Set the whole scale with one expression  (x, y, z)
myTransform.scale.set(1, 5, 1)

// Set the scale with a single number to maintain proportions
myTransform.scale.setAll(2)

// Set the scale with an object
myTransform.scale = new Vector3(1, 1, 1.5)
```

使用对象设置缩放时，可以使用 `Vector3` 对象，也可以使用带有 x，y 和 z 字段的任何其他对象。

## 继承

当实体嵌套在另一个实体中时，子实体从父项继承组件。这意味着如果父实体被定位、缩放或旋转，其子实体也会受到影响。子实体的位置、旋转和比例值不会覆盖父母的位置，旋转和比例值，相反，它们是复合的。

如果缩放父实体，则还会缩放其子项的所有位置值。


```ts
// Create entities
const parentEntity = new Entity()
const childEntity = new Entity()

// Set one as the parent of the other
childEntity.setParent(parentEntity)

// Create a transform for the parent
let parentTransform = new Transform({
  position: new Vector3(3, 1, 1),
  scale: new Vecot3(0.5, 0.5, 0.5)
})

parentEntity.addComponent(parentTransform)

// Create a transform for the child
let childTransform = new Transform({
  position: new Vector3(0, 1, 0)
})

childEntity.addComponent(childTransform)

// Add entities to the engine
engine.addEntity(childEntity)
engine.addEntity(parentEntity)

```

您可以使用没有形状组件的不可见实体，并将其他一些实体设为它的子实体。此不可见实体在场景渲染中不可见，但可用于对其子项进行分组，并将 Transform 应用于所有子实体。
