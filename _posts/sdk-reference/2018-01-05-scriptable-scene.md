---
date: 2018-01-05
title: 脚本场景对象
description: 学习如何制作脚本场景及其事件。
categories:
  - sdk-reference
type: Document
set: sdk-reference
set_order: 5
---


The `ScriptableScene` object is the base template for all Decentraland scenes. By working with this object, you can ignore the complexities of the low-level API below it. You build your scene by creating a custom class that extends `ScriptableScene`. Your custom class uses the parent's default functions except for the functions you want to override and define explicitly.

`ScriptableScene` 对象是所有 Decentraland 场景的基本模板。通过使用这个对象，您可以忽略下面的低级 API 的复杂性。您可以通过创建一个继承自 `ScriptableScene` 的自定义类来构建您的场景。除了您想要重写和明确定义的函数之外，您的自定义类使用父类的默认函数。

The way that `ScriptableScene` works mimics the way in which a [React component](https://reactjs.org/docs/thinking-in-react.html) works. Like when defining a React component, the scene object has:

`ScriptableScene` 的工作方式模仿了[React 组件](https://reactjs.org/docs/thinking-in-react.html)的工作方式。 与定义 React组件时一样，场景对象具有：

* State: variables for data that is meant to change over time.
* Props: porperties used to pass information on from parent to child.
* Lifecycle methods: a series of default methods that are executed at different times relative to when the scene is mounted, updated and unmounted.

* 状态：随时间变化的数据变量。
* 属性：用于将信息从父类传递给子类的属性。
* 生命周期方法：一系列默认方法，在场景装载、更新和卸载等不同时间执行。


## Scene state
## 场景状态

The scene state is made up of a series of variables that change over time.
场景状态由一系列随时间变化的变量组成。

You can define the type for the state object by declaring a custom interface. Doing this is optional but we recommend it, especially for complex scenes, as it helps validate inputs and makes debugging easier.

您可以通过声明自定义接口来定义状态对象的类型。 这样做是可选的，但我们建议使用它，特别是对于复杂的场景，因为它有助于验证输入并使调试更容易。

```tsx

export interface IState {
    buttonState: number;
    isDoorClosed: boolean;
    boxPosition: vector3
}

export default class Scene extends ScriptableScene<any, IState> {
 
    state = {
        buttonState: 0
        isDoorClosed: false,
        queboxPosition: { x: 0, y: 0, z: 0 },
    };

(...)
```

The `ScriptableScene` class optionally takes two arguments: the properties (`any` in this case, as none are used) and the scene state, which in this case must match the type `IState`, described in the custom interface.

`ScriptableScene` 类可选地接受两个参数：属性（在这种情况下为`any`，因为没有使用）和场景状态，在这里必须匹配自定义接口中描述的类型 `IState`。

You can access the scene's state by writing `this.scene.<variableName>` anywhere in the scene object.

您可以通过在场景对象中的任何位置通过 `this.scene.<variableName>` 来访问场景的状态。

See [scene state]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-04-scene-state %}) for more on how to work with the scene state.

有关如何使用场景状态的更多信息，请参阅[场景状态]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-04-scene-state %})。

## Props
## 属性

Properties are used for passing information from parent to child when instancing a class. In a scene object, they should be used for information that is meant to remain unchanged as the user interacts with the scene. Information that is meant to be changed should be part of the scene *state*.

实例化类时，属性用于将信息从父级传递给子级。 在场景对象中，它们应该用于在用户与场景交互时保持不变的信息。要更改的信息应该是场景*状态*的一部分。

You can pass props as an argument when instancing your custom scene class:

在实例化自定义场景类时，您可以将属性作为参数传递：

```tsx
export default class Scene extends ScriptableScene<Props, State> {
{

(...)
```

If you're using properties in your class, we recommend defining a custom interface for the properties, specifying what properties are expected and their types, this helps validate inputs and makes debugging easier.

如果您在自己的类中使用属性，我们建议为属性定义自定义接口，指定期望的属性及其类型，这有助于验证输入并使调试更容易。

You can access the scene's properties by writing `props.<propertyName>` anywhere in the scene object.

