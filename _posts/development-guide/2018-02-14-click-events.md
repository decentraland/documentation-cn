---
date: 2018-02-14
title: Button 事件
description: 如何在场景中处理点击
categories:
  - development-guide
redirect_from:
  - /sdk-reference/event-handling/
  - /development-guide/event-handling/
type: Document
set: development-guide
set_order: 14
---

可以通过鼠标、触摸屏、VR 控制器或其他设备完成点击操作，这些都生成相同类型的事件。

> 注: 点击时距离实体最大可达 10 米。


## OnPointerDown

处理单击事件的最简单方法是将 `OnPointerDown` 组件添加到要单击的实体。

通过在 `OnPointerDown` 组件中编写 lambda 函数，可以声明在鼠标按下时要执行的操作。

```ts
const myEntity = new Entity()
myEntity.addComponent(new BoxShape())

myEntity.addComponent(
  new OnPointerDown(e => {
    log("myEntity clicked", e)
  })
)
```

> 提示:为了使代码更容易阅读，`OnPointerDown` 中的 lambda 函数可以只包含一个函数调用，并由这个函数完成所有逻辑功能。

`OnPointerDown` 函数总是传递一个 _pointer down event_ 对象。 此事件对象包含了对该函数有用的各种属性。有关详细信息，请参阅[按钮事件的属性](#properties-of-button-events)。

```ts
const myEntity = new Entity()
myEntity.addComponent(new BoxShape())

myEntity.addComponent(
  new OnPointerDown(e => {
    log("Click distance: " + e.length)
  })
)
```

> 注意：没有形状组件或其形状 `visible` 设置为 _false_ 的实体不能产生单击事件。


## OnPointerUp

类似地，可以将 `OnPointerUp` 组件添加到实体，以跟踪玩家在指向实体时释放鼠标按钮的时间。

通过在 `OnPointerUp` 组件中编写 lambda 函数，可以声明在释放鼠标按钮时要执行的操作。

```ts
const myEntity = new Entity()
myEntity.addComponent(new BoxShape())

myEntity.addComponent(
  new OnPointerUp(e => {
    log("poiner up", e)
  })
)
```

## 按钮按下和释放事件

只要用户按下或释放输入控制器，就会触发 _button down_ 和 _button up_ 事件。

> 提示：在计算机上，为鼠标（或 trackpad ） _左键_，_ E_ 和 _F_ 键。

只要玩家站在场景中，无论玩家的指针指向何处，每次按下或释放按钮时都会触发这些事件。

<!--
 It doesn't make a difference if the click is also being handled by an entity's `OnClick` component.
 -->

实例化一个 `Input` 对象，使用其 `subscribe()` 方法来启动按钮事件的侦听。 每当捕获到事件时，它就会执行。

`subscribe()` 方法有 4 个参数:

- `eventName`: 动作类型，可以是 `"BUTTON_DOWN"` 或 `"BUTTON_UP"`
- `buttonId`: 侦听的按钮。 这可以是  `ActionButton.POINTER`, `ActionButton.PRIMARY`, 或 `ActionButton.SECONDARY` 
- `useRaycast`: 布尔值，用于定义是否使用光线投射。 如果为 `false`，则按钮事件将不包含在事件发生时指针所指的 `hit` 对象的信息。
- `fn`: 事件发生时执行的功能。

```ts
// Instance the input object
const input = Input.instance

// button down event
input.subscribe("BUTTON_DOWN", ActionButton.POINTER, false, e => {
  log("pointer Down", e)
})

// button up event
input.subscribe("BUTTON_UP", ActionButton.POINTER, false, e => {
  log("pointer Up", e)
})
```

上面的示例在每次按下或释放指针按钮时记录消息和事件对象的内容。

> 注意：此代码只需要对 `subscribe()` 方法执行一次即可保持对事件的轮询。 不要将此添加到系统的 `update()` 函数中，因为那样会在每个帧上注册一个新的侦听器。

`BUTTON_DOWN` and the `BUTTON_UP` 的事件对象都包含各种有用的属性。 有关更多详细信息，请参见[按钮事件属性](#properties-of-button-events)。

### 检测命中实体

如果 `subscribe()` 函数中 `useRaycast` 字段（第三个参数）为 true，并且玩家的指针指向实体，则事件对象包括嵌套的 `hit` 对象。 `hit` 对象包括命中的碰撞实体的信息。

全局按钮事件的射线仅检测具有碰撞网格的实体。 默认情况下，原始形状具有碰撞网格，但 3D 模型需要设置碰撞网格。

> 提示：有关如何向 3D 模型添加碰撞器网格的详细信息，请参见[碰撞]({{ site.baseurl }}{% post_url /3d-modeling/2018-01-12-colliders %})。

```ts
input.subscribe("BUTTON_DOWN",  ActionButton.POINTER, true, e => {
  if ( e.hit){
	let hitEntity = engine.entities[e.hit.entityId]
	hitEntity.addComponent(greenMaterial)
  } 
})
```

上面的示例检查是否有任何实体被命中，如果是，它将获取该实体并加入 material 组件。

> 提示：事件数据返回一个 `entityId`。 如果您想通过该 ID 引用实际实体并以某种方式影响它，请调用 `engine.entities[e.hit.entityId]` 。


## 按钮事件的属性

所有 _button down_ 和 _button up_ 以及来 OnPointerDown 和 OnPointerUp 组件都事件对象包含以下参数：

- `origin`：射线的原点，为 _Vector3_
- `direction`：射线的方向矢量，指向同一方向标准化的 _Vector3_。
- `length`：射线的长度，_number_
- `pointerId`：触发事件指针的 ID（_POINTER_, _PRIMARY_ 或 _SECONDARY_）
- `hit`：_（可选）_描述被点击的实体的对象。 如果点击未命中任何特定实体，则此字段不存在。 `hit` 对象包含以下参数：
 
    - `length`：以米为单位的射线长度，_number_
    - `hitPoint`：射线和实体网格之间的交点，_Vector3_
    - `meshName`：网格的名称（如有）为 _string_
    - `worldNormal`: The normal of the hit, in world space, as a _Vector3_
    - `entityId`：实体的ID（如果适用）_string_

## 按钮状态

您可以使用 _Input_ 对象检查按钮的当前状态 (up 或 down) 。

```ts
let buttonState = input.isButtonPressed(ActionButton.POINTER)
```

如果正按住指针按钮，则上面的语句返回值 _true_，否则返回 _false_。

用 `ActionButton.PRIMARY` 或 `ActionButton.SECONDARY` 作为 `isButtonPressed()` 函数的参数，可以以相同的方式检查 `PRIMARY` and `SECONDARY` 按钮的状态，

可以在系统的 `update()` 函数使用此功能，定期检查按钮状态。

```ts
// Instance the input object
const input = Input.instance

class ButtonChecker {
  update() {
    if (input.isButtonPressed(ActionButton.POINTER)) {
      log("pointer button down")
    } else {
      log("pointer button up")
    }
  }
}

engine.addSystem(new ButtonChecker())
```


## 区分模型中的网格

通常，_.glTF_ 3D 模型由多个网格组成，每个网格都有一个单独的内部名称。 _button down_ 和 _button up_ 事件包括单击特定网格的信息，因此您可以使用此信息触发不同的单击行为。

要查看模型中的网格是如何命名的，可以使用编辑工具打开 3D 模型，例如[Blender](https://www.blender.org/)。

<img src="/images/media/mesh-names.png" alt="Mesh internal names in an editor" width="250"/>

> 提示：您还可以通过输出到控制台，读取所单击的网格名称。

也可以访问由 click 事件返回的 `hit` 对象的 `meshName` 属性。

在下面的示例中，有一个房屋模型，包含一个名为 `firePlace` 的网格。 在点击网格时打开壁炉。


```ts
houseEntity.addComponent(
  new OnPointerDown(e => {
    log("button A Down", e.hit.meshName)
	      if (e.hit.meshName === "firePlace"){
		        // light fire
	      }
  })
)
```

## OnClick

可以使用 `OnClick` 组件跟踪完整的单击事件。 单击包含 _mouse down_ 事件，后跟 _mouse up_ 事件，在两者都指向同一实体时触发。

> 注意：点击主要用于 UI 元素。 对于大多数用例，最好使用 `OnPointerDown` 组件。

通过在 `OnClick` 组件中编写 lambda 函数，可以声明在单击时要执行的操作。

```ts
const myEntity = new Entity()
myEntity.addComponent(new BoxShape())

myEntity.addComponent(
  new OnClick(e => {
    log("myEntity clicked")
  })
)
```

> 提示: 为了使代码更容易阅读，`OnCLick` 中的 lambda 函数可以只包含对包含所有逻辑的单独一个函数的调用。

`OnClick`组件传递的事件信息比 `OnPointerDown` 组件少。 它只包含以下参数：

- `pointerId`: 触发事件指针的 ID (_PRIMARY_ 或 _SECONDARY_)

```ts
const myEntity = new Entity()
myEntity.addComponent(new BoxShape())

myEntity.addComponent(
  new OnClick(e => {
    log("pointer Id" + e.pointerId)
  })
)
```

> 注意：没有形状组件或其形状 `visible` 设置为 _false_ 的实体不能被点击。



<!--

## Custom events

Define an event manager

```ts
export namespace EventManager {

  const subscriptions: Record<string, Array<(params?: any) => void> > = {}

  export function on(evt: string, callback: (params?: any) => void) {
    if (!subscriptions[evt]){
      subscriptions[evt] = []
    }
    subscriptions[evt].push(callback)
  }

  export function emit(evt: string, params?: any) {
    if (subscriptions[evt]){
      subscriptions[evt].forEach(callback => callback(params))
    }
  }
}
```

Import the event manager

```ts
import { EventManager } from 'ts/EventManager'
```

Use it:

```ts
EventManager.emit("test", {test: 5})

EventManager.on("test", function(e) {
  log("test " + e.test)
 })

 ```

-->
