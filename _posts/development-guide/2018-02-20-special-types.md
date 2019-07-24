---
date: 2018-02-14
title: 特殊类型
description: 了解存在哪些特殊类型，包括向量、四元数等。
categories:
  - development-guide
type: Document
set: development-guide
set_order: 20
---

## 向量

Decentraland 使用向量来表示路径、空间中的点和方向。向量也可以用来定义旋转方向，作为 _quaternions_ 的更友好的替代。`Vector2`, `Vector3` 和 `Vector4` 分别定义为不同的类，包含不同数量的维度。

向量对象包含一系列方便的方法，您可以调用这些方法来避免处理大多数向量数学计算。

下面几行代码展示了一些基本的向量操作语法。

```ts
// Instance a vector object
let myVector = new Vector3(3, 1, 5)

// Edit one of its values
myVector.x = 5

// Call functions from the vector instance
let normalizedVector = myVector.normalize()

// Call functions from the vector class
let distance = Vector3.Distance(myVector1, myVector2)

let midPoint = Vector3.lerp(myVector1, myVector2, 0.5)
```

三维矢量也包含在几个组件中。例如，`Transform`组件包含实体的 _position_ 和 _scale_ 的 `Vector3` 值。

#### 方向向量的快捷方式

方向向量可以使用以下快捷方式:

- `Vector3.Zero()` returns _(0, 0, 0)_
- `Vector3.Up()` returns _(0, 1, 0)_
- `Vector3.Down()` returns _(0, -1, 0)_
- `Vector3.Left()` returns _(-1, 0, 0)_
- `Vector3.Right()` returns _(1, 0, 0)_
- `Vector3.Forward()` returns _(0, 0, 1)_
- `Vector3.Backward()` returns _(0, 0, -1)_

## 标量

标量只不过是一个数。因此，实例化一个 `Scalar` 对象来存储数据没有多大意义。但是，`Scalar` 类中公开了几个方便的函数(类似于_Vector_ 类中的函数)，这些函数可用于数字。

```ts

// Call functions from the Scalar class
let random = Scalar.RandomRange(1, 100)

let midPoint = Scalar.lerp(number1, number2, 0.5)

let clampedValue = Scalar.Clamp(myInput, 0, 100)
```

## 四元数

四元数用于存储转换组件的旋转信息。四元数由 -1 到 1 之间的四个数字组成: x, y, z, w。

```ts
// Instance a quaternion object
let myQuaternion = new Quaternion(0, 0, 0, 1)

// Edit one of its values
myQuaternion.x = 1

// Call functions from the quaternion instance
let quaternionAsArray = myQuaternion.asArray()

// Call functions from the quaternion class
let quaternionFromEuler = Quaternion.FromEulerAnglesRef(90, 0, 0)

let midPoint = Quaternion.Slerp(myQuaternion1, myQuaternion2, 0.5)
```





