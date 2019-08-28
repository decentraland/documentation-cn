---
date: 2018-02-15
title: 屏幕用户界面
description: 学习如何在场景中创建用户 UI。用于显示与游戏相关的信息。
categories:
  - development-guide
type: Document
set: development-guide
set_order: 15
---

有几种特殊类型的组件可用于在 2D 屏幕空间中作为 UI 的一部分使用，而不是在 3D 世界空间中使用。这些组件被固定显示在玩家的屏幕上。

UI 元素仅在玩家站在场景的 LAND 地块内时才可见，因为相邻场景可能要显示自己的 UI。 当玩家点击屏幕右下角的 _close UI_ 按钮时，所有 UI 元素都会消失。

当发生某些事件时，UI 也可以被触发打开，例如，如果玩家单击特定的位置。

为了防止恶意使用 UI，任何 UI 内容不能占据玩家屏幕的 10%，因为虚假的 UI 内容可能会伪装成 metamask 或 dapper钱包，或者是 Decentraland explorer 应用程序。

默认的 Decentraland 资源管理器 UI 包括聊天窗口小部件，地图和其他元素。 这些 UI 元素始终显示在顶层，任何特定于场景的 UI 之上。 所以如果你的场景有 UI 元素占据了和这些相同的屏幕空间，它们会被遮挡。

## 添加屏幕空间 UI

要在场景中添加屏幕空间 UI，必须创建一个 `UICanvas` 组件，该组件不需要属于任何实体。 您希望玩家看到的所有可见 UI 元素都将添加为此组件的子对象。

<!--
![](/images/media/UI-basic.png)
-->

```ts
// Create screenspace component
const canvas = new UICanvas()

// Create a textShape component, setting the canvas as parent
const text = new UIText(canvas)
text.value = 'Hello world!'
```

> 注意:每个场景只能创建一个 `UICanvas`。要在不同的时间使用不同的菜单，可将它们都设为 `UICanvas` 的子对象，然后设置它们的可见性。


## UI 内容的类型

您可以向屏幕空间添加几种不同类型的 UI 元素：

- 图像：添加`UIImage`组件以显示图像。  `source` 字段指向图像的路径。

- 文本：添加 `UIText` 组件显示文本。 设置属性与 `TextShape` 组件中的属性相同。 参见[文本]({{ site.baseurl }}{% post_url /development-guide/2018-02-11-text %})。


<!--
- Buttons: Add a `UIButton` to add a clickable button. The button offers some visual feedback when players mouse over it and when they click it.
-->

- 文本输入框：添加 `UIInputText`，玩家可以使用键盘或移动设备输入文本。

- 可滚动区域：添加 `UIScrollRect` 形成内容区域。 如果内容超出矩形区域，则矩形可选会有滑块。 玩家可以拖动此滑块来查看全部内容。

## 定位

所有 UI 组件都有几个字段用以设置组件在屏幕空间上的位置。

- `hAlign` 相对于父级的垂直对齐。 可能的值：`left`，`right`，`center`。

- `vAlign` 相对于父级的水平对齐。 可能的值：`top`, `bottom`, `center`。

- `positionX`，`positionY`：组件左上角相对于父组件的位置。 默认情况下，位于其父级的左上角。 如果设置了 `hAlign` 或 `vAlign` 属性，则 `positionX` 和 `positionY` 相对于这些对齐属性的位置偏移 UI 组件。

> 提示：从顶部开始计算时，`positionY` 的数字应为负数。 示例：要定位一个组件，相对于顶部和左侧的父项有 20 像素的边距，则将 `positionX` 设置为 20，将 `positionY` 设置为-20。

- `paddingLeft`，`paddingRight`，`paddingTop`，`paddingBottom`：填充留空。 如果值为数字则以像素为单位。要将这些字段设置为父项测量值的百分比，请将值写为以 "%" 结尾的字符串，例如 `10 %`。

- `with`，`height`：设置屏幕中组件的大小。如果值为数字则以像素为单位。要将这些字段设置为父项测量值的百分比，请将值写为以 "%" 结尾的字符串，例如 `10 %`。

```ts
const canvas = new UICanvas()

const message = new UIText(canvas)
message.value = 'Close UI'
message.fontSize = 15
message.width = 120
message.height = 30
message.vAlign = 'bottom'
message.positionX = -80
```

> 注意：UI 被有意被限制成不能超过屏幕的 10％。 因此，垂直“居中”的 UI 相对于可用的 90％ 屏幕居中，而不是整个屏幕空间。

要确定 UI 元素的 z 位置，UI 使用组件的父级层次结构。 因此，如果一个组件是另一个组件的子组件，它将出现在另一个组件的前面。

## 使用父元素组织放置其他元素

某些 UI 元素可以帮助您组织放置其他元素。

<!--
![](/images/media/UI-rectangle.png)
-->

为此，您可以使用 `UIContainerStack` 和 `UIContainerRect`。

这两种形状都具有设置颜色和线条粗细的属性。

