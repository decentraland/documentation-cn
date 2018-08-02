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

When users interact with the entities in your scene, these generate several types of events. These events can have an effect on the scene [state]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-04-scene-state %}), which triggers a new rendering of the scene.

当用户与场景中的实体交互时，会生成几种类型的事件。这些事件会对场景[状态(state)]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-04-scene-state %})产生影响，并触发一个新的场景渲染。

![](/images/media/events_state_diagram.jpeg)



Generally, a good way of having your scene respond to events is to set up a listener in the `sceneDidMount()` method. See [scriptable scene]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %}) for more context about when this method is executed.

通常，场景响应事件的一个好方法是在 `sceneDidMount()` 方法中设置监听器。有关何时执行此方法的更多信息请查看 [可编程场景]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %}) 。

```tsx
   async sceneDidMount() {
        this.eventSubscriber.on(`pointerDown`, () => console.log("pointer down"));
    }
```


To debug a scene, you can use `console.log()` to keep track of the occurance of events or to verify that the event's parameters are what you expected.

若要调试场景，可以使用 `console.log()` 来跟踪事件的发生或验证事件的参数是否符合预期。


## Clicking
## 点击

Clicks can be done either with a mouse, a touch screen, a VR controller or some other device, the events that are generated from this don't make any distinction. When the ray that the avatar casts forward points at a valid entity and the user clicks, this creates a `click` event.

点击可以由鼠标、触摸屏、VR 控制器或其他设备引起，并且生成的事件没有任何区别。当游戏中用户化身向前投射的光线指向一个有效的实体并被用户单击时，会创建一个 `click` 事件。

> Note: Only entities that have an id can generate click events. Clicks can be made from a maximum distance of 10 meters away from the entity.
> 注意：只有具有 id 的实体才能生成 click 事件。click 的最大有效距离是距离实体 10 米。


### The click event
### 点击事件

The generic `click` event represents all clicks done on valid entities. The event has two parameters:
一般的 `click` 事件表示所有对有效实体执行的点击。 该事件有两个参数：

* `elementId`: the Id of the entity that was clicked.
* `pointerId`: the id for the user who performed the click.

* `elementId`: 被点击的实体的 Id .
* `pointerId`: 实施点击的用户的 Id .


{% raw %}
```tsx
import { createElement, ScriptableScene } from 'metaverse-api';

export default class LastClicked extends ScriptableScene {
  state = { 
      lastClicked: "none"
   }

  async sceneDidMount() {   
      this.subscribeTo("click", e => {
            this.setState({ lastClicked: e.elementId });
             console.log(this.state.lastClicked);
        });
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

The example above uses the `subscribeTo` to initiate a listener that checks for all click events. When the user clicks on either of the two boxes, the scene stores the id of the clicked entity in the `lastClicked` state variable and prints it to console.

上面的例子使用 `subscribeTo` 启动一个检查所有点击事件的监听器。 当用户点击两个框中的任何一个时，场景将被点击的实体的 id 存储在 `lastClicked` 状态变量中，并将其打印到控制台。

### Entity-specific click events
### 特定实体的点击事件

A simpler way to deal with clicks that are made on a single entity is to listen for click events that are specific for that entity. The names of entity-specific click events are as follows: the id of the entity, an underscore and then *click*. For example, the event created from clicking an entity called `redButton` is named `redButton_click`.

实现单个实体上的单击事件的一种更简单的方法是侦听特定于该实体的单击事件。特定实体的单击事件的名称如下:实体的 id、下划线，然后是 *click*。例如，单击一个名为 `redButton` 的实体的事件会被命名为 `redButton_click`。

> Note: Entity-specific click events have no properties, so you can't access the user's id from this event.
> 注意：特定于实体的单击事件没有属性，因此您无法在此事件得到用户的 ID。

{% raw %}
```tsx
import { createElement, ScriptableScene } from 'metaverse-api';

export default class RedButton extends ScriptableScene {
  state = { 
      buttonState: false
   }

  async sceneDidMount() {   
    this.eventSubscriber.on('redButton_click', ()) => {
      this.setState({ buttonState: !this.state.buttonState });
      console.log(this.state.buttonState);
    });
  };

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

The scene above uses an `eventSubscriber` to initiate a listener that checks for click events done on the `redButton` entity. Whenever this occurs, the state of `buttonState` is toggled.

上面的场景使用 `eventSubscriber` 来启动一个监听器，检查在 `redButton` 实体上的点击事件。 每当发生这种情况时，就会切换`buttonState` 的状态。

## Pointer down and pointer up
## 指上或指下事件

The pointer down and pointer up events are fired whenever the user presses or releases an input controler. This could be a mouse, a touch screen, a VR controller or another kind of controller. It doesn't matter where the user's avatar is pointing at, the event is triggered every time.

