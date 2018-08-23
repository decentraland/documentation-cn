---
date: 2018-01-05
title: 事件处理
description: 了解场景中可能发生的事件以及如何捕捉它们
categories:
  - sdk-reference
type: Document
set: sdk-reference
set_order: 3
---

当用户与场景中的实体交互时，会生成几种类型的事件。这些事件会对场景[状态(state)]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-04-scene-state %})产生影响，并触发一个新的场景渲染。

![](/images/media/events_state_diagram.jpeg)

通常，场景响应事件的一个好方法是在 `sceneDidMount()` 方法中设置监听器。有关何时执行此方法的更多信息请查看 [可编程场景]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %}) 。

```tsx
async sceneDidMount() {
  this.eventSubscriber.on(`pointerDown`, () => console.log("pointer down"))
}
```

若要调试场景，可以使用 `console.log()` 来跟踪事件的发生或验证事件的参数是否符合预期。

## 点击

点击可以由鼠标、触摸屏、VR 控制器或其他设备引起，并且生成的事件没有任何区别。当游戏中用户化身向前投射的光线指向一个有效的实体并被用户单击时，会创建一个 `click` 事件。

> 注意：只有具有 id 的实体才能生成 click 事件。click 的最大有效距离是距离实体 10 米。

#### 点击事件

一般的 `click` 事件表示所有对有效实体执行的点击。 该事件有两个参数：

- `elementId`: 被点击的实体的 Id .
- `pointerId`: 实施点击的用户的 Id .

{% raw %}

```tsx
import { createElement, ScriptableScene } from "metaverse-api"

export default class LastClicked extends ScriptableScene {
  state = {
    lastClicked: "none"
  }

  async sceneDidMount() {
    this.subscribeTo("click", e => {
      this.setState({ lastClicked: e.elementId })
      console.log(this.state.lastClicked)
    })
  }

  async render() {
    return (
      <scene>
        <box id="box1" position={{ x: 2, y: 1, z: 0 }} color="ff0000" />
        <box id="box2" position={{ x: 4, y: 1, z: 0 }} color="00ff00" />
      </scene>
    )
  }
}
```

{% endraw %}

上面的例子使用 `subscribeTo` 启动一个检查所有点击事件的监听器。 当用户点击两个框中的任何一个时，场景将被点击的实体的 id 存储在 `lastClicked` 状态变量中，并将其打印到控制台。

#### 特定实体的点击事件

实现单个实体上的单击事件的一种更简单的方法是侦听特定于该实体的单击事件。特定实体的单击事件的名称如下:实体的 id、下划线，然后是 *click*。例如，单击一个名为 `redButton` 的实体的事件会被命名为 `redButton_click`。

> 注意：特定于实体的单击事件没有属性，因此您无法在此事件得到用户的 ID。

{% raw %}

```tsx
import { createElement, ScriptableScene } from "metaverse-api"

export default class RedButton extends ScriptableScene {
  state = {
    buttonState: false
  }

  async sceneDidMount() {
    this.eventSubscriber.on("redButton_click", ()) => {
      this.setState({ buttonState: !this.state.buttonState })
      console.log(this.state.buttonState)
    })
  }

  async render() {
    return (
      <scene>
        <box id="redButton" color="ff0000" />
      </scene>
    )
  }
}
```

{% endraw %}

上面的场景使用 `eventSubscriber` 来启动一个监听器，检查在 `redButton` 实体上的点击事件。 每当发生这种情况时，就会切换`buttonState` 的状态。

## 指针按下和释放事件

只要用户按下或释放输入控制器，就会触发 `pointerDown` 和 `pointerUp` 事件。 可以由鼠标、触摸屏、VR 控制器或其他类型的控制器产生。 不管用户在游戏中朝向哪里，事件都会被触发。

{% raw %}

```tsx
import { createElement, ScriptableScene } from "metaverse-api"

export default class BigButton extends ScriptableScene {
  state = {
    buttonState: 0
  }

  async sceneDidMount() {
    this.subscribeTo("pointerDown", e => {
      this.setState({ buttonState: 0 })
    })
    this.subscribeTo("pointerUp", e => {
      this.setState({ buttonState: 1 })
    })
  }

  async render() {
    return (
      <scene>
        <box
          id="button"
          position={{ x: 3, y: this.state.buttonState, z: 3 }}
          transition={{ position: { duration: 200, timing: "linear" } }}
        />
      </scene>
    )
  }
}
```

{% endraw %}

上面的场景使用两个 `subscribeTo` 函数来启动监听器，检查用户何时单击或释放指针按钮。 两个监听器函数都会改变场景中的 `buttonState` 状态变量。 然后使用此变量来设置方框的高度以模仿用户按下按钮。

## 位置变化

`positionChanged` 事件由每次用户的位置发生变化时触发。

`positionChanged` 事件有以下属性：

- `position`: 包含用户相对于场景中基础地块的位置的三维矢量。
- `cameraPosition`: 用户相对于虚拟世界的绝对位置的三维矢量。
- `playerHeight`: 用户的眼睛高度，以米为单位。

{% raw %}

```tsx
import { createElement, ScriptableScene } from "metaverse-api"

export default class BoxFollower extends ScriptableScene {
  state = {
    boxPosition: { x: 0, y: 0, z: 0 }
  }

  async sceneDidMount() {
    this.subscribeTo("positionChanged", e => {
      this.setState({ boxPosition: e.position })
    })
  }

  async render() {
    return (
      <scene>
        <box position={this.state.boxPosition} />
      </scene>
    )
  }
}
```

{% endraw %}

上面的场景使用 `subscribeTo` 函数来启动监听器以跟踪用户位置变化。 当用户移动时，场景将当前位置存储在状态变量 `boxPosition` 中，该状态变量用于设置跟随用户的方框的位置。

## 转向变化

`rotationChanged` 事件发送用户查看角度的变化。

 `rotationChanged` 事件有以下属性：

- `rotation`: 用户转向的三维矢量.

- `quaternion`: 用四元数表示的用户转向.

{% raw %}

```tsx
import { createElement, ScriptableScene } from "metaverse-api"

export default class ConeHead extends ScriptableScene {
  state = {
    rotation: { x: 0, y: 0, z: 0 }
  }

  async sceneDidMount() {
    this.subscribeTo("rotationChanged", e => {
      e.rotation.x += 90
      this.setState({ rotation: e.rotation })
    })
  }

  async render() {
    return (
      <scene>
        <cone position={{ x: 0, y: 1, z: 2 }} rotation={this.state.rotation} />
      </scene>
    )
  }
}
```

{% endraw %}

上面的场景使用 `subscribeTo` 函数来启动一个监听器来跟踪用户的转向。 当用户向不同方向看时，场景将当前角度存储在状态变量 `rotation` 中。 此示例将此角度的 X 轴再添加 90 度，以使输出更有趣。 该角度被用来定位一个面朝用户并模仿用户的圆锥体。