```ts
const canvas = new UICanvas()

const inventoryContainer = new UIContainerStack(canvas)
inventoryContainer.adaptWidth = true
inventoryContainer.width = '40%'
inventoryContainer.positionY = 100
inventoryContainer.positionX = 10
inventoryContainer.color = Color4.White()
inventoryContainer.hAlign = 'left'
inventoryContainer.vAlign = 'top'
inventoryContainer.stackOrientation = UIStackOrientation.VERTICAL
```

容器组件还具有以下属性：

- `adaptWidth``adapabilityHeight`：设置父组件。 如果这些设置为 true，则宽度和高度将包装子组件（加上 padding）。 如果这些是 true ，则忽略 `width` 和 `height` 值。

- `stackOrientation`：`UIContainerStack` 组件属性来设置垂直还是水平扩展。


#### 可滚动的矩形区域

您还可以将 UI 元素添加到 `UIScrollRect` 中。 如果这些矩形中的内容超过其宽度或高度，则将显示滑块，用户可以使用滑块浏览内容。

可滚动矩形可以是水平或垂直，或两者。


```ts
const canvas = new UICanvas()

const scrollableContainer = new UIScrollRect(canvas)
scrollableContainer.width = '50%'
scrollableContainer.height = '50%'
scrollableContainer.backgroundColor = Color4.Gray()
scrollableContainer.isVertical = true
scrollableContainer.onChanged = new OnChanged(() => {
  log("scrolled to ", scrollableContainer.positionY)
})
```

滚动值标准为 0 到 1。可通过 `valueX` 和 `valueY` 属性手动设置滚动值。

`onChanged` 属性允许您在滚动条的值更改时调用函数。

## 设置透明度

将 `opacity` 属性设置为小于 1，可以使 UI 元素部分透明。


```ts
const canvas = new UICanvas()

const rect = new UIContainerRect(canvas)
rect.width = '100%'
rect.height = '100%'
rect.color =  Color4.Blue()
rect.opacity = 0.5
```

设置元素的不透明度也会影响其所有子元素。 如果不希望其子项也透明，如希望背景是透明的而不是文本，则可以使用具有四个值的十六进制字符串设置颜色，其中一个是 Alpha 通道值。

## 文本

可以用 `UIText` 组件添加文本。它具有类似于 `TextShape` 组件的属性。详见[文本]({{ site.baseurl }}{% post_url /development-guide/2018-02-11-text %})。

- `value`: 要显示的字符串
- `color`: `Color4` 对于文字颜色
- `fontSize`: 字体大小
- `fontWeight`: 'normal', 'bold' or 'italic'
- `lineSpacing` : 行间距
- `lineCount`: 最多几行文字
- `textWrapping`: 自动换行
- `outlineWidth`, `outlineColor`: 加入轮廓
- `shadowBlur`, `shadowOffsetX`, `shadowOffsetY`, `shadowColor`: 为文本添加阴影


## 使用图像集中的图像

可以在单个图像文件中存储多个图像和图标。 然后，您可以根据源图像内的像素位置，像素宽度和像素高度在 UI 中显示此图像文件的矩形部分。

下面是一个图像集的示例，其中多个图标排列在一个文件中。

![](/images/media/UI-atlas.png)

`UIImage` 组件使用以下字段来裁剪原始图像的子部分：

- `sourceTop`: 选择顶部的 _y_ 坐标（以像素为单位）
- `sourceLeft`: 选区左侧的 _x_ 坐标（以像素为单位）。
- `sourceWidth`: 所选区域的宽度（以像素为单位）
- `sourceHeight`: 所选区域的高度（以像素为单位）


构造 `UIImage` 组件时，必须传递一个 `Texture` 组件作为参数。 更多信息请阅读[材质]({{ site.baseurl }}{% post_url /development-guide/2018-02-7-materials %})中的 Texture 组件。


```ts
let imageAtlas = "images/image-atlas.jpg"
let imageTexture = new Texture(imageAtlas)

const canvas = new UICanvas()

const playButton = new UIImage(canvas, imageTexture)
playButton.sourceLeft = 26
playButton.sourceTop = 128
playButton.sourceWidth = 128
playButton.sourceHeight = 128

const startButton = new UIImage(canvas, imageTexture)
startButton.sourceLeft = 183
startButton.sourceTop = 128
startButton.sourceWidth = 128
startButton.sourceHeight = 128

const exitButton = new UIImage(canvas, imageTexture)
exitButton.sourceLeft = 346
exitButton.sourceTop = 128
exitButton.sourceWidth = 128
exitButton.sourceHeight = 128

const expandButton = new UIImage(canvas, imageTexture)
expandButton.sourceLeft = 496
expandButton.sourceTop = 128
expandButton.sourceWidth = 128
expandButton.sourceHeight = 128
```

## 可单击 UI 元素

所有 UI 元素都有一个 `isPointerBlocker` 属性，用于确定是否可以单击它们。 如果此值为 false，则指针会忽略它们并响应元素后面的内容。

可点击 UI 元素还有一个 `OnClick` 属性，可以添加函数，以便在每次单击时执行。

