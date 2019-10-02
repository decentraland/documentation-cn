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

Raycasting is a fundamental tool in game development. With raycasting, you can trace an imaginary line in space, and query if any entities are intersected by the line. This is useful for calculating lines of sight, trajectories of bullets, pathfinding algorithms and many other applications.

光线投射是游戏开发的基本工具。通过空间中的这条假想线，可以查询有哪些实体跟这条线相交。这对视线、子弹轨迹的计算，寻路算法和许多其他应用都非常有用。

When a player clicks or pushes the primary or secondary button, a ray is traced from the player's position in the direction they are looking, see [button events]({{ site.baseurl }}{% post_url /development-guide/2018-02-14-click-events %}) for more details about this. This document covers how to trace an invisible ray from any arbitrary position and direction, independent of player actions, which you can use in many other scenarios.

当玩家点击或按压主按钮或第二按钮时，射线在玩家位置沿着视线方向发出，更多细节，请查看[按钮事件]({{ site.baseurl }}{% post_url /development-guide/2018-02-14-click-events %})。本文档介绍了不依赖于玩家的操作，如何在任意位置和方向使用不可见的光线投射，并可以在许多其他场景中使用。

## PhysicsCast

The `PhysicsCast` object is a static class that serves as the main raycasting interface. You can refer to it in your scene as `PhysicsCast.instance`. You'll see it has several methods that are all specific to raycasting.

`PhysicsCast` 对象是主要的光线投射接口的静态类。可以在场景中使用 `PhysicsCast.instance` 引用。它有几个特定的有关光线投射方法。



```typescript
let physicsCast = PhysicsCast.instance
```

## Create a ray 创建射线

A ray object describes the invisible ray that will be used to query for entities. Rays are defined using three bits of information:

Ray 对象描述了将被用于查询实体的无形射线。由以下三个信息定义：


- `origin`: _Vector3_ with the coordinates in scene space to start the ray from. 场景空间坐标，射线开始位置。
- `direction`: _Vector3_ describing the direction (as if the ray started from _0,0,0_).射线方向（就好像从射线开始_0,0,0_）。
- `distance`: _number_ to set the length with which this ray will be traced. 设置射线跟踪长度。


```typescript
let originPos = new Vector3(2, 1, 4)
let direction = new Vector3(0, 1, 1)

let ray: Ray = {
      origin: originPos,
      direction: direction,
      distance: 10
    }
```

As an alternative, you can also generate a ray from providing two points in space. Both the direction and distance will be implicit in this information. To do this, use the `getRayFromPositions()` method of the `PhysicsCast` object.

还有一种生成射线的方法是提供空间中两个点的坐标。而射线的方向和距离这些信息是隐含的。要做到这一点，可以使用 `PhysicsCast` 对象中的 `getRayFromPositions()` 方法。

```typescript
let physicsCast = PhysicsCast.instance

let originPos = new Vector3(2, 1, 1)
let targetPos = new Vector3(2, 3, 3)

let rayFromPoints = physicsCast.getRayFromPositions(originPos, targetPos)
```

You can also get generate a ray from the player's current position and rotation. The only piece of information you need to pass in this case is the distance. To do this, use the `getRayFromCamera()` method of the `PhysicsCast` object.

您也可以使用 `PhysicsCast` 对象的 `getRayFromCamera()` 的方法，使用玩家的当前位置和旋转来生成射线，只需要提供射线距离就可。

```typescript
let physicsCast = PhysicsCast.instance

let rayFromCamera = physicsCast.getRayFromCamera(1000)
```

## Run a raycast query 光线投射查询

Raycast queries are run by the `PhysicsCast` class. there are two methods available for doing this:

光线投射查询由 `PhysicsCast` 类完成。有两种方法：


- `hitFirst()`: this method only returns the first hit entity, starting from the origin point.：此方法只返回从原点出发第一个碰到的实体。
- `hitAll()`: this method returns all hit entities, from the origin through to the length of the ray. ：该方法返回从原点到射线长度间所有命中的实体。

Both these methods need to be passed a ray and a callback function to execute. Note that the callback function is always executed, even if no entities were hit by the ray.

这两种方法都需要传递射线和一个回调函数。请注意，即使没有实体被射线击中也会执行回调函数。

![](/images/media/raycast.png)

This sample queries only for the first entity hit by the ray: 此示例仅查询射线碰到的第一个实体：

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

This sample queries for all entities hit by the ray: 返回射线遇到的所有实体示例：

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

## Results from raycast query 光线投射查询结果

After running a raycast query, the callback function will be able to use the following information from the event object:

运行光线投射查询后，回调函数将能够使用事件对象的以下信息：

- `didHit`: _boolean_ that is _true_ if at least one entity was hit, _false_ if there were none.：也就是说，如果至少一个实体被击中则为 _true_，如果没有碰到任何实体，则为 _false_。
- `ray`: _Ray_ that has been used in the query：查询中使用的 _Ray_
- `hitPoint`: _Vector3_ for the point in scene space where the hit occurred. If multiple entities were hit, it returns the first point of ray collision.：场景的空间的命中位置。如果有多个实体被命中，它返回第一个。
- `hitNormal`: _Vector3_ for the normal of the hit in world space. If multiple entities did hit, it returns the normal of the first point of ray collision.：_Vector3_为正常在世界空间中命中。如果多个实体没有命中，则返回正常射线碰撞的第一点。

- `entity`: _Object_ with info about the entity that was hit. This is returned when using `hitFirst()`, and it's only returned if there were any entities hit. 有关被命中的实体信息。就是调用 `hitFirst()` 的返回值，仅有实体命中才会返回值。
- `entities` : _Array of `entity` objects_, each with info about the entities that were hit. This is returned when using `hitAll()`, and it's only returned if there were any entities hit. `entity` 对象数组，每个命中的实体。即 `hitAll()` 的返回值，仅有实体命中才会返回值。

The `entity` object, and the objects in the `entities` array contain the following data:
`entity` 对象及 `entities` 数组中的对象包含以下数据：

- `entityId`: _String_ with the id for the hit entity_String_ 命中的实体 id
- `meshName`: _String_ with the internal name of the specific mesh in the 3D model that was hit. This is useful when a 3D model is composed of multiple meshes._String_ 命中的3D 模型具体网格的内部名称。这对 3D 模型由多个网格组成时，非常有用。

The example below shows how you can access these properties from the event object in the callback function:
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

> Tip: To reference an entity based on its ID, use engine's `entities` array, like this: `engine.entities[e.entity.entityId]`. 

> 提示：要引用基于 ID 的实体，可使用引擎的 `entities` 数组，如：`engine.entities[e.entity.entityId]`。


The example below does the same, but dealing with an array of entities returned from the `hitAll()` function:
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