---
date: 2018-01-05
title: 状态管理
description: 如何更新和读取场景状态变量
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

状态应该只 **包含** 数据，没有逻辑或方法。我们不建议将具有自己方法的对象实例分配给状态中的变量。所有场景的逻辑都应该在您的自定义类中执行，该类扩展了 [scriptable scene]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %}) 类。

首次渲染场景时，必须为每个状态变量赋予初始值。

构成场景状态的变量应该是代表场景的最小可能集合，不应该保存冗余信息。要确定是否应将某数据作为状态的一部分，问一下自己下面几个问题：

- 此信息会随时间而变化吗？如果没有，它应该声明为常量而不是状态。
- 在实例化场景类时是否传递了此信息，如果是这样，它可能应该是 `Props` 而不是状态的一部分。
- 你是否可以通过计算其他状态变量或属性就可以得到这些信息吗？如果是，您应该考虑每次计算它而不是将其存储在状态中。 请记住，这会对场景性能产生影响，因此在某些情况下，最好在场景状态下存储更多信息。

## 本地和远程场景

**本地场景**将场景状态存储在本地，作为 `ScriptableScene` 对象的一部分。 例如，使用 Decentraland CLI 创建场景时， _Basic_ 和 _Interactive_ 选项的默认场景都是本地场景。

{% raw %}

```tsx
export default class Scene extends ScriptableScene {
  state: IState = {
    buttonState: 0,
    isDoorClosed: false,
    queboxPosition: { x: 0, y: 0, z: 0 }
  }

  // (...)
}
```

{% endraw %}

本地场景完全在用户的本地客户端中运行。当场景状态中的变量值发生更改时，它仅更改客户端中的本地表示，并且不会传播到场景的其他用户。这意味着每个用户可能对场景的外观有不同的感知，即使用户可能看到彼此移动。

例如，如果用户打开门，其他用户将不会看到此门打开，因为用户只能影响他们自己的本地场景状态。

**远程场景**将场景状态存储在远程服务器中。本地客户端从此服务器检索信息以便在场景中呈现场景。当场景状态中的变量值更改时，此更改将被推送到远程服务器以更新其场景状态的表示。 然后，场景的所有用户将根据存储在远程服务器中的场景状态在本地渲染其场景，因此所有用户都将看到更改。

回到前面的例子：在远程场景中，如果用户打开门，所有其他用户也应该能看到此门打开。

## 自定义场景状态接口

您可以通过声明自定义接口来定义状态对象的类型。这样做是可选的，但我们建议使用它，特别是对于复杂的场景，因为它有助于验证输入并更容易调试。

下面的示例为场景状态定义了一个自定义接口，并将其传递给 scriptable scene 对象。

{% raw %}

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

{% endraw %}

`ScriptableScene` 类可选地接受两个参数：属性（在这种情况下为 `any`，因为没有使用）和场景状态，在这种情况下必须匹配自定义接口中描述的类型 `IState`。

## 取得状态值

您可以从场景对象中的任何位置引用状态变量的值。

#### 本地场景中

在 _本地_ 场景中，状态存储在场景对象本身中，因此您可以从 `this.state.<variable name>` 获取。

{% raw %}

```tsx
async checkDoor(){
  return this.state.isDoorClosed
}
```

{% endraw %}

在下面的示例中，`render()` 方法绘制动态场景，其中实体的位置基于状态中的变量。 一旦该变量的值发生变化，渲染的场景也会发生变化。

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

#### 远程场景中

在 _远程_ 场景中，状态存储在远程服务器中。由名为 _State.ts_ 的文件处理。通过调用 `getState()` 函数获得状态，该函数在 _State.ts_ 中定义。

{% raw %}

```tsx
async checkDoor(){
  return getState().isDoorClosed
}
```

{% endraw %}


## 设置状态

您可以使用 `setState()` 设置状态变量的值。

`setState()` 只影响由它显式调用的变量。如果场景状态中还有其他未命名的变量，则保持不变。

