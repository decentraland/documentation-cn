---
date: 2018-01-05
title: ScriptableScene 对象
description: 学习如何设计 ScriptableScene 及其事件。
categories:
  - sdk-reference
type: Document
set: sdk-reference
set_order: 5
---

`ScriptableScene` 对象是所有 Decentraland 场景的基本模板。通过使用这个对象，您可以忽略下面的低级 API 的复杂性。您可以通过创建一个继承自 `ScriptableScene` 的自定义类来构建您的场景。除了您想要重写和明确定义的函数之外，您的自定义类使用父类的默认函数。

`ScriptableScene` 的工作方式模仿了[React 组件](https://reactjs.org/docs/thinking-in-react.html)的工作方式。 与定义 React组件时一样，场景对象具有：

- 状态：随时间变化的数据变量。
- 属性：用于将信息从父类传递给子类的属性。
- 生命周期方法：一系列默认方法，在场景装载、更新和卸载等不同时间执行。

## 场景状态

场景状态由一系列随时间变化的变量组成。

您可以通过声明自定义接口来定义状态对象的类型。 这样做是可选的，但我们建议使用它，特别是对于复杂的场景，因为它有助于验证输入并使调试更容易。

```tsx
export interface IState {
  buttonState: number
  isDoorClosed: boolean
  boxPosition: Vector3Component
}

export default class Scene extends ScriptableScene<any, IState> {
  state = {
    buttonState: 0,
    isDoorClosed: false,
    queboxPosition: { x: 0, y: 0, z: 0 }
  }
  // (...)
}
```

`ScriptableScene` 类可选地接受两个参数：属性（在这种情况下为`any`，因为没有使用）和场景状态，在这里必须匹配自定义接口中描述的类型 `IState`。

您可以通过在场景对象中的任何位置通过 `this.scene.<variableName>` 来访问场景的状态。

有关如何使用场景状态的更多信息，请参阅[场景状态]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-04-scene-state %})。

## 属性

实例化类时，属性用于将信息从父级传递给子级。 在场景对象中，它们应该用于在用户与场景交互时保持不变的信息。要更改的信息应该是场景*状态*的一部分。

在实例化自定义场景类时，您可以将属性作为参数传递：

```tsx
export default class Scene extends ScriptableScene<Props, State> {
  // (...)
}
```

如果您在自己的类中使用属性，我们建议为属性定义自定义接口，指定期望的属性及其类型，这有助于验证输入并使调试更容易。

您可以在场景对象中的任何位置通过 `props.<propertyName>` 来访问场景的属性。

## 生命周期类方法

`ScriptableScene` 对象有几个*生命周期类方法*，可在场景装载、更新和卸载等不同时间执行。定义自定义场景类时，可以覆盖 `ScriptableScene`类的默认生命周期方法，这使您可以编写在该过程中的特定时间运行的代码。

作为一般规则，请记住：

- 前缀为 `will` 的方法会在事件发生之前调用。
- 前缀为 `did` 的方法在事件发生后调用。

下面是一个包含类和生命周期方法的示例。

