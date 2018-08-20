---
date: 2018-01-08
title: TypeScript 编码指南
description: 场景开发技巧
categories:
  - documentation
type: Document
set: sdk-reference
set_order: 8
---

Decentraland SDK 通过 TypeScript（.tsx）文件使用。本节介绍了构建场景时可以利用的一些提示和技巧。 这里讨论的内容与 SDK 的功能没有直接关系，而是有关如何使用 TypeScript 语言和如何尽量摆脱上下文相关。

## 控制台输出

您可以在查看场景时将消息输出到浏览器的 JavaScript 控制台。

执行此操作您不需要导入任何其他库，只需在 scene.tsx 文件的任何部分中使用 `console.log()`。 例如，您可以使用日志消息来确认事件的发生，或者验证变量的值是否按预期方式更改。

{% raw %}

```tsx
this.subscribeTo("pointerDown", e => {
  console.log("click")
})
```

{% endraw %}

在运行场景预览时要查看记录的消息，请查看 JavaScript 控制台，您可以在浏览器的开发人员设置中打开该控制台。

![](/images/media/console_log.png)

## 创建全局常量

您可以在_.tsx_文件的根级别定义全局常量。定义后，就可以在整个文件中引用它。

这对于在场景中多次使用且需要一致性的值非常有用。这样可以更轻松地维护代码，因为您只需要更改一行。

{% raw %}

```tsx
import { createElement, ScriptableScene } from "metaverse-api"

const updateRate = 300
const myColors = [
  "#3d9693",
  "#e8daa0",
  "#968fb7",
  "#966161",
  "#879e91",
  "#66656b",
  "#6699cc"
]

export default class myScene extends ScriptableScene {
  state = {
    interval: updateRate,
    boxColor: myColors[1],
    doorColor: myColors[4]
  }

  // (...)
}
```

{% endraw %}

## 定义自定义数据类型

您可以在_.tsx_文件中定义自己的自定义数据类型，这些对于使用仅能够在场景中保存特定值的变量非常有用。使用自定义类型可使代码更具可读性。如果您正在使用Visual Studio Code 或 Atom 等高级代码编辑器，由于代码编辑器提供了自动完成选项和验证，因此它还可以使代码编写更容易。

{% raw %}

```tsx
export type characterState = "walking" | "won" | "falling"
```

{% endraw %}

上面定义的自定义类型只能包含上面列出的三个可能值。然后，您可以将其用于场景状态中的变量。

{% raw %}

```tsx
state = {
  characterNow: (characterState = "walking")
}
```

{% endraw %}

为此变量设置值时，代码编辑器只会建议其类型的有效值。

![](/images/media/autocomplete_types.png)

<!---

## Define a custom enum  ???

With TypeScript, you can create custom string enums. These can be thought of as dictionaries that can only contain specific string fields.

https://blog.decentraland.org/building-a-memory-game-using-decentralands-sdk-87ee35968f8d



{% raw %}
```tsx
export enum characterStates {

}
```
{% endraw %}

[definition]

You can then set this type as the type of a variable

[scene state definition that sets this as a type]

When using that variable, the code editor then
[screenshot]
-->

## 场景状态接口

场景状态对象可以定义为它自己的类型。 这可以确保状态对象始终具有正确的变量，并且它们都具有相应类型的有效值。如果您正在使用 Visual Studio Code 或 Atom 等高级代码编辑器，则定义状态接口有助于编辑器提供类型验证和智能自动完成。

{% raw %}

```tsx
interface IState {
  doorState: boolean
  boxes: number
  userPos: Vector3Component
}
```

{% endraw %}

定义接口后，可以将其作为第二个参数传递给自定义场景类，该参数设置场景状态对象的类型。

{% raw %}

```tsx
export default class ArtPiece extends ScriptableScene<any, IState> {
  state = {
    pedestalColor: "#3d30ec",
    dogAngle: 0,
    donutAngle: 0
  }

  // (...)
}
```

{% endraw %}

<!---

## Advanced 3D math operations