处理场景状态中的数组时，不能一次只更新数组中的单个元素。您必须为整个新数组的变量设置新值，包括其它未更改的元素。

每次更新场景的状态时，都会调用 `render()` 函数使用新的状态渲染场景。

> 注意：为了防止场景每次都被渲染，你可以使用 [`shouldSceneUpdate()` 函数]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %})， 这样它会只根据某些规则有条件地运行 `Render()` 函数。

#### 本地场景中

在 _local_ 场景中，状态存储在 scriptable scene 对象中，因此您可以使用 `this.setState()` 来设置，如下所示：

{% raw %}

```tsx
async buttonPressed(){
  this.setState({
    buttonState : 1,
    isDoorClosed: false
    })
}
```

{% endraw %}

#### 远程场景中

在 _远程_ 场景中，状态存储在远程服务器中。并由名为 _State.ts_ 的文件处理。您可以通过调用 _State.ts_ 中定义的 `setState()` 函数来设置状态。

{% raw %}

```tsx
async buttonPressed(){
  setState({
    buttonState : 1,
    isDoorClosed: false
    })
}
```

{% endraw %}

## 强制更新

如果全部使用 `setState()` 更改场景状态，那么渲染的场景始终会与场景状态同步。 但是，对于特殊情况，可能需要调用 `forceUdate()` 方法手动刷新场景。

 ```
 this.forceUpdate()
 ```

## 你必须避免的做法

#### 始终使用 setState() 来更改状态

重要的是，每次更改状态时，都要通过 `setState` 函数执行，不要直接设置值。否则将导致场景的生命周期出现问题。

{% raw %}

```tsx
// Wrong
this.state.buttonState = 1

// Correct for local scenes
this.setState({ buttonState: 1 })

// Correct for remote scenes
setState({ buttonState: 1 })
```

{% endraw %}

#### 避免多次调用 setState

每次调用 `setState()` 时，也会触发 `render()` 函数。 因此，多次调用 `setState()` ，会运行几次不必要的场景渲染。因为这些渲染中都是异步的，所以最后的渲染有可能来自较旧的场景状态，使得渲染的场景过时。

 {% raw %}

 ```tsx
 // Wrong
 async buttonPressed(){
   setState({ buttonState : 1 })
   setState({ isDoorClosed: false })
 }

 // Correct
 async buttonPressed(){
   setState({
     buttonState : 1,
     isDoorClosed: false
     })
 }
 ```
 
 {% endraw %}


## 从子组件引用状态

不像 React，所有组件都可以拥有自己的状态，SDK 只允许自定义场景类具有状态。在 react terms 中，场景的所有子组件都是[controlled components 受控组件](https://reactjs.org/docs/forms.html#controlled-components)，这意味着它们可以具有属性但没有自己的状态，并且它们由其父组件控制。

例如，您可以声明一个名为 `elevator` 的电梯实体，并在其中嵌入一系列子实体，并定义自己的功能来打开和关闭门并处理按钮。如果您的场景中有许多电梯，那么您的场景代码将更加高效和清洁。缺点是该子实体中的任何函数都不再能够轻松访问场景的状态。在 `elevator` 实体中，使用表达式 `this.state` 是无效的，因为 `this` 不再引用你的自定义场景类，它引用了`elevator` 实体的当前实例。为了访问存储在场景状态中的信息，您可以：
  
- 在子组件的属性中复制场景状态的值。然后电梯中的函数可以通过 `props。<propertyName>` 访问这些数据。如果场景中有多个级别的继承，例如，如果电梯的按钮是具有自己功能的自定义实体，则状态数据可以根据需要多次从父级属性向下传递给子级属性。但是这样代码可能难以阅读。

- 导入像 [Redux](https://redux.js.org/) 这样的库来创建一个可以保存状态数据并且可以从任何地方一致地引用的全局存储。如果您的场景具有多个级别的继承，这样做是理想的。