---
date: 2018-01-10
title: 多人场景
description: 开发多用户场景技巧
redirect_from:
  - /documentation/remote-scene-considerations/
categories:
  - development-guide
type: Document
set: development-guide
set_order: 30
---

Decentraland 在玩家的浏览器中本地运行场景。 默认情况下，相互间玩家可以看到并直接互动，但每个玩家都独立与环境互动。 缺省情况下玩家之间并不共享环境的变化。需要手动实现此功能。

让所有玩家在相同内容的场景中看到同样的状态，对于玩家以更有意义的方式进行交互非常重要。如果不是这样，那么如果玩家打开门进入房内，其他的玩家看到的是该门仍然关闭着，并且玩家看起来是直接通过了关闭的门走向其他玩家。

有两种方法可以在所有玩家间同步看到的场景状态：

- 在用户之间发送 P2P 消息以更新更改
- 使用授权服务器跟踪场景的状态

第一个选项是最容易实现的。 缺点是更依赖玩家间的连接速度。 此外，如果有可以利用的激励措施（例如：游戏中得分最高的玩家有奖品，或者游戏中存在交易），则始终建议使用授权服务器，因为这样可以让您获得更多控制并减少漏洞。

## P2P 消息

### 启动消息总线

可以创建消息总线对象，来处理玩家间发送和接收消息。


```ts
const sceneMessageBus = new MessageBus();
```

### 发送信息

使用 `.emit` 命令向场景中的所有其他玩家发送消息。

```ts
const sceneMessageBus = new MessageBus()

box1.AddComponent(
  new OnClick(e => {
    sceneMessageBus.emit("box1Clicked", {})
}))

```

每条消息的第二个参数是一个 `Object` 类型，可以包含您希望发送的任何相关数据。

```ts
const sceneMessageBus = new MessageBus()

let spawnPos = new Vector3(5, 0, 5)

sceneMessageBus.emit("spawn", {position: spawnPos })
```

> 提示：如果需要单条消息来发送包含多个变量的数据，请创建一个自定义类型以将所有这些数据保存在单个对象中。


### 接收消息

要处理该场景中所有其他玩家的消息，请使用 `.on`。 使用此函数时，要提供消息字符串并定义要执行的函数。 每次有匹配字符串的消息到达时，给定的函数执行一次。

```ts
const sceneMessageBus = new MessageBus()

sceneMessageBus.on("spawn", (info: NewBoxPosition) => {
  let newCube = new Entity()
  let transform = new Transform()
  transform.position.set(info.position.x, info.position.y, info.position.z)
  newCube.addComponent(transform)
  engine.addComponent(newCube)
});
```

> 注意：玩家发送的消息也会被同一玩家接收。 `.on` 方法无法区分同一玩家发出的消息还是其他玩家发出的消息。

### 完整的例子

此示例使用消息总线在每次单击主立方体时发送新消息，从而在随机位置生成新的立方体。 该消息包括新立方体的位置，以便所有玩家在相同位置看到这些新立方体。

```ts
  
  /// --- Spawner function ---
  
  function spawnCube(x: number, y: number, z: number) {
    // create the entity
    const cube = new Entity();
  
    // add a transform to the entity
    cube.addComponent(new Transform({ position: new Vector3(x, y, z) }));
  
    // add a shape to the entity
    cube.addComponent(new BoxShape());
  
    // add the entity to the engine
    engine.addEntity(cube);
  
    return cube;
  }
  
/// --- Create message bus ---
const sceneMessageBus = new MessageBus();

/// --- Define a custom type to pass in messages ---
type NewBoxPosition = {
  position: ReadOnlyVector3;
};

/// --- Call spawner function ---
const cube = spawnCube(8, 1, 8);

/// --- Emit messages ---
cube.addComponent(
  new OnClick(() => {
    const action: NewBoxPosition = {
      position: {
        x: Math.random() * 8 + 1,
        y: Math.random() * 8,
        z: Math.random() * 8 + 1
      }
    };

    sceneMessageBus.emit("spawn", action);
  })
);

/// --- Receive messages ---
sceneMessageBus.on("spawn", (info: NewBoxPosition) => {
  cube.getComponent(Transform).scale.z *= 1.1;
  cube.getComponent(Transform).scale.x *= 0.9;

  spawnCube(info.position.x, info.position.y, info.position.z);
});
```

### 在本地测试 P2P 场景

如果您启动场景预览，并在两个（或更多）不同的浏览器窗口中打开它，则每个打开的窗口将被解释为单独的玩家，模拟通信服务器将使这些玩家保持同步。

在一个窗口上与场景交互，然后切换到另一个窗口，以查看该交互的效果在那里也可见。

> 注意：请打开单独的浏览器_窗口_。 如果是在同一窗口中打开单独的_选项卡_，则交互功能将无法正常工作，因为浏览器每次只将一个选项卡视为活动。

## 使用授权服务器

### 授权服务器的示例场景

要复制实现授权威服务器的场景示例，请按照[复制场景示例]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %}#clone-an-example-scene)中的步骤进行操作。

[Door scene](https://github.com/decentraland-scenes/Remote-door)

[Mural scene](https://github.com/decentraland-scenes/Remote-mural)

### 使用授权服务器预览场景

要预览使用授权服务器的场景，必须同时运行它所依赖的场景和服务器。 作为测试的简便方法，服务器可以在与预览相同的机器上本地运行。

要启动服务器，请在 `/server` 文件夹下运行 `npm start`。

一旦服务器运行，不管是远程还是本地，您就可以像本地场景一样运行 `dcl start`。

场景预览运行后，您可以打开指向同一本地地址的多个浏览器选项卡。每个选项卡将在同一场景中实例化一个单独的玩家，这些玩家在场景变化时将共享相同的场景状态。

有关详细信息，请参阅[场景预览]({{ site.baseurl }}{% post_url /getting-started/2018-01-04-preview-scene %})。

### 多人游戏的持久性

与每次玩家走进时重新加载的本地场景不同，使用授权服务器场景的寿命远远超出玩家进入和离开场景时的寿命。

因此，您必须在设计同时，考虑到玩家不会总是在相同的初始状态下进入场景。

对场景所做的任何更改都可能会让其他用户发觉，您必须确保这些更改不会以不希望的方式干扰未来用户的体验。

#### 重置状态

加载场景时，请确保基于存储在服务器中的共享信息构建场景，而不是处于默认状态。

在某些情况下，在场景中包含某种重置按钮是有意义的。按下重置按钮可以优雅地重置场景。

有时，这只是意味着将场景状态中的变量设置回默认值。但重置场景可能还涉及取消事件侦听并停止服务器端的循环。如果每次重置场景时仍然保留空循环，那么这些循环就会不断堆积，并对场景的性能产生不良影响。

> 提示：要停止基于时间的循环，可以使用 `clearInterval()`，并将 loop id 传递给它。
