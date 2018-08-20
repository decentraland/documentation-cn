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

The state should contain **only** data, and no logic or methods. We don't recommend assigning instances of objects that have methods of their own to variables in the state. All of the scene's logic should be carried out in your custom class that extends the [scriptable scene]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %}) class.

状态应该只 **包含** 数据，没有逻辑或方法。我们不建议将具有自己方法的对象实例分配给状态中的变量。所有场景的逻辑都应该在您的自定义类中执行，该类扩展了 [scriptable scene]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %}) 类。

Each state variable must be given an intial value for when the scene is first rendered.
首次渲染场景时，必须为每个状态变量赋予初始值。

The variables you choose to make up your scene's state should be the minimal possible set that represents the scene, you shouldn't add redundant information. To determine if you should include a piece of information as part of the state, ask yourself:

构成场景状态的变量应该是代表场景的最小可能集合，不应该保存冗余信息。要确定是否应将某数据作为状态的一部分，问一下自己下面几个问题：

- Does this information change over time? If it doesn't, it should probably be declared as a constant instead of as the state.
- Is this information passed in when instantiating the scriptable scene class? If so, it probably should be part of `Props` rather than of the state.
- Can you easily compute this information based on other state variables or the props? If so, you should consider computing it each time instead of storing it in the state. Keep in mind that this has an impact on the scene performance, so in some cases it might be better to store more information in the scene state.

- 此信息会随时间而变化吗？如果没有，它应该声明为常量而不是状态。
- 在实例化场景类时是否传递了此信息，如果是这样，它可能应该是 `Props` 而不是状态的一部分。
- 你是否可以通过计算其他状态变量或属性就可以得到这些信息吗？如果是，您应该考虑每次计算它而不是将其存储在状态中。 请记住，这会对场景性能产生影响，因此在某些情况下，最好在场景状态下存储更多信息。

## Local and remote scenes
## 本地和远程场景

**Local scenes** store the scene state locally as part of the scriptable scene object. For example, when you create scenes with the Decentraland CLI, the default scenes for the options _Basic_ and _Interactive_ are both local scenes.

**本地场景**将场景状态存储在本地，作为 `ScriptableScene` 对象的一部分。 例如，使用 Decentraland CLI 创建场景时， _Basic_ 和 _Interactive_ 选项的默认场景都是本地场景。

{% raw %}

```tsx
export default class Scene extends ScriptableScene {
  state = {
    buttonState: 0,
    isDoorClosed: false,
    queboxPosition: { x: 0, y: 0, z: 0 }
  }

  // (...)
}
```

{% endraw %}

Local scenes run entirely in the user's local client. When the value of a variable in the scene state changes, it only changes the local representation in the client, and isn't propagated to other users of the scene. That means each user might be having a different perception of what the scene looks like, even though the users might see each other moving around.

本地场景完全在用户的本地客户端中运行。当场景状态中的变量值发生更改时，它仅更改客户端中的本地表示，并且不会传播到场景的其他用户。这意味着每个用户可能对场景的外观有不同的感知，即使用户可能看到彼此移动。

For example, if a user opens a door, other users won't see this door open, because users can only affect their own local representation of the scene state.

例如，如果用户打开门，其他用户将不会看到此门打开，因为用户只能影响他们自己的本地场景状态。

**Remote scenes** store the scene state in a remote server. The local client retrieves information from this server to render the scene lcoally. When the value of a variable in the scene state is changed, this change is pushed to the remote server to update its representation of the scene state. All users of the scene will then render their scenes locally based on the scene state that's stored in the remote server, so all users will see the change.

**远程场景**将场景状态存储在远程服务器中。本地客户端从此服务器检索信息以便在场景中呈现场景。当场景状态中的变量值更改时，此更改将被推送到远程服务器以更新其场景状态的表示。 然后，场景的所有用户将根据存储在远程服务器中的场景状态在本地渲染其场景，因此所有用户都将看到更改。

Referring back to the previous example: in a remote scene, if a user opens a door, all other users should see this door open as well.

回到前面的例子：在远程场景中，如果用户打开门，所有其他用户也应该能看到此门打开。

## Define an interface for the scene state
## 自定义场景状态接口

You can define the type for the state object by declaring a custom interface. Doing this is optional but we recommend it, especially for complex scenes, as it helps validate inputs and makes debugging easier.

您可以通过声明自定义接口来定义状态对象的类型。这样做是可选的，但我们建议使用它，特别是对于复杂的场景，因为它有助于验证输入并更容易调试。

The example bleow defines a custom interface for the scene state and passes it to the scriptable scene object.

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

The `ScriptableScene` class optionally takes two arguments: the properties (`any` in this case, as none are used) and the scene state, which in this case must match the type `IState`, described in the custom interface.

`ScriptableScene` 类可选地接受两个参数：属性（在这种情况下为 `any`，因为没有使用）和场景状态，在这种情况下必须匹配自定义接口中描述的类型 `IState`。

## Get the state
## 取得状态值

You can reference the value a state variable from anywhere in the scene object.

您可以从场景对象中的任何位置引用状态变量的值。

#### In a local scene
#### 本地场景中

In a _local_ scene the state is stored in the scene object itself, so you fetch it by writing `this.state.<variable name>`.

在 _本地_ 场景中，状态存储在场景对象本身中，因此您可以从 `this.state.<variable name>` 获取。

