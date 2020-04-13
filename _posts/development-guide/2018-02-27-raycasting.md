---
date: 2018-02-27
title: 光线投射
description: 使用光线投射技术跟踪和查询场景中的实体。
categories:
  - development-guide
type: Document
set: development-guide
set_order: 27
---

光线投射是游戏开发的基本工具。通过空间中的这条假想线，可以查询有哪些实体跟这条线相交。这对视线、子弹轨迹的计算，寻路算法和许多其他应用都非常有用。

当玩家点击或按压主按钮或第二按钮时，射线在玩家位置沿着视线方向发出，更多细节，请查看[按钮事件]({{ site.baseurl }}{% post_url /development-guide/2018-02-14-click-events %})。本文档介绍了不依赖于玩家的操作，如何在任意位置和方向使用不可见的光线投射，并可以在许多其他场景中使用。

## PhysicsCast

`PhysicsCast` 对象是主要的光线投射接口的静态类。可以在场景中使用 `PhysicsCast.instance` 引用。它有几个特定的有关光线投射方法。

```typescript
let physicsCast = PhysicsCast.instance
```

## 创建射线

Ray 对象描述了将被用于查询实体的无形射线。由以下三个信息定义：

- `origin`: _Vector3_ 场景空间坐标，用于指定射线开始位置。
- `direction`: _Vector3_ 射线方向（射线从 _0,0,0_ 开始）。
- `distance`: _number_ 设置射线跟踪长度。


```typescript
let originPos = new Vector3(2, 1, 4)
let direction = new Vector3(0, 1, 1)

let ray: Ray = {
      origin: originPos,
      direction: direction,
      distance: 10
    }
```

还有一种生成射线的方法是提供空间中两个点的坐标。而射线的方向和距离这些信息是隐含的。要做到这一点，可以使用 `PhysicsCast` 对象中的 `getRayFromPositions()` 方法。

```typescript
let physicsCast = PhysicsCast.instance

let originPos = new Vector3(2, 1, 1)
let targetPos = new Vector3(2, 3, 3)

let rayFromPoints = physicsCast.getRayFromPositions(originPos, targetPos)
```

您也可以使用 `PhysicsCast` 对象的 `getRayFromCamera()` 的方法，使用玩家的当前位置和旋转来生成射线，只需要提供射线距离就可。

```typescript
let physicsCast = PhysicsCast.instance

let rayFromCamera = physicsCast.getRayFromCamera(1000)
```

## 光线投射查询

光线投射查询由 `PhysicsCast` 类完成。有两种方法：

- `hitFirst()`: 此方法只返回从原点出发第一个碰到的实体。
- `hitAll()`: 该方法返回从原点到射线长度间所有命中的实体。

这两种方法都需要传递射线和一个回调函数。请注意，即使没有实体被射线击中也会执行回调函数。

![](/images/media/raycast.png)

此示例仅查询射线碰到的第一个实体：

```typescript
let physicsCast = PhysicsCast.instance

let originPos = new Vector3(2, 1, 4)
let direction = new Vector3(0, 1, 1)

let ray: Ray = {
      origin: originPos,
      direction: direction,
      distance: 10
	}

physicsCast.hitFirst(ray, (e) => {
	log(e.entity.entityId)
})
```

返回射线遇到的所有实体示例：

```typescript
let physicsCast = PhysicsCast.instance

let originPos = new Vector3(2, 1, 4)
let direction = new Vector3(0, 1, 1)

let ray: Ray = {
      origin: originPos,
      direction: direction,
      distance: 10
	}

physicsCast.hitAll(ray, (e) => {
	for (let entityHit of e.entities) {
         log(entityHit.entity.entityId)
    }
})
```

## 光线投射查询结果

运行光线投射查询后，回调函数将能够使用事件对象的以下信息：

- `didHit`: _boolean_ 如果至少一个实体被击中则为 _true_，如果没有碰到任何实体，则为 _false_。
- `ray`: _Ray_ 查询中使用的 _Ray_
- `hitPoint`: _Vector3_ 场景的空间的命中位置。如果有多个实体被命中，返回第一个。
- `hitNormal`: _Vector3_ for the normal of the hit in world space. If multiple entities did hit, it returns the normal of the first point of ray collision.

- `entity`: _Object_ 有关被命中的实体信息。就是调用 `hitFirst()` 的返回值，仅有实体命中才会返回值。
- `entities` : `entity` 对象数组，每个命中的实体。即 `hitAll()` 的返回值，仅有实体命中才会返回值。

`entity` 对象及 `entities` 数组中的对象包含以下数据：

- `entityId`: _String_ 命中的实体 id
- `meshName`: _String_ 命中的3D 模型具体网格的内部名称。这对 3D 模型由多个网格组成时，非常有用。

下面的例子演示了如何从回调函数的事件对象中访问这些属性：

```typescript
let physicsCast = PhysicsCast.instance

let originPos = new Vector3(2, 1, 4)
let direction = new Vector3(0, 1, 1)

let ray: Ray = {
      origin: originPos,
      direction: direction,
      distance: 10
	}

physicsCast.hitFirst(ray, (e) => {
	if (e.didHit) {
		engine.entities[e.entity.entityId].addComponentOrReplace(hitMaterial)
	}
})
```

> 提示：要引用基于 ID 的实体，可使用引擎的 `entities` 数组，如：`engine.entities[e.entity.entityId]`。

下面的例子跟前面的一样，只是使用了 `hitAll()` 来取得实体数组： 

```typescript
let physicsCast = PhysicsCast.instance

let originPos = new Vector3(2, 1, 4)
let direction = new Vector3(0, 1, 1)

let ray: Ray = {
      origin: originPos,
      direction: direction,
      distance: 10
	}

physicsCast.hitAll(ray, (e) => {
	if (e.didHit) {
		for (let entityHit of e.entities) {
			engine.entities[entityHit.entity.entityId].addComponentOrReplace(hitMaterial)
		}	
	}
})
```


<!--

## Collide with the player

You can't directly hit the player with a ray, but what you can do as a workaround is position an entity occupying the same space as the player

would the entity's colliders bother?
-->

<!--

## Hit avatars


PhysicsCast.hitFirstAvatar( query:RaycastQuery, 
			    hitCallback:(e:RaycastHitAvatar) => {} )

PhysicsCast.hitAllAvatars( query:RaycastQuery, 
			  hitCallback:(e:RaycastHitAvatars) => {} )		


## Cast a sphere


-->