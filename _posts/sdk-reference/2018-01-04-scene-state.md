---
date: 2018-01-05
title: 场景状态
description: 如何更新和取回场景的状态变量
categories:
  - sdk-reference
type: Document
set: sdk-reference
set_order: 4
---


The scene state is made up of a series of variables that change over time. The state changes by the occurance of [events]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-03-event-handling %}) in the scene. When the state changes, this retriggers the rendering of the scene, using the new values of the state. 

场景状态由一系列随时间变化的变量组成。状态因场景中[events]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-03-event-handling %})事件的出现而发生变化。当状态改变时，这将重新触发场景使用新的状态渲染。



![](/images/media/events_state_diagram.jpeg)


If you're familiar with the [React](https://reactjs.org/docs/thinking-in-react.html) framework, you'll find that the scene handles its states in a way that's very similar to how components in React do this.

如果您熟悉[React](https://reactjs.org/docs/thinking-in-react.html) 框架，您会发现场景处理其状态的方式跟 React 处理组件相似。


```tsx
    state = {
        buttonState: 0
        isDoorClosed: false,
        boxPosition: { x: 0, y: 0, z: 0 },
    };
```
Each state variable must be given an intial value for when the scene is first rendered. You can define the type for the state object by declaring a custom interface. Doing this is optional but we recommend it, especially for complex scenes, as it helps validate inputs and makes debugging easier.

首次渲染场景时，必须为每个状态变量赋予初始值。您可以通过声明自定义接口来定义状态对象的类型。这样做是可选的，但我们建议这样做，特别是对于复杂的场景，因为它有助于验证输入并使得调试更为容易。

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

`ScriptableScene` 类可选地接受两个参数：属性（在这种情况下为 `any`，因为没有使用）和场景状态，在这种情况下必须匹配自定义接口中描述的类型 `IState`。


The variables you choose to make up your scene's state should be the minimal possible set that represents the scene, you shouldn't add redundant information. To determine if you should include a piece of information as part of the state, ask yourself:

您选择构成场景状态的变量应该是代表场景的最小可能集合，您不应该添加冗余信息。确定是否将一条信息作为状态的一部分，可以问一下以下的几个问题：

* Does this information change over time? If it doesn't, it probably shouldn't be part of the state.
* Is this information passed in when instantiating the scene class, if so it probably should be part of `Props` rather than of the state.
* Can you compute this information based on other state variables or the props? If so, don't include it.

* 此信息会随时间而变化吗？如果没有，它可能不应该是状态的一部分。
* 在实例化场景类时是否传递了此信息，如果是这样，它可能应该是 `Props` 而不是状态的一部分。
* 你是否可以通过计算其他状态变量或属性就可以得到这些信息吗？如果是，请不要包括。


### Set the state
### 设置状态

You can set the value of a state variable from any method in the scene object. To do so, use `this.setState` as shown below:

您可以在场景对象的任何方法中设置状态变量的值。为此，请使用 `this.setState`，如下所示：


```tsx

async buttonPressed(){
     this.setState({buttonState : 1 });
};
```

Each time the scene's state is updated, the `render()` function is called to render the scene using the new state. 

每次更新场景的状态时，都会调用 `render()` 函数使用新状态渲染场景。


> Note: To prevent the scene from being rendered every time, you can use the [`shouldSceneUpdate()` function]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %}) so that it only runs the `Render()` function conditionally based on some rule. 

>注意：为了防止场景每次都被渲染，你可以使用 [`shouldSceneUpdate()` 函数]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %})， 这样它会只根据某些规则有条件地运行 `Render()` 函数。


If the state holds multiple variables and your `setState()` statement only affects one of them, it will leave all other variables untouched.

如果状态包含多个变量并且 `setState()` 语句只影响其中一个变量，那么它将保持所有其他变量不变。


It's important that each time you change the state you do it through the `setState` function, NEVER do it by directly setting a value. Otherwise this will cause problems with the lifecycle of the scene.

重要的是，每次更改状态时，都要通过 `setState` 函数执行，不要直接设置值。否则将导致场景周期出现问题。