```ts
const canvas = new UICanvas()

const clickableImage = new UIImage(canvas, new Texture('icon.png'))
clickableImage.name = 'clickable-image'
clickableImage.width = '92px'
clickableImage.height = '91px'
clickableImage.sourceWidth = 92
clickableImage.sourceHeight = 91
clickableImage.isPointerBlocker = true
clickableImage.onClick = new OnClick(() => {
  // DO SOMETHING
})
```

<!--
![](/images/media/UI-clicks.png)
-->


> 注意：如果桌面用户想要单击 UI 组件，则必须单击 `Esc` 先从视图控件中解锁光标，然后将光标移到 UI 组件上。 

> 提示：如果要在按钮上添加文本，请记住文本需要将 `isPointerBlocker` 属性设置为 `false`，否则玩家可能会单击文本而不是按钮。


## 输入文本

可以将输入框添加到 UI 用以输入文本。 添加 `UIInputText` 组件。 玩家必须先点击此框才能输入。

```ts
const canvas = new UICanvas()

const textInput = new UIInputText(canvas)
textInput.width = '80%'
textInput.height = '25px'
textInput.vAlign = 'bottom'
textInput.hAlign = 'center'
textInput.fontSize = 10
textInput.placeholder = 'Write message here'
textInput.placeholderColor = Color4.Gray()
textInput.positionY = '200px'
textInput.isPointerBlocker = true

textInput.onTextSubmit = new OnTextSubmit(x => {
  const text = new UIText(textInput)
  text.value = '<USER-ID> ' + x.text
  text.width = '100%'
  text.height = '20px'
  text.vAlign = 'top'
  text.hAlign = 'left'
})
```


以下是可设置的一些主要属性：

- `focusedBackground`：您可以更改背景颜色以指示当前选择了输入框。

- `placeholder`：设置占位符文本。

- `placeholderColor`：将占位符文本设为不同的颜色，以便区分。 通常它的颜色比输入的文本颜色浅一些。

当用户与组件交互时，您可以使用以下事件来触发代码的执行：

- `OnFocus（）`：用户点击了 UI 组件并显示了光标。
- `OnBlur（）`：用户点击了其它地方，使得光标消失。
- `OnChanged（）`：用户键入或删除改变了组件上的字符串。
- `OnTextSubmit（）`：用户输入 `Enter` 键提交该字符串。


```ts
textInput.onChanged = new OnChanged((data: { value: string }) => {
  inputTextState = data.value
})
```


<!--
In some cases, it's best to add a _submit_ button next to the input box. In this case instead of reacting when the string is changed, or to the `OnTextSubmit` event, you only react when the button is clicked.


```ts

```
-->


## 打开 UI

您可以让场景的代码在特定事件发生时使 UI 可见，例如在游戏结束时显示最终得分。

要做到这一点，只需将包含 UI 的 `UICanvas` 组件的 `visible` 属性设置为 _true_ 或 _false_。

如果 UI 是可点击的，或者具有可点击的部分，您还应该将 `isPointerBlocker` 属性设置为 _true_ 或 _false_，这样当 UI 不用时，玩家可以在游戏世界中自由点击。

下面的代码将一个立方体添加到场景中，单击时打开 UI。


```ts
const uiTrigger = new Entity()
const transform = new Transform({ position: new Vector3(5, 1, 5), scale: new Vector3(0.3, 0.3, 0.3) })
uiTrigger.addComponent(transform)

uiTrigger.addComponent(
  new OnPointerDown(() => {
	canvas.visible = true
	canvas.isPointerBlocker = true
  })
)

uiTrigger.addComponent(new BoxShape())
engine.addEntity(uiTrigger)
```

玩家可以通过单击右上角的图标来关闭 UI。 请注意，以这种方式关闭 UI 时，即使代码将它们设置为可见，它们也不会在场景中显示 UI 组件。

最好的做法是在 UI 元素上添加一个关闭按钮，这样不会影响以后其他 UI 组件的显示。

还可以在特定事件发生时自动关闭 UI，例如，当游戏新的一局开始时。

要做到这一点，只需设置包含 UI 的 `UIScreenSpace` 组件的 `visible` 属性为 _false_。

如果 UI 是可点击的，或者具有可点击的部分，您还应该将 `isPointerBlocker` 属性设置为 _false_，以不影响玩家可以在游戏世界中的点击操作。


```ts
const canvas = new UICanvas()

const close = new UIImage(canvas, new Texture('icon.png'))
close.name = 'clickable-image'
close.width = '120px'
close.height = '30px'
close.sourceWidth = 92
close.sourceHeight = 91
close.vAlign = 'bottom'
close.isPointerBlocker = true
close.onClick = new OnClick(() => {
	log('clicked on the close image')
	canvas.visible = false
	canvas.isPointerBlocker = false
})
```



<!--

## Worldspace UI

Instead of adding UI elements to a the player's screenspace, you can add the same UI elements to a fixed location of the world space. These would be seen as an in-world screen.

UIWorldSpace

-->