```tsx
import { ScriptableScene, createElement } from "metaverse-api"

interface State {
  counter: number
}

interface Props {}

export default class Scene extends ScriptableScene<Props, State> {
  eventSubscriber: EventSubscriber
  timer: number
  state: State = { counter: 0 }

  /**
   * Called immediately after the scene is mounted. You must start your
   * processes, timers, pollers in this method.
   * Setting the value of a state variable here would trigger re-rendering of
   * the scene.
   */
  async sceneDidMount() {
    this.eventSubscriber = new EventSubscriber(this.entityController)

    this.eventSubscriber.on("button_click", evt => {
      this.setState({ counter: this.state.counter + 1 })
    })
  }

  /**
   * This method is called when the scene is initially created and then each
   * time the scene's state or props are updated, unless the `shouldSceneUpdate`
   * method prevents it.
   * This method should return a tree of entities to be serialized and sent
   * to the engine. This tree should have a single <scene> as the root entity.
   */
  async render() {
    const barScale = {
      x: 1,
      y: 1 + this.state.counter,
      z: 1
    }

    return (
      <scene>
        <box id="button" color="#ff0000" scale={2} />
        <box scale={barScale} />
      </scene>
    )
  }

  /**
   * Called to determine whether a change in state or props should trigger a
   * re-render.
   *
   * If this method returns `true`, `render`, and `sceneDidUpdate` are called.
   */
  async shouldSceneUpdate(newProps: Props) {
    return this.state.counter < 20
  }

  /**
   * Called immediately after any change occurs to the state or props,
   * unless shouldSceneUpdate returns false.
   * Not called for the initial render.
   */
  async sceneDidUpdate() {
    // stub
  }

  /**
   * Called immediately before a scene is destroyed. Perform any necessary
   * cleanup in this method, such as cancelled network requests, or cleaning up
   * any elements created in
   * `sceneDidMount`.
   */
  async sceneWillUnmount() {
    // gracefully tear down created things.
  }

  /**
   * Makes a subscription to remote events, those events occur in the context of
   * the game and are sent thru the wire protocol.
   *
   * @param event name of the remote event to listen
   * @param handler an async
   */
  subscribeTo<T extends IEventNames>(
    event: T,
    handler: (data: IEvents[T]) => void | Promise<void>
  )
}
```

这些步骤总结了上述每种方法的调用时间：

1. 用户进入您的场景，由低级 API 在后台运行一系列进程。
2. 满足所有要求后，调用 `sceneDidMount()`。
3. 然后第一次调用`render()`。
4. 然后，用户浏览场景，通过与场景交互来创建各种事件。 如果这些事件中的任何一个导致场景状态发生变化，则调用`shouldSceneUpdate()`。 如果此函数返回`true`，则：

     1. 调用 `sceneDidUpdate()`。
     2. 再次调用 `render()`。
5.  如果用户离开场景，则调用 `sceneWillUnmount()` 以在卸载之前优雅地清理场景。

## 客户端场景

客户端场景，也称为“本地场景”，由 `metaverse-compiler` 编译到 WebWorker 中。 `scene.json` 指向编译的 `scene.js` 文件。

WebWorker的入口点在 `build.json` 中定义：

```json
[
  {
    "name": "Compile local scene",
    "kind": "Webpack",
    "file": "./scene.tsx",
    "target": "webworker"
  }
]
```

> **注意：** 文件 `scene.tsx` 必须包含场景类的 `export default` 语句。 引擎的加载程序需要此语句来实例化场景。

## 服务器端场景

对于本地场景，描述您体验的所有组件都编译为一个脚本。 当您的地块在客户端中呈现时，此脚本在 WebWorker 的上下文中运行，或在服务器中远程运行。 这使您可以在玩家的客户端内运行自定义逻辑，从而可以在 Decentraland 中创建更丰富的体验。

当您创建多人游戏场景时，您的 `scene.json` 将指向主机的 URL。 `scene.json` 脚本将使用定义的传输通过基于 RPC 的协议与主机应用程序通信。 CLI 启动配置了 `WebSocketTransport` 的服务器并初始化远程场景。 要运行此场景，您应首先启动 WebSocket 服务器。

您可以查看[远程场景示例代码](https://github.com/decentraland/sample-scene-server)以获取更多实现细节。

## 低级 API

`scriptableScene` 对象与后台交互的协议和内部结构在 [metaverse-rpc](https://github.com/decentraland/metaverse-rpc) 中有说明。

理解 `entityController` 注入类的方式很重要。

1. `ScriptableScene` 类使用 `EntityController` 作为 injected API，如下所示：
   
    ```tsx
    class ScriptableScene extends Script {
      @inject("EntityController") entityController: EntityController = null
    }
    ```

2. 使用 [transport](https://github.com/decentraland/metaverse-rpc#transports) 作为参数创建您定义的场景类的实例。
3. 一旦创建了类，它需要来自主机（引擎）的 `EntityController` 实例，这是一个异步调用。
4. 主机响应该请求，创建相应的代理并分配给属性 `entityController`。
5. 一旦满足所有要求，场景对象就会调用 `sceneDidMount()` 方法。 此时，您可以确认在类实例中加载了所需的 API。