只要用户按下或释放输入控制器，就会触发指下和指上事件。 可以由鼠标、触摸屏、VR 控制器或其他类型的控制器产生。 不管用户在游戏中朝向哪里，事件都会被触发。

{% raw %}
```tsx
import { createElement, ScriptableScene } from 'metaverse-api';

export default class BigButton extends ScriptableScene {
  state = { 
      buttonState: 0
   }

  async sceneDidMount() {  
    this.subscribeTo("pointerDown", e => {
        this.setState({ buttonState: 0});
    }); 
    this.subscribeTo("pointerUp", e => {
        this.setState({ buttonState: 1});
    });
  }

  async render() {
    return (
      <scene>
          <box id="button" 
              position={{ x: 3, y: this.state.buttonState, z: 3 }} 
              transition={{ position: { duration: 200, timing: "linear" }}} 
          />
      </scene>
    )
  }
}
```
{% endraw %}

The scene above uses two `subscribeTo` functions to initiate listeners that check both when the user clicks or releases a pointer button. Both listener functions alter the `buttonState` state variable in the scene. This variable is then used to set the height of a box that mimics the pressing of the user's button.

上面的场景使用两个 `subscribeTo` 函数来启动监听器，检查用户何时单击或释放指针按钮。 两个监听器函数都会改变场景中的`buttonState` 状态变量。 然后使用此变量来设置方框的高度以模仿用户按下按钮。

## Position change
## 位置变化

The `positionChanged` event sends the position of the user each time it changes.

`positionChanged` 事件由每次用户的位置发生变化时触发。


The `positionChanged` event has the following properties:

`positionChanged` 事件有以下属性：

* `position`: a Vector3Component with the user's position relative to the base parcel of the scene.
* `cameraPosition`: a Vector3Component with the user's absolute position relative to the world.
* `playerHeight`: the eye height of the user, in meters.

* `position`: 包含用户相对于场景中基础地块的位置的三维矢量。
* `cameraPosition`: 用户相对于虚拟世界的绝对位置的三维矢量。
* `playerHeight`: 用户的眼睛高度，以米为单位。


{% raw %}
```tsx
import { createElement, ScriptableScene } from 'metaverse-api';

export default class BoxFollower extends ScriptableScene {
  state = { 
      boxPosition: { x: 0, y: 0, z: 0 } 
  }

  async sceneDidMount() {
      this.subscribeTo('positionChanged', e => {
          this.setState({ boxPosition: e.position });         
      });
  }

  async render() {
    return (
      <scene>
        <box position= {this.state.boxPosition} ignoreCollisions />
      </scene>
    )
  }
}
```
{% endraw %}

The scene above uses a `subscribeTo` function to initiate a listener to track when the position of the user changes. When the user moves, the scene stores the current position in the state variable `boxPosition`, which is used to set the position of a box that follows the user around.

上面的场景使用 `subscribeTo` 函数来启动监听器以跟踪用户位置变化。 当用户移动时，场景将当前位置存储在状态变量 `boxPosition` 中，该状态变量用于设置跟随用户的方框的位置。

## Rotation change
## 转向变化

The `rotationChanged` event sends the angle in which the user is looking each time it changes.

`rotationChanged` 事件发送用户查看角度的变化。

The `rotationChanged` event has the following properties:
 `rotationChanged` 事件有以下属性：

* `rotation`: a Vector3Component with the user's rotation.

* `quaternion`: the rotation of the user expressed as a quaternion.

* `rotation`: 用户转向的三维矢量.

* `quaternion`: 用四元数表示的用户转向.


{% raw %}
```tsx
import { createElement, ScriptableScene } from 'metaverse-api';

export default class ConeHead extends ScriptableScene {
  state = { 
    rotation: { x: 0, y: 0, z: 0 } 
  }

  async sceneDidMount() {
      this.subscribeTo('rotationChanged', e => {
          e.rotation.x +=  90;
          this.setState({ rotation: e.rotation });
      });
  }

  async render() {
    return (
      <scene>
      <cone position={{ x: 0, y: 1, z: 2 }} rotation={this.state.rotation}/>
      </scene>
    )
  }
}
```
{% endraw %}

The scene above uses a `subscribeTo` function to initiate a listener to track the user's rotation. When the user looks in a different direction, the scene stores the current angle in the state variable `rotation`. This example adds another 90 degrees to the X axis of this angle just to make the output more fun to play with. This angle is used to orient a cone that faces and mimics the user.

上面的场景使用 `subscribeTo` 函数来启动一个监听器来跟踪用户的转向。 当用户向不同方向看时，场景将当前角度存储在状态变量 `rotation` 中。 此示例将此角度的 X 轴再添加 90 度，以使输出更有趣。 该角度被用来定位一个面朝用户并模仿用户的圆锥体。
