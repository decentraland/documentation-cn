---
date: 2018-01-15
title: 实体和组件
description: Decentraland 场景中实体和组件的基本知识
categories:
  - development-guide
redirect_from:
  - /sdk-reference/scene-state/
  - /sdk-reference/scene-content-guide/
  - /docs/entities
  - /sdk-reference/entity-interfaces/
  - /development-guide/scene-state/
  - /development-guide/scene-content/
  - /development-guide/entity-interfaces/
type: Document
set: development-guide
set_order: 1
---

Decentraland 场景是基于[_entities（实体）_, _components（组件）_ 和 _systems（系统）_](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system)构建的。 这是很多游戏引擎架构常用的一种模式，可以轻松地实现可组合性和可扩展性。

![](/images/media/ecs-big-picture.png)

## 概述

_Entities（实体）_ 是 Decentraland 场景中所有内容的基本单位。 在场景中所有可见和不可见的 3D 对象和音频播放器都是实体。 实体不过是一个容纳组件的容器。 其本身没有自己的属性或方法，而只是将几个组件组合在一起。

_Components（组件）_ 定义实体的特征。 例如，`transform` 组件确定了实体的坐标、旋转和比例。 `BoxShape` 组件在场景中渲染时使得实体显示为一个立方体形状，`Material` 组件为实体提供颜色或纹理。 您还可以创建一个自定义的 `health` 组件来存储实体的健康值，并可将其添加到游戏中一个非玩家敌人的实体中。

如果您熟悉 Web 开发，可以把实体看作 _DOM_ 树中的 _Elements_ ，并将组件作为这些元素的 _attributes_。

> 注意：在以前版本的SDK中，_scene state（场景状态）_ 存储在与实体本身分开的对象中。 从版本 5.0 开始，_scene state_ 直接由场景中实体使用的组件来代替。

<img src="/images/media/ecs-components.png" alt="Armature" width="400"/>

诸如 `Transform`，`Material` 或者 _shape_ 类组件与场景的渲染紧密相关。 如果这些组件中的值发生变化，那么在下一帧中场景就会改变渲染。

组件用于存储有关其父实体的数据。 它们只存储这些数据，不应自行修改。 组件中对值的所有更改都要由[Systems]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %})来进行。 Systems 完全与组件和实体本身分离。 实体和组件与 _systems_ 对其的操作无关。

有关预定义组件的所有可用构造函数的参考，请参见[组件参考](https://github.com/decentraland/ecs-reference)。

## 实体和组件的语法

实体和组件声明为 TypeScript 对象。 如下是一些实体创建、配置和加入 engine 的基本操作。

```ts
// Create an entity
const cube = new Entity()

// Create and add a `Transform` component to that entity
cube.addComponent(new Transform())

// Set the fields in the component
cube.getComponent(Transform).position.set(3, 1, 3)

// Create and apply a `CubeShape` component to give the entity a visible form
cube.addComponent(new CubeShape())

// Add the entity to the engine
engine.addEntity(cube)
```

## 将实体添加到 engine（引擎）

创建新实体，即实例化对象并将其存储在内存中。 在将其添加到 _engine（引擎）_ 之前，新创建的实体不会被渲染显示，并且用户也无法与其进行交互，。

engine 是场景的一部分，处于中间位置并且管理着所有其他事物。 它确定实体的渲染以及用户与它们的交互方式。 并决定执行 [systems]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %}) 中的哪些函数以及何时执行。

```ts
// Create an entity
const cube = new Entity()

// Give the entity a shape
cube.addComponent(new CubeShape())

// Add the entity to the engine
engine.addEntity(cube)
```

在上面的示例中，在将其添加到引擎前，场景用户无法看到新创建的实体。

> 注意：在实体被添加到引擎前，实体也不会被添加到[Component groups（组件组）]({{ site.baseurl }}{% post_url /development-guide/2018-02-2-component-groups %})中。

有时候可以预先创建实体并且在使用之前不添加到引擎中。尤其对于具有精细几何形状但可能需要很长时间才能加载的实体特别有用。

当实体添加到引擎时，它的 `alive` 属性被隐式设置为 _true_。 您可以用这个属性来确定实体是否已添加到引擎。

