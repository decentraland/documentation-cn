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

## OnClick

处理点击事件的最简单方法是向要点击的实体添加一个 `OnClick` 组件。

通过在`OnClick`组件中编写 lambda 函数，可以声明在单击事件中要做什么。

```tsx
const myEntity = new Entity()
myEntity.add(new BoxShape())

myEntity.add(
  new OnClick(e => {
    log("myEntity clicked")
  })
)
```

> 提示:为了使代码更容易阅读，`OnCLick` 中的 lambda 函数可以只包含对包含所有逻辑的单独一个函数的调用。

_click event_ 对象总是作为函数的参数隐式传递到 `OnClick` 中。此事件对象包含以下参数，您的函数可以访问这些参数:

- `pointerId`: 触发事件指针的 ID (_PRIMARY_ 或 _SECONDARY_)

```tsx
const myEntity = new Entity()
myEntity.add(new BoxShape())

myEntity.add(
  new OnClick(e => {
    log("pointer Id" + e.pointerId)
  })
)
```

## 按下或释放按钮事件

每当用户按下或释放输入控制器时，都会触发 _button down_ 和 _button up_ 事件。

无论指针指向何处，每次按下或释放按钮时都会触发这些事件。如果单击也由实体的 `OnClick` 组件处理，则没有什么区别。

使用 Input 对象的 `subscribe()` 方法来启动订阅单击事件的侦听器。无论何时捕获事件，它都会执行一个 lambda 函数。

```tsx
// Instance the input object
const input = Input.instance

// button down event
input.subscribe("BUTTON_A_DOWN", e => {
  log("button A Down", e)
})

// button up event
input.subscribe("BUTTON_A_UP", e => {
  log("button A Up", e)
})
```

> 注意: 对于 `subscribe()` 方法，为保持对事件的轮询，此代码只需要执行一次。不要将此添加到系统的`update()`函数中，因为这会在每个帧上注册一个新的侦听器。

所有点击事件对象都包含以下参数:

- `from`: 射线的原点，为 _Vector3_
- `direction`: 射线的方向向量，标准化的 _Vector3_
- `length`: 射线的长度，_number_
- `pointerId`: 触发事件指针的 ID (_PRIMARY_ 或 _SECONDARY_)

## 指针状态

您也可以使用 _Input_ 对象检查按钮的当前状态，而不是创建一个侦听器来捕捉按钮更改状态事件。

```tsx
let buttonState = input.state[Pointer.PRIMARY].BUTTON_A_DOWN
```

如果 _A_ 按钮按下，`BUTTON_A_DOWN` 的值为 _true_，如果 _A_ 按钮放开，它的值为 _false_。

您可以在系统的 `update()` 函数中实现此功能，以便定期检查按钮状态。

```tsx
// Instance the input object
const input = Input.instance

class ButtonChecker {
  update() {
    if (input.state[Pointer.PRIMARY].BUTTON_A_DOWN) {
      log("button A down")
    } else {
      log("button A up")
    }
  }
}

engine.addSystem(new ButtonChecker())
```

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
