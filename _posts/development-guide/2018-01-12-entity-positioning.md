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

You can set the _position_, _rotation_ and _scale_ of an entity by using the `Transform` component. This can be used on any entity, which can also a primitive shape component (cube, sphere, plane, etc) or a 3D model component (glTF, Obj).

可以使用 `Transform` 组件设置实体的 _position_，_rotation_ 和 _scale_。并可在任何实体、或基本形状组件（立方体，球体，平面等）或 3D 模型组件（glTF，Obj）上使用。


<img src="/images/media/ecs-simple-components.png" alt="nested entities" width="400"/>


```ts
// Create a new entity
const ball = new Entity()

// Add a transform component to the entity
ball.add(new Transform())
ball.get(Transform).position.set(5, 1, 5)
ball.get(Transform).scale.set(2, 2, 2)
```

For brevity, you can also create a `Transform` entity and give it initial values in a single statement, passing it an object that can optionally include _position_, _rotation_ and _scale_ properties.

为简洁起见，还可以创建一个 `Transform` 实体，并在一条语句中提供初始值，参数为可选的包含 _position_，_rotation_ 和 _scale_ 属性的对象。

```ts
myEntity.add(new Transform({ 
    position: new Vector3(5, 1, 5), 
    rotation: new Quaternion(0, 0, 0, 0),
    scale: new Vector3(2, 2, 2)
    }))
```

To move, rotate or resize an entity in your scene, change the values on this component incrementally, frame by frame. See [Move entities]({{ site.baseurl }}{% post_url /development-guide/2018-02-12-move-entities %}) for more details and best practices.

要在场景中移动、旋转或调整实体大小，需要逐帧逐步更改此组件上的值。有关详细信息和最佳做法，请参阅[移动实体]({{ site.baseurl }}{% post_url /development-guide/2018-02-12-move-entities %})。

## Position
## 定位

`position` is a _3D vector_, it sets the position of the entity's center on all three axes, stored as a `Vector3` object.

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

> Note: When setting the value of the position with an object, you can either use a `Vector3` object, or any other object with _x_, _y_ and _z_ fields.

> 注意：使用对象来设置位置时，可以使用 `Vector3` 对象，也可以使用带有 _x_，_y_ 和 _z_ 字段的任何其他对象。


When setting a position, keep the following considerations in mind:

设置位置时，请注意以下事项：

- The numbers in a position vector represent _meters_.
- 位置向量中的数字单位为_米_。

  > Note: If you're positioning a child of a parent entity that has a scale that's different from 1, the position vector is scaled accordingly.
  > 注意：如果您正在定位一个缩放值不为 1 的父实体的子项，position 向量也会相应缩放。

- `x:0, y:0, z:0` refers to the _South-East_ corner of the scene's base parcel, at ground level.
- `x:0, y:0, z:0` 位于场景基块的东南角，位于地面。

  > Tip: When viewing a scene preview, a compass appears in the (0,0,0) point of the scene with labels for each axis as reference.
  > 提示：查看场景预览时，场景的（0,0,0）点会有一个坐标，并且每轴都设有标签以供参考。

  > Note: You can change the base parcel of a scene by editing the `base` attribute of _scene.json_.
  > 注意：您可以通过编辑 _scene.json_ 的 `base` 属性来更改场景的基块。


- If an entity is a child of another, then `x:0, y:0, z:0` refers to the center of its parent entity, wherever it is in the scene.

- 如果实体是另一个实体的子节点，那么 `x：0，y：0，z：0` 指的是其父实体的中心，无论其在场景中的哪个位置。

- Every entity in your scene must be positioned within the bounds of the parcels it occupies at all times. If an entity leaves these boundaries, it will raise an error.
- 场景中的每个实体必须始终位于其地块范围内。如果实体超出边界，则会引发错误。

  > Tip: When viewing a scene in preview mode, entities that are out of bounds are highlighted in _red_.
  > 提示：在预览模式下查看场景时，超出边界范围的实体会以_红色_突出显示。

- Your scene is also limited in height. The more parcels that make up the scene, the higher you're allowed to build. See [scene limitations]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %}) for more details.
- 场景的高度也有限制。场景使用的地块越多，所允许的高度越高。详细信息，请参阅[场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})。


## Rotation
## 旋转

