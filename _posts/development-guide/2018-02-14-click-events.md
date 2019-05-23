---
date: 2018-02-14
title: 点击事件
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
    log("myEntity clicked")
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

## 通用按钮按下和释放事件

只要用户按下或释放输入控制器，就会触发 _button down_ 和 _button up_ 事件。

无论指针指向何处，每次按下或释放按钮时都会触发事件。它跟由实体的 `OnClick` 组件处理的单击没有什么区别。

使用 Input 对象的 `subscribe()` 方法监听 click 事件。 当事件发生时，它会执行对应的 lambda 函数。

```ts
// Instance the input object
const input = Input.instance

// button down event
input.subscribe("BUTTON_DOWN", e => {
  log("button A Down", e)
})

// button up event
input.subscribe("BUTTON_UP", e => {
  log("button A Up", e)
})
```

`BUTTON_DOWN` 和 `BUTTON_UP` 事件都包含有函数可能需要的各种属性。 有关详细信息，请参阅[按钮事件的属性](#properties-of-button-events)。

> 注意：这个代码只需要执行 `subscribe()` 方法一次，以保持对事件的轮询。 不要将它添加到系统的 `update()` 函数中，因为这会在每个帧上注册一个新的监听器。

## 按钮事件的属性

所有 _button down_ 和 _button up_ 事件对象包含以下参数：

 - `origin`：射线的原点，为 _Vector3_
 - `direction`：射线的方向矢量，标准化的 _Vector3_
 - `length`：射线的长度，_number_
 - `pointerId`：触发事件指针的 ID（_PRIMARY_ 或 _SECONDARY_）
 - `hit`：_（可选）_描述被点击的实体的对象。 如果点击未命中任何特定实体，则此字段不存在。 `hit` 对象包含以下参数：
 
    - `length`：以米为单位的射线长度，_number_
    - `hitPoint`：射线和实体网格之间的交点，_Vector3_
    - `meshName`：网格的名称（如有）为 _string_
    - `normal`: The normal of the hit, as a _Vector3_
    - `worldNormal`: The normal of the hit, in world space, as a _Vector3_
    - `entityId`：实体的ID（如果适用）_string_

## 指针状态

您可以使用 _Input_ 对象检查按钮的当前状态，而不用监听按钮状态更改事件。

```ts
let buttonState = input.state[Pointer.PRIMARY].BUTTON_DOWN
```

如果按下 _A_ 按钮，`BUTTON_DOWN` 的值为 _true_，如果 _A_ 按钮没有按下，则其值为 _false_。

可以在系统的 `update()` 函数中使用，定时检查按钮状态。

```ts
// Instance the input object
const input = Input.instance

class ButtonChecker {
  update() {
    if (input.state[Pointer.PRIMARY].BUTTON_DOWN) {
      log("button A down")
    } else {
      log("button A up")
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

```ts
const input = Input.instance

input.subscribe("BUTTON_DOWN", e => {
  log("button A Down", e.hit.meshName)

  if (e.hit.meshName === "firePlace"){
    // light fire
  }
})
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