You can import javascript libraries to enable you to perform mathematical operations that the SDK doesn’t cover. For example, for vector operations, you can import

  babylon's library. only

## Accessing variables globally

When having the code for your scene distributed amongst multiple separate files with child objects, you need to take care of how to reference



-->

## 执行时间控制


TypeScript 提供了各种方法，用来控制什么时候执行代码。

scriptableScene 对象带有许多默认函数，这些函数在场景生命周期的不同时间执行，例如`sceneDidMount()`在场景开始时调用一次，每次场景状态改变时调用`render()` 。 有关详细信息，请参阅[scriptable scene]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-05-scriptable-scene %})。

实体可以包含_transition_组件以使任何更改逐渐发生，这非常类似于 CSS 中的过渡。 有关详细信息，请参阅[场景内容指南]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-21-scene-content-guide %})。

#### 启动基于时间的循环

`setInterval()` 函数启动一个循环，该循环以设定的间隔重复执行函数。

{% raw %}

```tsx
setInterval(() => {
  this.setState({ randomNumber: Math.random() })
}, 1000)
```

{% endraw %}

此示例启动一个循环，该循环将场景状态中的 `randomNumber` 变量设置为每 1000 毫秒设置为一个新的随机数。

#### 结束循环

`setInterval()` 函数返回循环 id，你可以通过运行 `clearInterval()` 函数来终止循环的执行，以循环 id 作为参数传递。

{% raw %}

```tsx
  let count = 0
  const loopId = setInterval(() => {
    count += 1
    console.log(count)
    if (count === 5) {
      clearInterval(loopId)
    }
  }
```

{% endraw %}

这个例子遍历循环直到满足条件，在这种情况下调用 `clearInterval()` 来停止循环。

#### 延迟执行

`setTimeout()` 函数延迟语句或函数的执行。

{% raw %}

```tsx
setTimeout(f => {
  console.log("you'll have to wait for this message")
}, 3000)
```

{% endraw %}

setTimeout 函数要求您传递一个函数或语句来执行，然后是延迟执行的毫秒数。

#### 停止运行直到完成

在语句的开头添加 `await` 会停止当前线程的所有执行，直到该语句返回一个值。

{% raw %}

```tsx
await this.runImportantProcess()
```

{% endraw %}

在这个例子中，线程的执行被延迟，直到函数 `runImportantProcess()` 返回一个值。

当需要存储 return 语句的值时，可以象这样将 `await` 放在等号后：

{% raw %}

```tsx
const importantValue = await this.runImportantProcess()
```

{% endraw %}

`await` 只能在异步函数的上下文中使用，否则会冻结场景执行的主线程，这是绝对不可取的。

如果您熟悉C＃语言，您会发现 TypeScript 中的异步函数跟它一致。默认情况下，函数同步运行，但您可以通过在定义名称前添加 `async` 来使它们异步运行。

