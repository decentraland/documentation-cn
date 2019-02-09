---
date: 2018-02-2
title: 组件组
description: 如何在场景中跟踪具有相同组件的实体列表，以方便更新
categories:
  - development-guide
type: Document
set: development-guide
set_order: 2
---

组件组能够跟踪所有具有特定[组件]({{ site.baseurl }}{% post_url /development-guide/2018-02-1-entities-components %})的实体列表。

![](/images/media/ecs-big-picture-w-compgroup.png)


在如下情形时，engine 引擎会自动更新列表：

- 向引擎添加新实体
- 从引擎中删除实体
- 向引擎中的实体添加新组件
- 从引擎中的实体删除组件

> 注意：组件组只适用于添加到引擎的实体。 已创建但未添加到引擎的实体或已从引擎中删除的实体不会出现在组件组中。

创建组件组后，您无需手动添加或删除实体，引擎会负责处理。

```ts
const myGroup = engine.getComponentGroup(Transform)
```

[系统]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %})通常在其更新方法中迭代这些组件组中的实体，对每个实体执行相同的操作。 拥有一组预定义的有效实体是节省资源的好方法，特别是对于像 `update()` 这样在每一帧上运行的函数。 如果在每个帧上系统必须在场景中的所有实体中寻找它需要的那个，那将会非常耗时。

您可以通过以下方式访问组件组中的实体：如果组名称为 `myGroup`，则调用 `myGroup.entities` 将返回包含其中所有实体的数组。

> 注意：请记住，组件组会占用用户计算机本地内存。 通常，从组件组中获得的速度好处是一个非常值得的权衡。 但是，如果您需要一个非常大的组却又不需要经常访问，那么最好不要建组。

## 需要的组件

创建组件组时，请指定添加到组中的每个实体需要存在的组件。 您可以根据需要选择任意数量的组件，组件组将只接受具有**所有**列出的组件的实体。

```ts
const myGroup = engine.getComponentGroup(Transform, Physics, NextPosition)
```

> 提示：如果您的场景包含多个具有相同组件的实体，但您只想要组件组中的一些实体，可以创建一个自定义组件作为[标记]({{ site.baseurl }}{% post_url /development-guide/2018-02-1-entities-components %}#components-as-flags)。 该组件不需要包含任何属性。 只需将此组件添加到希望组件组处理的实体中。

## 在系统中使用组件组

```ts
const myGroup = engine.getComponentGroup(Transform, Physics)

export class PhysicsSystem implements ISystem {
  update() {
    for (let entity of myGroup.entities) {
      const position = entity.get(Transform).Position
      const vel = entity.get(Physics).velocity
      position.x += vel.x
      position.y += vel.y
      position.z += vel.z
    }
  }
}
```

在上面的例子中，在游戏循环的每一帧中，`PhysicsSystem` 都会执行 `update()` 函数，这个函数会遍历 `myGroup` 中的实体。

- 如果场景有几个 _ball_ 实体，每个实体都有一个 `Position` 和一个 `Physics` 组件，那么它们将包含在 `myGroup` 中。 然后 `PhysicsSystem` 将在每一帧中更新它们的位置。

- 如果你的场景还有其他实体，如 _hoop_ 和 _scoreBoard_ 只有一个 `Physics` 组件，那么它们不会在 `myGroup` 中，也不会受到 `PhysicsSystem` 的影响。

## 所有实体

您可以通过 `engine.entities` 得到已添加到引擎的所有实体，无论它们具有哪些组件。

```ts
engine.entities
```

## 在遍历时更改组件组

组件组是可变的。 在遍历组件组时，不应该修改组件组，因为这可能会产生意想不到的后果。

例如，如果迭代组件组以从引擎中删除每个实体，则删除实体的操作会重新定位阵列中的其他实体，可能导致某些实体没有被遍历。

要解决此问题，可以使用以下代码从引擎中删除所有实体：

```ts
while (myGroup.entities.length) {
  engine.removeEntity(myGroup.entities[0])
}
```