```tsx
// Wrong
this.state.buttonState = 1;

// Correct
this.setState({buttonState: 1});
```

### Reference the state
### 状态引用

You can reference the value a state variable from anywhere in the scene object by writing `this.state.<variable name>`.

您可以通过 `this.state.<variable name>` 从场景对象中的任何位置引用状态变量的值。


```tsx
async checkDoor(){
     return this.state.isDoorClosed;
};
```

In the example below, the `render()` method draws a dynamic scene where the position of an entity is based on a variable in the state. As soon as the value of that variable changes, the rendered scene changes as well.

在下面的示例中，`render()`方法绘制动态场景，其中实体的位置基于状态中的变量。一旦该变量的值发生变化，渲染的场景也会发生变化。


{% raw %}
```tsx
async render() {
  return (
    <scene>
          <box position={this.state.boxPosition} />
    </scene>
    );
  }
```
{% endraw %}

To debug a scene, you can use `console.log(this.state)` to log the entire scene state at key moments in the scene's execution. You can also use `console.log` to log a specific variable from the state.

要调试场景，可以使用 `console.log(this.state)` 在场景执行的关键时刻记录整个场景状态。您还可以使用`console.log` 来记录状态中的特定变量。


{% raw %}
```tsx
 async sceneDidMount() {  
    this.subscribeTo("pointerDown", e => {
       console.log(this.state);
    }); 
    (...)
```
{% endraw %}

#### Reference the state from a child component
### 从子组件引用状态

Unlinke React, where all components can have their own state, only your custom scene class is allowed to have a state. In react terms, all child components of your scene are [controlled components](https://reactjs.org/docs/forms.html#controlled-components), this means that they can have properties but no state of their own, and they are controlled by their parent component. 

不像 React，所有组件都可以拥有自己的状态，SDK 只允许自定义场景类具有状态。在 react terms 中，场景的所有子组件都是[controlled components 受控组件](https://reactjs.org/docs/forms.html#controlled-components)，这意味着它们可以具有属性但没有自己的状态，并且它们由其父组件控制。


For example, you could declare a type of entity called `elevator`, and embed a series of child entities in it and define its own functions to open and close the doors and handle the buttons. If you're planning to have many elevators in your scene, that would make the code for your scene a lot more efficient and cleaner. The downside is that any functions in this child entity would no longer have easy access to the scene's state. From the `elevator` entity, it would not be valid to use the expression `this.state`, as `this` no longer refers to your custom scene class, it refers to the current instance of the `elevator` entity. In order to access the information stored in the scene's state you can:

例如，您可以声明一个名为 `elevator` 的电梯实体，并在其中嵌入一系列子实体，并定义自己的功能来打开和关闭门并处理按钮。如果您的场景中有许多电梯，那么您的场景代码将更加高效和清洁。缺点是该子实体中的任何函数都不再能够轻松访问场景的状态。在 `elevator` 实体中，使用表达式 `this.state` 是无效的，因为 `this` 不再引用你的自定义场景类，它引用了`elevator` 实体的当前实例。为了访问存储在场景状态中的信息，您可以：


* Copy the values of the scene state in the properties of the child component. Functions in the elevator could then access this data via `props.<propertyName>`. If there are multiple levels of inheritance in your scene, for example if the elevator's buttons are custom entities with their own functions, the state data can be passed down from the properties of the parent to the properties of the children recurrently as many times as necessary, but by doing this the code can get difficult to read.
  
* 在子组件的属性中复制场景状态的值。然后电梯中的函数可以通过 `props。<propertyName>` 访问这些数据。如果场景中有多个级别的继承，例如，如果电梯的按钮是具有自己功能的自定义实体，则状态数据可以根据需要多次从父级属性向下传递给子级属性。但是这样代码可能难以阅读。


* Import a library like [Redux](https://redux.js.org/) to create a univesal store that can hold the state data and that can be consistently referenced from anywhere. If your scene has multiple levels of inheritance, this might be ideal.

* 导入像 [Redux](https://redux.js.org/) 这样的库来创建一个可以保存状态数据并且可以从任何地方一致地引用的全局存储。如果您的场景具有多个级别的继承，这样做是理想的。


