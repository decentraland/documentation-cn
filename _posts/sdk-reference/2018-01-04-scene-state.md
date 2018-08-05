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

场景状态由一系列随时间变化的变量组成。状态因场景中[events]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-03-event-handling %})事件的出现而发生变化。当状态改变时，这将重新触发场景使用新的状态渲染。

![](/images/media/events_state_diagram.jpeg)

如果您熟悉[React](https://reactjs.org/docs/thinking-in-react.html) 框架，您会发现场景处理其状态的方式跟 React 处理组件相似。

```tsx
state = {
  buttonState: 0,
  isDoorClosed: false,
  boxPosition: { x: 0, y: 0, z: 0 }
}
```

首次渲染场景时，必须为每个状态变量赋予初始值。您可以通过声明自定义接口来定义状态对象的类型。这样做是可选的，但我们建议这样做，特别是对于复杂的场景，因为它有助于验证输入并使得调试更为容易。

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

`ScriptableScene` 类可选地接受两个参数：属性（在这种情况下为 `any`，因为没有使用）和场景状态，在这种情况下必须匹配自定义接口中描述的类型 `IState`。

您选择构成场景状态的变量应该是代表场景的最小可能集合，您不应该添加冗余信息。确定是否将一条信息作为状态的一部分，可以问一下以下的几个问题：

- 此信息会随时间而变化吗？如果没有，它可能不应该是状态的一部分。
- 在实例化场景类时是否传递了此信息，如果是这样，它可能应该是 `Props` 而不是状态的一部分。
- 你是否可以通过计算其他状态变量或属性就可以得到这些信息吗？如果是，请不要包括。


## 设置状态

您可以在场景对象的任何方法中设置状态变量的值。为此，请使用 `this.setState`，如下所示：

```tsx
async buttonPressed(){
  this.setState({buttonState : 1 })
}
```

每次更新场景的状态时，都会调用 `render()` 函数使用新状态渲染场景。

>注意：为了防止场景每次都被渲染，你可以使用 [`shouldSceneUpdate()` 函数]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %})， 这样它会只根据某些规则有条件地运行 `Render()` 函数。

如果状态包含多个变量并且 `setState()` 语句只影响其中一个变量，那么它将保持所有其他变量不变。

重要的是，每次更改状态时，都要通过 `setState` 函数执行，不要直接设置值。否则将导致场景周期出现问题。

```tsx
// Wrong
this.state.buttonState = 1

// Correct
this.setState({ buttonState: 1 })
```

When dealing with arrays in the scene state, you can't update a single element in the array at a time. You must set a new value for the variable consiting of an entire new array, including any elements that haven't changed.

处理场景状态中的数组时，不能一次更新数组中的单个元素。 您必须为整个新数组的变量设置新值，包括未更改的任何元素。

## 强制更新

If you always change the scene state through `setState()`, the rendering of your scene should always be in sync with the scene state. However, for exceptional cases you might need to refresh the rendering of the scene manually. To do this, call the `forceUdate()` method.

如果始终通过 `setState()` 更改场景状态，则场景的渲染始终与场景状态同步。 但是，对于特殊情况，您可能需要手动刷新场景的渲染。为此，请调用 `forceUdate()` 方法。

```
this.forceUpdate()
```

### 状态引用

您可以通过 `this.state.<variable name>` 从场景对象中的任何位置引用状态变量的值。

```tsx
async checkDoor(){
  return this.state.isDoorClosed
}
```

在下面的示例中，`render()`方法绘制动态场景，其中实体的位置基于状态中的变量。一旦该变量的值发生变化，渲染的场景也会发生变化。

{% raw %}

```tsx
async render() {
  return (
    <scene>
      <box position={this.state.boxPosition} />
    </scene>
  )
}
```

{% endraw %}

要调试场景，可以使用 `console.log(this.state)` 在场景执行的关键时刻记录整个场景状态。您还可以使用`console.log` 来记录状态中的特定变量。

{% raw %}

```tsx
async sceneDidMount() {  
  this.subscribeTo("pointerDown", e => {
    console.log(this.state)
  })
  // (...)
}
```

{% endraw %}

## 从子组件引用状态

不像 React，所有组件都可以拥有自己的状态，SDK 只允许自定义场景类具有状态。在 react terms 中，场景的所有子组件都是[controlled components 受控组件](https://reactjs.org/docs/forms.html#controlled-components)，这意味着它们可以具有属性但没有自己的状态，并且它们由其父组件控制。

例如，您可以声明一个名为 `elevator` 的电梯实体，并在其中嵌入一系列子实体，并定义自己的功能来打开和关闭门并处理按钮。如果您的场景中有许多电梯，那么您的场景代码将更加高效和清洁。缺点是该子实体中的任何函数都不再能够轻松访问场景的状态。在 `elevator` 实体中，使用表达式 `this.state` 是无效的，因为 `this` 不再引用你的自定义场景类，它引用了`elevator` 实体的当前实例。为了访问存储在场景状态中的信息，您可以：
  
- 在子组件的属性中复制场景状态的值。然后电梯中的函数可以通过 `props。<propertyName>` 访问这些数据。如果场景中有多个级别的继承，例如，如果电梯的按钮是具有自己功能的自定义实体，则状态数据可以根据需要多次从父级属性向下传递给子级属性。但是这样代码可能难以阅读。

- 导入像 [Redux](https://redux.js.org/) 这样的库来创建一个可以保存状态数据并且可以从任何地方一致地引用的全局存储。如果您的场景具有多个级别的继承，这样做是理想的。