> 提示：如果您想了解 JavaScript promises, async and await 背后的原因，我们建议您阅读[这篇文章](https://zeit.co/blog/async-and-await)。

## 处理场景状态中的数组

在处理属于 Decentraland 场景的场景状态的数组时，需要考虑许多事项。

因为你必须总是通过 `.setState()` 方法更新场景状态，所以你不能只使用像 `.push()` 或 `pop()` 这样的数组方法来直接改变这个变量。您必须调用`setState()` 并传入实现更改后的完整数组。

如果您需要复制一个要修改的数组，请确保完全克隆它，而不仅仅是引用它的值。否则更改该数组也会影响原始数组。要复制数组的值而不是数组本身，请使用_展开运算符_（三个点）。

{% raw %}

```tsx
const newArray = [...this.state.myArray]
```

{% endraw %}

#### 加入数组

在数组末尾添加新元素：


{% raw %}

```tsx
this.setState({ myArray: [...this.state.myArray, newValue] })
```

{% endraw %}

在数组的开头添加新元素：

{% raw %}

```tsx
this.setState({ myArray: [newValue, ...myArray.state.list] })
```

{% endraw %}

#### 更新数组上的元素

改变 `valueIndex` 处元素的值：

{% raw %}

```tsx
this.setState({
  myArray: [
    ...this.state.myArray.slice(0, valueIndex),
    newValue,
    ...this.state.myArray.slice(valueIndex + 1)
  ]
})
```

{% endraw %}

#### 从数组中删除

此示例移出数组的第一个元素，所有其他元素相应移动以填充空位。（例子中先复制数组除第一个元素的值，然后用此值设置场景状态）

{% raw %}

```tsx
const [_, ...rest] = this.state.list
this.setState({ list: [...rest] })
```

{% endraw %}

此例删除数组的最后一个元素：

{% raw %}

```tsx
const [...rest, _] = this.state.list
this.setState({ list: [...rest] })
```

{% endraw %}

此示例删除所有符合特定条件的元素。这里是删除所有 `id` 等于 `toRemove` 的元素。

{% raw %}

```tsx
this.setState({ myArray: ...myArray.state.list.filter(x => x.id === toRemove) })
```

{% endraw %}

#### map 操作

有两种数组方法可以分别在数组的每个元素上运行同一个函数：`map` 和 `forEach`。 它们之间的主要区别在于 `map` 返回一个新数组而不影响原始数组，但 `forEach` 会覆盖原始数组中的值。

`map` 操作在数组的每个元素上运行一个函数，它返回一个包含结果的新数组。

{% raw %}

```tsx
renderLeaves(){
  return this.state.fallingLeaves.map((leaf, leafIndex) =>
    <plane
      position={{ x: leaf.x , y: leaf.y, z:leaf.z }}
      scale={0.2}
      key={leafIndex.toString()}
    />
  )
}
```

{% endraw %}

这个例子遍历 `fallingLeaves` 数组中的元素，并在每个元素上运行同一个函数。原始数组的类型是 `Vector3Component`，因此其中的每个元素都具有 _x_，_y_ 和 _z_ 坐标的值。每个元素运行函数后返回一个平面实体，该实体使用存储在数组中的位置，并具有基于数组索引的键。

#### 与 filter 结合使用

您可以将 `map` 或 `forEach` 操作与 `filter` 操作组合，来处理仅满足特定条件的数组元素。

{% raw %}

```tsx
renderLeaves(){
  return this.state.fallingLeaves
    .filter(pos => pos.x > 0)
    .map( (leaf, leafIndex) =>
      <plane
        position={{ x: leaf.x , y: leaf.y, z: leaf.z }}
        scale={0.2}
        key={leafIndex.toString()}
      />
    )
}
```

{% endraw %}

这个例子与上面的例子类似，但它首先过滤 `fallingLeaves` 数组，只处理 _x_ 位置大于 0 的叶子。`fallingLeaves` 数组的类型为 `Vector3Component`，因此数组中的每个元素都 _x_，_y_ 和 _z_ 坐标。

#### forEach 操作

`forEach` 在数组的每个元素上运行相同的函数。

{% raw %}

```tsx
renderLeaves() {
  var leaves: ISimplifiedNode[] = []

  this.state.fallingLeaves.forEach( (leaf,leafIndex) => {
    leaves.push(
      <plane
        position={{ x: leaf.x, y: leaf.y, z: leaf.z }}
        scale={0.2}
        key={leafIndex.toString()}
      />
    )
  })

  return leaves
}
```

{% endraw %}

与上面用于解释 map 运算符的示例一样，此示例将遍历 `fallingLeaves` 数组的元素并运行相同的函数。原始数组的类型为 `Vector3Component`，因此其中的每个元素都具有 _x_，_y_ 和 _z_ 坐标的值。遍历数组每个元素并运行的函数返回一个使用保存在数据中的位置的平面实体。

`forEach()` 函数执行后没有使用 `return` 语句。 如果有的话，它会覆盖`this.state.fallingLeaves` 数组的内容。相反，我们创建一个名为 `leaves` 的新数组并将元素加入，然后我们返回最后的完整数组。

> 注意：请记住，在处理场景状态中的变量时，您无法通过直接设置来更改其值。您必须始终通过`this.setState()` 操作更改场景状态变量的值。

## 动态化 render 函数

`render()` 函数绘制用户在场景中看到的内容。在其最简单的形式中，它的`return`语句包含类似于具有固定值的一组实体的文字 XML 定义。让场景交互的一个重要部分是使渲染函数改变其输出以响应场景状态的变化。

虽然 render() 的`return`语句中的典型内容可能类似于纯 XML，但`{}`之间的所有内容 使用 TypeScript 处理。这意味着您可以使用大括号中断 XML 的标记和属性语法，以便在您选择的任何位置添加 JSX 逻辑。

#### render 中变量的引用

更改某些内容呈现方式的最简单方法是从其中一个 XML 属性的值引用变量的值。

{% raw %}

```tsx
async render() {
  return (
    <scene>
      <box
        color= {this.state.boxColor}
        scale={this.state.boxSize}
      />
    </scene>
  )
}
```

{% endraw %}

#### 在 render 中添加条件逻辑

使 render() 响应变量变化的另一种简单方法是添加条件逻辑。

{% raw %}

```tsx
async render() {
  return (
    <scene>
      {this.state.boxOrSphere == sphere
        ?<sphere />
        :<box />
      }
    </scene>
  )
}
```

{% endraw %}

在上面的示例中，render 函数返回是一个箱子还是一个球体，具体取决于 `boxOrSphere` 变量的值。请注意，我们需要将整个条件表达式包装在 `{}` 中，以便将其作为 TypeScript 处理。

{% raw %}

```tsx
async render() {
  return (
    <scene>
      <box
          position={{x: 2, y: this.state.liftBox ? 5:0 , z:1}}
          transition={{ position:
            { duration: 300, timing: this.state.bounce? "bounce-in" : "linear" }
          }}
      />  
    </scene>
  )
}
```

{% endraw %}

在第二个例子中，箱子的 _y_ 位置是根据 `liftBox` 的值确定的，其转换的时间是基于`bounce` 的值。 请注意，这两个条件表达式都添加在已经作为 TypeScript 处理的代码的部分中，因此不需要附加 `{ }`。

#### 定义未确定数量的实体

对于实体数量不固定的场景，可以使用数组表示这些实体及其属性，然后在 `render()` 函数中使用`map`操作。

{% raw %}

```tsx
async render() {
  return (
    <scene>
      { this.state.secuence.map(num =>
        <box
          position={{ x: num * 2, y: 1, z: 1 }}
        />
      }
    </scene>
  )
}
```

{% endraw %}

此函数使用 `map` 为`secuence` 数组中的每个元素创建一个箱子实体，并使用此数组中存储的数字来设置每个这些箱子的 x 坐标。这使您可以通过更改场景状态中的 `secuence` 变量来动态更改出现的箱子数和位置。

#### 保持 render 函数可读

render() 函数的输出可以包括对其他函数的调用。 由于每次更新场景状态时都会调用render()，因此这里的所有函数也会被 render() 调用。

这样做可以使 render() 中的代码更具可读性。在简单的场景中，推荐在 `render()` 函数中定义场景的所有实体，但是当处理不同数量的实体或可以被视为数组的大量实体时，通常放在 render() 函数外处理。

{% raw %}

```tsx
async render() {
  return (
    <scene>
      {this.renderObstacles()}
      {this.renderFruit()}
      {this.status.difficulty === 'hard' ?
        this.renderHardEnemies()
        : this.renderEasyEnemies()
      }
    </scene>
  )
}
```

{% endraw %}

当然，作为返回声明的一部分调用的函数必须返回与正在呈现的内容的其余部分完美组合，以生成有效 XML 输出的值。在上面的例子中，`renderObstacles()` 函数可以包含以下内容：

{% raw %}

```tsx
renderObstacles() {
  return this.state.secuence.map(num =>
    <box
      position={{ x: num * 2, y: 1, z: 1 }}
    />
  )
}
```

{% endraw %}
