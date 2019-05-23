---
date: 2018-02-16
title: 自定义事件
description: 发出自定义事件并为其添加侦听器
categories:
  - development-guide
type: Document
set: development-guide
set_order: 16
---

有时需要分离场景代码的不同部分，然后使用事件让它们相互交互。

Decentraland 场景处理一些如 `click`，`buttonDown` 或 `buttonUp` 等[默认事件](({{ site.baseurl }}{% post_url /development-guide/2018-02-14-click-events %}))，也可以创建自己的事件来处理特定于你场景的内容。

例如，您可以在每次用户在场景中拾取硬币时发出 `pickedCoin` 事件。 然后，计分板就可以监听这些事件并更新相应的分数。 这样，处理拾取硬币的部分代码不需要对更新计分板的代码有任何引用。

## 初始化事件管理器

在发出或侦听事件之前，您需要在场景中启动事件管理器。

```ts
const events = new EventManager()
```

## 定义事件类型

如果希望场景中的事件包含自定义数据字段，则需要为事件定义特定类型。 您可以通过定义一个具有 `@EventConstructor()` 装饰器的类来完成此操作。

```ts
@EventConstructor()
class MyEvent {
  constructor(public field1: string, public field2: number) {}
}
```

## 发出事件

要发出事件，可以调用事件管理器的 `fireEvent()` 函数。

```ts
events.fireEvent(new MyEvent(field1, field2))
```

请注意，在此示例中，正在发送的事件包含自定义事件类型的对象。

## 监听事件

要监听事件，可以添加调用事件管理器的 `addListener()` 函数。 此函数采用以下参数：

- 要监听的事件对象的 `type`。
- 使用的 listener 。 这几乎总是 `null`。
- 每次捕获事件时执行的功能。

```ts
events.addListener(MyEvent, null, ({ field1, field2 }) => {
  // function
})
```

## 完整的例子

以下示例场景发出并侦听事件。

```ts
// Initiate event manager
const events = new EventManager()

// Define an event type
@EventConstructor()
class UpdateEvent {
  constructor(public entity: Entity, public dt: number) {}
}

// Define a system
export class RotatorSystem implements ISystem {
  group = engine.getComponentGroup(Transform)

  update(dt: number) {
    for (let entity of this.group.entities) {

      // Emit custom event
      events.fireEvent(new UpdateEvent(entity, dt))
    }
  }
}

engine.addSystem(new RotatorSystem())

// Add a listener
events.addListener(UpdateEvent, null, ({ entity, dt }) => {
  const transform = entity.getComponent(Transform)
  const euler = transform.rotation.eulerAngles
  euler.y += dt * 10
  transform.rotation.eulerAngles = euler
})

// Add an entity to work with
const cube = new Entity()
const transform = new Transform()

cube.addComponentOrReplace(transform)
transform.position.set(5, 0, 5)

const boxShape = new BoxShape()
cube.addComponentOrReplace(boxShape)

engine.addEntity(cube)
```