{% raw %}

```tsx
async checkDoor(){
  return this.state.isDoorClosed
}
```

{% endraw %}

In the example below, the `render()` method draws a dynamic scene where the position of an entity is based on a variable in the state. As soon as the value of that variable changes, the rendered scene changes as well.

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

#### In a remote scene
#### 远程场景中

In a _remote_ scene the state is stored in a remote server. This is handled by a file called _State.ts_. You get the state by calling the `getState()` function, that's defined in _State.ts_.

在 _远程_ 场景中，状态存储在远程服务器中。由名为 _State.ts_ 的文件处理。通过调用 `getState()` 函数获得状态，该函数在 _State.ts_ 中定义。

{% raw %}

```tsx
async checkDoor(){
  return getState().isDoorClosed
}
```

{% endraw %}


## 设置状态

You can set the value of a state variable by using `setState()`.

您可以使用 `setState()` 设置状态变量的值。

`setState()` only affects the variables that are explicitly called out by it. If there are other variables in the scene state that aren't named, these are left untouched.

`setState()` 只影响由它显式调用的变量。如果场景状态中还有其他未命名的变量，则保持不变。

When dealing with arrays in the scene state, you can't update a single element in the array at a time. You must set a new value for the variable consiting of an entire new array, including any elements that haven't changed.

处理场景状态中的数组时，不能一次只更新数组中的单个元素。您必须为整个新数组的变量设置新值，包括其它未更改的元素。

Each time the scene's state is updated, the `render()` function is called to render the scene using the new state.

每次更新场景的状态时，都会调用 `render()` 函数使用新的状态渲染场景。

> Note: To prevent the scene from being rendered every time, you can use the [`shouldSceneUpdate()` function]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %}) so that it only runs the `Render()` function conditionally based on some rule.

> 注意：为了防止场景每次都被渲染，你可以使用 [`shouldSceneUpdate()` 函数]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %})， 这样它会只根据某些规则有条件地运行 `Render()` 函数。


#### In a local scene
#### 本地场景中

In a _local_ scene the state is stored in the scriptable scene object, so you set it with `this.setState()` as shown below:

在 _local_ 场景中，状态存储在 scriptable scene 对象中，因此您可以使用 `this.setState()` 来设置，如下所示：

```tsx
async buttonPressed(){
  this.setState({
    buttonState : 1,
    isDoorClosed: false
    })
}
```

#### In a remote scene
#### 远程场景中

In a _remote_ scene the state is stored in a remote server. This is handled by a file called _State.ts_. You set the state by calling the `setState()` function, that's defined in _State.ts_.

在 _远程_ 场景中，状态存储在远程服务器中。并由名为 _State.ts_ 的文件处理。您可以通过调用 _State.ts_ 中定义的 `setState()` 函数来设置状态。

```tsx
async buttonPressed(){
  setState({
    buttonState : 1,
    isDoorClosed: false
    })
}
```

#### Always use setState()
#### 始终使用 setState()

It's important that each time you change the state you do it through the `setState` function, NEVER do it by directly setting a value. Otherwise this will cause problems with the lifecycle of the scene.

重要的是，每次更改状态时，都要通过 `setState` 函数执行，不要直接设置值。否则将导致场景的生命周期出现问题。

```tsx
// Wrong
this.state.buttonState = 1

// Correct for local scenes
this.setState({ buttonState: 1 })

// Correct for remote scenes
setState({ buttonState: 1 })
```

## 强制更新

If you always change the scene state through `setState()`, the rendering of your scene should always be in sync with the scene state. However, for exceptional cases you might need to refresh the rendering of the scene manually. To do this, call the `forceUdate()` method.

如果始终通过 `setState()` 更改场景状态，则场景的渲染始终与场景状态同步。 但是，对于特殊情况，您可能需要手动刷新场景的渲染。为此，请调用 `forceUdate()` 方法。

```
this.forceUpdate()
```


## 从子组件引用状态

不像 React，所有组件都可以拥有自己的状态，SDK 只允许自定义场景类具有状态。在 react terms 中，场景的所有子组件都是[controlled components 受控组件](https://reactjs.org/docs/forms.html#controlled-components)，这意味着它们可以具有属性但没有自己的状态，并且它们由其父组件控制。

例如，您可以声明一个名为 `elevator` 的电梯实体，并在其中嵌入一系列子实体，并定义自己的功能来打开和关闭门并处理按钮。如果您的场景中有许多电梯，那么您的场景代码将更加高效和清洁。缺点是该子实体中的任何函数都不再能够轻松访问场景的状态。在 `elevator` 实体中，使用表达式 `this.state` 是无效的，因为 `this` 不再引用你的自定义场景类，它引用了`elevator` 实体的当前实例。为了访问存储在场景状态中的信息，您可以：
  
- 在子组件的属性中复制场景状态的值。然后电梯中的函数可以通过 `props。<propertyName>` 访问这些数据。如果场景中有多个级别的继承，例如，如果电梯的按钮是具有自己功能的自定义实体，则状态数据可以根据需要多次从父级属性向下传递给子级属性。但是这样代码可能难以阅读。

- 导入像 [Redux](https://redux.js.org/) 这样的库来创建一个可以保存状态数据并且可以从任何地方一致地引用的全局存储。如果您的场景具有多个级别的继承，这样做是理想的。