```ts
if (myEntity.alive) {
  log("the entity is added to the engine")
}
```

> 注意：始终建议在将实体添加到引擎之前，先添加 `Transform`  组件到实体中。 没有 Transform 组件的实体会在场景的 _（0，0，0）_ 位置进行渲染，因此，如果实体没有 `Transform` 组件，则该实体会暂时在该位置进行渲染， 并保留其原始大小和旋转度。

## 从引擎中删除实体

已添加到引擎的实体也可以从中删除。 删除实体后，场景将不会再渲染，用户也无法再与其进行交互。

```ts
// Remove an entity from the engine
engine.removeEntity(cube)
```

注意：删除实体后，也会从所在的[组件组]({{ site.baseurl }}{% post_url /development-guide/2018-02-2-component-groups %})中删除。

如果场景中有一个引用到已删除实体的指针，它会保留在内存中，您仍然可以访问并更改其组件的值，并在需要时重新添加到 engine。

如果删除的实体还带有子实体，所有的子实体也将被删除。

## 嵌套实体

实体可以将其他实体作为子项。多亏了这一点，我们可以将实体安排到树中，就像网页的 HTML 一样。

<img src="/images/media/ecs-nested-entities.png" alt="nested entities" width="400"/>


要将实体设置为另一个实体的父节点，可以使用 `.setParent()`：

```ts
// Create entities
childEntity = new Entity()
parentEntity = new Entity()

// Set parent
childEntity.setParent(parentEntity)
```

一旦指定了父实体，就可以使用 `.getParent()` 从子实体中得到它。

```ts
// Get parent from an entity
const parent = childEntity.getParent()
```
<!--
You can also iterate over an entity's children in the following way.

```ts
// Get the first child listed in the library
for(let id in parent.children){
  const child = parent.children[id]
  // do something with the child entity
}
```

> Note: `.children` returns a library that lists all the child entities.
-->

如果父实体具有影响其位置、缩放或旋转的 `transform` 组件，则其子实体也会受到影响。

- **position**，位置，父实体的中心是 _0,0,0_
- 对于**rotation**，旋转，父实体是四元数 _0,0,0,1_（相当于欧拉角 _0,0,0_）
- 对于**scale**，缩放，父实体的大小为 _1_。 任何对父实体的调整都会按比例影响子实体的位置和缩放。

没有形状（shape）组件的实体在场景中是不可见的。 但是可以包括子实体组，可以用来处理和定位多个实体。

删除父实体，可以将实体的 parent 设为 `null`.

  ```ts
 childEntity.setParent(null)
 ```

如果你为一个已经有父实体的实体设置父实体，会覆盖原设置。

## 根据 ID 取得实体

场景中的每个实体都具有唯一的自动生成的 _id_。 您可以通过引用 `engine.entities[]` 数组，根据此 ID 从引擎中取得特定实体。

```typescript
engine.entities[entityId]
```

例如，如果玩家的点击或射线命中实体，则会返回命中实体的 ID，您可以使用上面的命令获取与该 ID 匹配的实体。

## 将组件添加到实体

将组件添加到实体时，组件的值会影响实体。

执行此操作的一种方法是首先创建组件实例，然后将其添加到实体：

```ts
// Create entity
cube = new Entity()

// Create component
const myMaterial = new Material()

// Configure component
myMaterial.albedoColor = Color3.Red()

// Add component
cube.addComponent(myMaterial)
```

也可以使用一行代码来创建组件的新实例并将其添加到实体：

```ts
// Create entity
cube = new Entity()

// Create and add component
cube.addComponent(new Material())

// Configure component
cube.getComponent(Material).albedoColor = Color3.Red()
```

> 注意：在上面的示例中，由于未定义指向实体材质组件的指针，因此需要使用父实体的 `.getComponent()` 函数取得。

#### 添加或更换组件

使用 `.addComponentOrReplace()` 而不是 `.addComponent()`，您可以覆盖特定实体上相同类型的任何现有组件。

## 从实体中删除组件

从实体中删除组件，可以使用实体的 `removeComponent()` 方法。

```ts
myEntity.removeComponent(Material)
```

如果试图删除实体中不存在的组件，此操作不会报错。