`rotation` is stored as a [_quaternion_](https://en.wikipedia.org/wiki/Quaternion), a system of four numbers, _x_, _y_, _z_ and _w_.
`rotation` 存储为[_四元数_](https://en.wikipedia.org/wiki/Quaternion)，由四个数字组成，_x_，_y_，_z_ 和 _w_。

```ts
// Create transform with a predefined rotation in Quaternions
let myTransform = new Transform({rotation: new Quaternion(0, 0, 0, 1)})

// Set rotation with four numbers (x, y, z, w)
myTransform.rotation.set(0, 0, 1, 0)

// Set rotation with a quaternion
myTransform.rotation = new Quaternion(1, 0, 0, 0)
```

You can also set the rotation field with [_Euler_ angles](https://en.wikipedia.org/wiki/Euler_angles), the more common _x_, _y_ and _z_ notation with numbers that go from 0 to 360 that most people are familiar with. To use Euler angles, use one of the following notations:

也可以使用[欧拉角度](https://en.wikipedia.org/wiki/Euler_angles)设置旋转，用一个更为常见的 _x_，_y_ 和 _z_ 来表示，数字使用大多数人都熟悉的从 0 到360。可以使用以下方式使用欧拉角度：


```ts
// Create transform with a predefined rotation in Euler angles
let myTransform = new Transform({rotation: Quaternion.Euler(0, 90, 0)})

// Use the .setEuler() function
myTransform.rotation.setEuler(0, 90, 180)

// Set the `eulerAngles` field
myTransform.rotation.eulerAngles = new Vector3(0, 90, 0)
```

When using a _3D vector_ to represent Euler angles, _x_, _y_ and _z_ represent the rotation in that axis, measured in degrees. A full turn requires 360 degrees.

当使用 _3D vector_ 表示欧拉角时，_x_，_y_ 和 _z_ 表示该轴的旋转，以度为单位。 360 度为一整圈。


> Note: If you set the rotation using _Euler_ angles, the rotation value is still stored internally as a quaternion.

> 注意：如果使用 _Euler_ angles 欧拉角度设置旋转，则内部存储的旋转值仍为四元数。

When you retrieve the rotation of an entity, it returns a quaternion by default. To obtain the rotation expressed as in Euler angles, get the `.eulerAngles` field:

查询实体的旋转度时，默认情况下返回四元数。要获得以欧拉角表示的旋转，使用 `.eulerAngles` 字段：

```ts
myEntity.get(Transform).rotation.eulerAngles
```

## Face the user
## 面向用户旋转

You can set a shape component to act as a _billboard_, this means that it will always rotate the entity to face the user. All components for primitive shapes and glTF models have a `billboard` field to allow you to set this.

您可以将形状组件设置为 _billboard_，这意味着它会始终旋转实体来面对用户。基本形状和 glTF 模型的所有组件都有一个可设置的 `billboard` 字段。

Billboards were a common technique used in 3D games of the 90s, where most entities were 2D planes that always faced the player, but the same can also be used to rotate a 3D model.

Billboards 是 90 年代 3D 游戏中常用的技术，大部分实体是始终面向玩家的 2D 平面，但同样也可用于旋转 3D 模型。

You can also choose to only rotate the shape in this way in one of its axis. For example, if you set the billboard mode of a cube to only rotate in the Y axis, it will follow the user when moving at ground level, but the user will be able to look at it from above or from below.

您也可以选择仅在其中一个轴上以这种方式旋转形状。例如，如果将立方体的 billboard 模式设置为仅在 Y 轴上旋转，则用户在地面移动时它会转向用户，但用户将能够从上方或从下方查看。

Set the `billboard` field with a value from the `BillboardMode` enum. For example, to rotate in all axis, set the value to `BillboardMode.BILLBOARDMODE_ALL`.

使用 `BillboardMode` 枚举中的值设置 `billboard` 字段。例如，要在所有轴上旋转，请将值设置为 `BillboardMode.BILLBOARDMODE_ALL`。

- `BILLBOARDMODE_NONE` (0): 任何轴都不旋转（默认值）
- `BILLBOARDMODE_X` (1): 仅 **X** 轴，其他轴不旋转。
- `BILLBOARDMODE_Y` (2): 仅 **Y** 轴，其他轴不旋转。
- `BILLBOARDMODE_Z` (4): 仅 **Z** 轴，其他轴不旋转。
- `BILLBOARDMODE_ALL` (7): 跟随用户在所有轴上旋转。

```ts
// Create a transform
let box = new BoxShape()

// Set its billboard mode
box.billboard = BillboardMode.BILLBOARDMODE_Y
```

Billboards are also very handy to add to _text_ entities, since it makes them always legible.

Billboards 对 _text_ 实体非常有用，因为它能使它们始终清晰易读。

If an entity has both a Transform component configured with a specific `rotation` and also a shape component with a `billboard` value other than 0, the user will see the entity behaving according to the billboard mode.

如果实体同时配置了 Transform 组件的 `rotation` 及 `billboard` 值不为 0 的形状组件，则该实体将根据 billboard 模式运行。

> Note: If there are multiple users present at the same time, each will see the entities with billboard mode facing themselves.

> 注意：如果同时有多个用户在场，则每个用户都会看到面向自己的 billboard 模式实体。


## Face a set of coordinates
## 面向坐标点

You can use `lookAt()` on the Transform component to orient an entity fo face a specific point in space by simply passing it that point's coordinates. This is a way to avoid dealing with the math for calculating the necessary angles.

您可以在 Transform 组件上使用 `lookAt()`，通过简单地该点的坐标来设置面向空间中特定点的实体。这是一种避免数学角度计算的方法。


```ts
// Create a transform
let myTransform = new Transform()

// Rotate to face the coordinates (4, 1, 2)
myTransform.lookAt(new Vector3(4, 1, 2))
```

This field requires a _Vector3_ object as a value, or any object with _x_, _y_ and _z_ attributes. This vector indicates the coordinates of the position of the point in the scene to look at.

在此使用 _Vector3_ 对象，或者具有 _x_，_y_ 和 _z_ 属性的任何对象。此向量表示要朝向的场景中点的位置坐标。


The `lookAt()` function has a second optional argument that sets the global direction for _up_ to use as reference. For most cases, you won't need to set this field.

`lookAt()` 函数还有一个可选参数，设置要用作参考的全局方向。在大多数情况下，您无需设置此值。


## Scale
## 缩放

`scale` is also a _3D vector_, stored as a `Vector3` object, including the scale factor on the _x_, _y_ and _z_ axis. The shape of the entity scaled accordingly, whether it's a primitive or a 3D model.

`scale` 也是一个_3D vector_，存储为 `Vector3` 对象，包括 _x_，_y_ 和 _z_ 轴上的缩放比例。无论是基本模型还是 3D 模型，实体的形状都会被相应地放大和缩小。

You can either use the `set()` operation to provide a value for each of the three axis, or use `setAll()` to provide a single number and maintain the entity's proportions as you scale it.

您可以使用 `set()` 为三个轴分别设置值，或者使用 `setAll()` 设置同一数值，以在缩放时保持实体的比例。


The default scale is 1, so assign a value larger to 1 to stretch an entity or smaller than 1 to shrink it.

默认 scale 比例为 1，因此大于 1 的值会放大实体，而小于 1 会缩小实体。

You can either set each dimension individually, or use the `set` operation to set all dimensions.

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

When setting the value of the scale with an object, you can either use a `Vector3` object, or any other object with _x_, _y_ and _z_ fields.

使用对象设置缩放时，可以使用 `Vector3` 对象，也可以使用带有 _x_，_y_ 和 _z_ 字段的任何其他对象。


## Inherit transformations from parent
## 继承


When an entity is nested inside another, the child entities inherit components from the parents. This means that if a parent entity is positioned, scaled or rotated, its children are also affected. The position, rotation and scale values of children entities don't override those of the parents, instead these are compounded.

当实体嵌套在另一个实体中时，子实体从父项继承组件。这意味着如果父实体被定位、缩放或旋转，其子实体也会受到影响。子实体的位置、旋转和比例值不会覆盖父母的位置，旋转和比例值，相反，它们是复合的。

If a parent entity is scaled, all position values of its children are also scaled.

如果缩放父实体，则还会缩放其子项的所有位置值。

```tsx
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

parentEntity.add(parentTransform)

// Create a transform for the child
let childTransform = new Transform({
  position: new Vector3(0, 1, 0)
})

childEntity.add(childTransform)

// Add entities to the engine
engine.addEntity(childEntity)
engine.addEntity(parentEntity)

```

You can use an invisible entity with no shape component to wrap a set of other entities. This entity won't be visible in the rendered scene, but can be used to group its children and apply a transform to all of them.

您可以使用没有形状组件的不可见实体，并将其他一些实体设为它的子实体。此不可见实体在场景渲染中不可见，但可用于对其子项进行分组，并将 Transform 应用于所有子实体。