您可以在场景对象中的任何位置通过 `props.<propertyName>` 来访问场景的属性。


## Lifecycle methods
## 生命周期方法

The `ScriptableScene` object has several *lifecycle methods* that are executed at different times relative to when the scene is loaded, interacted with or abandoned. When you define your custom scene class, you override the default lifecycle methods of the `ScriptableScene` class, this lets you write code that runs at particular times in the process. 

`ScriptableScene` 对象有几个*生命周期方法*，在场景装载、更新和卸载等不同时间执行。 定义自定义场景类时，可以覆盖 `ScriptableScene`类的默认生命周期方法，这使您可以编写在该过程中的特定时间运行的代码。

As general rules, remember that:

作为一般规则，请记住：

* Methods prefixed with `will` are called right before something happens.
* Methods prefixed with `did` are called right after something happens.

* 前缀为 `will` 的方法会在事件发生之前调用。
* 前缀为 `did` 的方法在事件发生后调用。

Below is an example containing a class and hooks to lifecycle methods.

下面是一个包含类和生命周期方法的示例。


```tsx
import { ScriptableScene, createElement } from 'metaverse-api'

interface State {
  counter: number
}

interface Props {

}

export default class Scene extends ScriptableScene<Props, State> {
  eventSubscriber: EventSubscriber
  timer: number
  state: State = { counter: 0 }

  /**
   * Called immediately after the scene is mounted. You must start your processes, timers, pollers in this method.
   * Setting the value of a state variable here would trigger re-rendering of the scene.
   */
  async sceneDidMount() {
    this.eventSubscriber = new EventSubscriber(this.entityController)

    this.eventSubscriber.on('button_click', evt => {
      this.setState({ counter: this.state.counter + 1 })
    })
  }

  /**
   * This method is called when the scene is initially created and then each time the scene's state or props are updated, unless the `shouldSceneUpdate` method prevents it.
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
        <box id="button" color="#ff0000" scale={2}/>
        <box scale={barScale} />
      </scene>
    )
  }

  /**
   * Called to determine whether a change in state or props should trigger a re-render.
   *
   * If this method returns `true`, `render`, and `sceneDidUpdate` are called.
   */
  async shouldSceneUpdate(newProps: Props) {
    return this.state.counter < 20;
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
   * Called immediately before a scene is destroyed. Perform any necessary cleanup in this
   * method, such as cancelled network requests, or cleaning up any elements created in
   * `sceneDidMount`.
   */
  async sceneWillUnmount() {
    // gracefully tear down created things.
  }

  /**
   * Makes a subscription to remote events, those events occur in the context of the game and are sent thru the wire
   * protocol.
   *
   * @param event name of the remote event to listen
   * @param handler an async
   */
  subscribeTo<T extends IEventNames>(event: T, handler: (data: IEvents[T]) => void | Promise<void>)
}
```

These steps summarize when each of the methods above are called:
这些步骤总结了上述每种方法的调用时间：

1. The user enters your scene, a series of processes are run in the backround by the low level API.
2. After all requirements are fulfilled, `sceneDidMount()` is called.
3. `render()` is then called for the first time.
4. The user then navigates the scene, creating various events by interacting with it. If any of those events leads to a change in the scene's state, then `shouldSceneUpdate()` is called. If this function returns `true`, then:

    1. `sceneDidUpdate()` is called.
    2. `render()` is called again.
5. If the user then leaves the scene, then `sceneWillUnmount()` is called to gracefully clean up the scene before it is unmounted.

1. 用户进入您的场景，由低级 API 在后台运行一系列进程。
2. 满足所有要求后，调用 `sceneDidMount()`。
3. 然后第一次调用`render()`。
4. 然后，用户浏览场景，通过与场景交互来创建各种事件。 如果这些事件中的任何一个导致场景状态发生变化，则调用`shouldSceneUpdate()`。 如果此函数返回`true`，则：

     1. 调用 `sceneDidUpdate()`。
     2. 再次调用 `render()`。
5. 如果用户离开场景，则调用 `sceneWillUnmount()` 以在卸载之前优雅地清理场景。

## Client side scenes
## 客户端场景