删除的组件即使在删除后仍可能仍保留在内存中。 如果您的场景定期添加新组件然后删除，这些删除的组件将会累积并导致内存问题。 建议尽可能使用[对象池](#pooling-entities-and-components)来处理这些组件。

## 从实体访问组件

您可以使用实体的 `.getComponent()` 函数通过其父实体访问组件。

```ts
// Create entity and component
cube = new Entity()

// Create and add component
cube.addComponent(new Transform())

// Using get
let transform = cube.getComponent(Transform)

// Edit values in the component
transform.position = new Vector3(5, 0, 5)

`getComponent()` 函数取得组件对象的引用。 如果更改此函数返回的值，则表示您正在更改组件本身。 例如，在上面的例子中，我们将组件中存储的 `position` 设置为_(5, 0, 5)_。

```ts
cube.getComponent(Transform).scale.x = Math.random() * 10
```

上面的示例直接修改 Transform 组件上缩放值 _x_ 。

如果你不能确定实体是否有需要的组件，请使用 `getComponentOrNull()` 或 `getComponentOrCreate()`

```ts
//  getComponentOrNull
scale = cube.getComponentOrNull(Transform)

// getComponentOrCreate
scale = cube.getComponentOrCreate(Transform)
```

如果您要检索的组件在实体中不存在：

 - `getComponent()` 会返回错误。
 - `getComponentOrNull()` 返回 `Null`。
 - `getComponentOrCreate()` 会实例化一个新组件并返回。

当您处理[可互换组件](#可互换的组件)时，您还可以按 _space name_ 而不是按类型获取组件。 例如，`BoxShape` 和 `SphereShape` 都在实体的`shape` 命名空间中。 如果想知道实体有哪个组件，您可以获取实体的 `shape`，它将返回 `shape` 命名空间的组件。

```ts
let entityShape = myEntity.getComponent(shape)
```

## 自定义组件

如果您需要存储实体的信息，但是这些信息未在 SDK 的默认组件中（请参阅[组件参考](https://github.com/decentraland/ecs-reference)），那么您可以创建自定义组件。

提示：可以在场景的 `.ts` 文件中定义自定义组件，但对于较大的项目，我们建议在单独的 `ts` 文件中定义它们然后导入。

组件可以根据需要存储任意数据。

```ts
@Component('wheelSpin')
export class WheelSpin {
  spinning: boolean
  speed: number
}
```

请注意，这里我们为组件定义了两个名称：`wheelSpin` 和 `WheelSpin`。 _class name 类名称_（大写的名称）是用于将组件添加到实体的名称。 _space name 命名空间_（以小写字母开头的那个）通常可以忽略，除非你想将它用作[可互换组件](#可互换的组件)。

定义后，您可以在场景的实体中使用该组件：

```ts
// Create entities
wheel = new Entity()
wheel2 = new Entity()

// Create instances of the component
wheel.addComponent(new WheelSpin())
wheel2.addComponent(new WheelSpin())

// Set values on component
wheel.getComponent(WheelSpin).spinning = true
wheel.getComponent(WheelSpin).speed = 10
wheel2.getComponent(WheelSpin).spinning = false
```

添加组件的每个实体，都会实例化一个组件的新副本，用于保存该实体的特定数据。

#### 构造函数

组件的构造函数可以在创建实例时初始化组件值。

```ts
@Component('wheelSpin')
export class WheelSpin {
  spinning: boolean
  speed: number
  constructor(spinning: boolean, speed: number) {
    this.spinning = spinning
    this.speed = speed
  }
}
```

如果组件包含构造函数，则可以使用以下语法：

```ts
// Create entity
wheel = new Entity()

// Create instance of component and set its values
wheel.addComponent(new WheelSpin(true, 10))
```

> 提示：如果使用代码编辑器，在实例化具有构造函数的组件时，可以通过将鼠标置于表达式上来查看参数。

<!-- img -->

参数可以设置默认值，使得参数可选。 如果存在默认值，并且在实例化组件时未声明参数，则它将使用默认值。

```ts
@Component('wheelSpin')
export class WheelSpin {
  spinning: boolean
  speed: number
  constructor(spinning?: boolean = false, speed?: number = 3) {
    this.spinning = spinning
    this.speed = speed
  }
}
```
```ts
// Create entity
wheel = new Entity()

// Create instance of component with default values
wheel.addComponent(new WheelSpin())
```

#### 从其他组件继承

您可以创建基于现有组件的组件，并利用其现有的所有方法和字段。

以下示例定义 _Velocity_ 组件，该组件从现有的 _Vector3_ 组件继承其字段和方法。

```ts
@Component("velocity")
export class Velocity extends Vector3 {
  // x, y and z fields are inherited from Vector
  constructor(x: number, y: number, z: number) {
    super(x, y, z)
  }
}
```

#### 可互换的组件

某些组件不能在单个实体中共同存在。 例如，实体不能同时有 `BoxShape` 和 `PlaneShape` 组件。 如果使用 `.addComponentOrReplace()` 指定了一个，则另一个（如果存在）会被覆盖。

可以创建这一类自定义组件，每个实体只能分配其中一个组件。

要定义可互换且在实体中占用相同 _空间_ 的组件，可以设置相同的内部名称：

```ts
@Component("animal")
export class Dog {
 (...)
}

@Component("animal")
export class Cat {
 (...)
}
```

在上面的示例中，请注意两个组件都使用 _animal_ 空间。 场景中的每个实体只能分配一个 _animal_ 组件。

如果使用 `.addComponentOrReplace()` 将 _Dog_ 组件分配给具有 _Cat_ 组件的实体，则 _Dog_ 组件将覆盖 _Cat_ 组件。

## 将组件作为标记使用

您可以添加一个组件，该组件仅标记实体以将其与其他实体区分开来，而不使用它来存储任何数据。

这在使用[组件组]({{ site.baseurl }}{% post_url /development-guide/2018-02-2-component-groups %})时尤其有用。 由于组件组基于它们拥有的组件列出实体，因此简单的标记组件可以将实体区分开来。

```ts
@Component("myFlag")
export class MyFlag {}
```

## 实体和组件池

如果您在场景中生成和删除实体，那么在内存中保留一组固定的实体通常是一种很好的做法。 您可以在引擎中添加和删除现有实体，而不是创建新实体并删除它们。 这是处理用户内存的有效方法。

未添加到引擎的实体不会在场景中呈现出来，但它们会保留在内存中，以便在需要时快速加载。 因为它们不会被渲染，所以也不会累计到场景的最多三角形数限制中。

```ts
// Define spawner singleton object
const spawner = {
  MAX_POOL_SIZE: 20,
  pool: [] as Entity[],

  spawnEntity() {
    // Get an entity from the pool
    const ent = spawner.getEntityFromPool()

    if (!ent) return

    // Add a transform component to the entity
    let t = ent.getComponentOrCreate(Transform)
    t.scale.setAll(0.5)
    t.position.set(5, 0, 5)

    //add entity to engine
    engine.addEntity(ent)
  },

  getEntityFromPool(): Entity | null {
    // Check if an existing entity can be used
    for (let i = 0; i < spawner.pool.length; i++) {
      if (!spawner.pool[i].alive) {
        return spawner.pool[i]
      }
    }
    // If none of the existing are available, create a new one, unless the maximum pool size is reached
    if (spawner.pool.length < spawner.MAX_POOL_SIZE) {
      const instance = new Entity()
      spawner.pool.push(instance)
      return instance
    }
    return null
  }
}

spawner.spawnEntity()
```

将实体添加到引擎时，`alive` 会隐式设置为 `true`，删除时，此字段设置为 `false`。

使用对象池具有以下好处：

 - 如果您的实体使用复杂的 3D 模型或纹理，从内容服务器加载可能会需要一段时间。 如果实体在需要时已经在内存中，则不会发生延迟。
 - 这是一个避免常见问题的解决方案，被删除的实体在被删除后仍然可以在内存中保留，并且这些未使用的实体可能会在内存中累加，直到它们变得太多而无法处理。 通过回收相同的实体，您可以确保不会发生这种情况。

<!--
Similarly, if you plan to set and remove certain components from your entities, it's recommendable to create a pool of fixed components to add and remove rather than create new component instances each time.

```ts
```
-->