Client-side scenes, also known as "Local scenes", are compiled using the `metaverse-compiler` into a WebWorker. The `scene.json` points to the compiled `scene.js` file.

The entry point for the WebWorker is defined in the `build.json`:

客户端场景，也称为“本地场景”，由 `metaverse-compiler` 编译到 WebWorker 中。 `scene.json` 指向编译的 `scene.js` 文件。

WebWorker的入口点在 `build.json` 中定义：

```json
[{
  "name": "Compile local scene",
  "kind": "Webpack",
  "file": "./scene.tsx",
  "target": "webworker"
}]
```

> **Note:** The file `scene.tsx` must include an `export default` statement for the class of the scene. The loader of the engine requires this statment to instantiate the scene.

> **注意：** 文件 `scene.tsx` 必须包含场景类的 `export default` 语句。 引擎的加载程序需要此语句来实例化场景。

## Server side scenes
## 服务器端场景

For local scenes, all of the components that describe your experience are compiled into one script. When your parcels are rendered in the client, this script runs in the context of a WebWorker, or remotely in a server.  This makes it possible for you to run custom logic inside the player's client, allowing you to create richer experiences in Decentraland.

对于本地场景，描述您体验的所有组件都编译为一个脚本。 当您的地块在客户端中呈现时，此脚本在 WebWorker 的上下文中运行，或在服务器中远程运行。 这使您可以在玩家的客户端内运行自定义逻辑，从而可以在 Decentraland 中创建更丰富的体验。

When you create a multiplayer scene, your `scene.json` will point to a host URL. The `scene.json` script will communicate with the host application through a RPC based protocol using a defined transport.  The CLI bootstraps a server that configures a `WebSocketTransport` and inits a RemoteScene.  For running this scene you should start the WebSocket server first.

当您创建多人游戏场景时，您的 `scene.json` 将指向主机的 URL。 `scene.json` 脚本将使用定义的传输通过基于 RPC 的协议与主机应用程序通信。 CLI 启动配置了 `WebSocketTransport` 的服务器并初始化远程场景。 要运行此场景，您应首先启动 WebSocket 服务器。

You can take a look at the [remote scene sample code](https://github.com/decentraland/sample-scene-server) for more implementation details.

您可以查看[远程场景示例代码](https://github.com/decentraland/sample-scene-server)以获取更多实现细节。


## Low level API
## 低级API

The protocol and internals that the `ScriptableScene` object interfaces with in the background are explained in the [metaverse-rpc](https://github.com/decentraland/metaverse-rpc)
respository. 

`scriptableScene` 对象与后台交互的协议和内部结构在 [metaverse-rpc](https://github.com/decentraland/metaverse-rpc) 中有说明。

It's important to understand the way `entityController` gets injected into the class.

理解 `entityController` 注入类的方式很重要。

1. The `ScriptableScene` class is decorated with the `EntityController` as an injected API, like this:
  ```tsx
class ScriptableScene extends Script {
  @inject('EntityController')
  entityController: EntityController = null
}
  ```
1. An instance of the scene class you defined is created using a [transport](https://github.com/decentraland/metaverse-rpc#transports) as an argument.
2. Once the class is created, it requires an instance of `EntityController` from the host (the engine), this is an asynchronous call.
3. The host responds to that request and a proxy is created and assigned to the property `entityController`.
4. Once all requirements are fulfilled, the scene object calls the `sceneDidMount()` method. At this point, you can be sure the required APIs are loaded in your class instance.


5. `ScriptableScene` 类使用 `EntityController` 作为 injected API，如下所示：
   
  ```tsx
class ScriptableScene extends Script {
  @inject('EntityController')
  entityController: EntityController = null
}
  ```
2. 使用 [transport](https://github.com/decentraland/metaverse-rpc#transports) 作为参数创建您定义的场景类的实例。
3. 一旦创建了类，它需要来自主机（引擎）的 `EntityController` 实例，这是一个异步调用。
4. 主机响应该请求，创建相应的代理并分配给属性 `entityController`。
5. 一旦满足所有要求，场景对象就会调用 `sceneDidMount()` 方法。 此时，您可以确认在类实例中加载了所需的 API。