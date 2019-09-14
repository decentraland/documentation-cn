---
date: 2018-02-7
title: 在代码中使用材质
description: 如何给基本形状的实体添加材质和纹理。
categories:
  - development-guide
type: Document
set: development-guide
set_order: 7
---

## 材质

通过将材质设置为组件，可以将材质应用于使用基本形状（立方体，球体，平面等）的实体。

您可以设置 `Material` 或 `BasicMaterial` 组件。每个实体只能有其中一个组件。这两个组件都有属性字段，用于配置材质的属性，添加纹理和设置纹理的映射。

不能向 _glTF_ 模型添加材质组件。 _glTF_ 模型中包括有它们自己的材质，这些材质与模型一起隐式导入到场景中。

使用自己的材质导入 3D 模型时，请记住，并非所有着色器都受到 Decentraland 引擎的支持。仅支持标准材质和 PBR（基于物理的渲染）材质。有关详细信息，请参阅[外部 3D 模型注意事项]({{ site.baseurl }}{% post_url /3d-modeling/2018-01-10-materials %})。

## 创建并使用材质

下面的示例创建一个材质，设置它的一些字段以赋予它红色和金属属性，然后将材质应用于也具有`boxShape`组件的实体。

```ts
//Create entity and assign shape
const myEntity = new Entity()
myEntity.addComponent(new BoxShape())

//Create material and configure its fields
const myMaterial = new Material()
myMaterial.albedoColor = Color3.Blue()
myMaterial.metallic = 0.9
myMaterial.roughness = 0.1

//Assign the material to the entity
myEntity.addComponent(myMaterial)
```

`BasicMatieral` 组件的 `Material` 中可配置的所有属性字段的完整列表，请参阅[组件参考](https://github.com/decentraland/ecs-reference))。

## 基本材质

可以通过 `BasicMaterial` 实体定义材质，而不是使用 `Material` 组件。就可以形成无阴影且不受光影响的材质。这对于创建应始终明亮的用户界面非常有用，它还可用于为场景提供更简约的外观。

```ts
const myMaterial = new BasicMaterial()
```

> 注意：基本材质的某些属性名称与普通材质中的属性名称不同。如，它使用 `texture` 而不是 `albedoTexture`。

## 材质颜色

为了给材质颜色。在 `BasicMaterial` 组件中，可以设置 `color` 字段。在 `Material` 组件中，设置 `albedoColor` 字段就可以。Albedo Color 响应光线照射，并且可以包含阴影。

所有颜色字段都是 `Color3` 类型，它们包含三个值，分别为 _Red_，_Green_ 和 _Blue_。这些数字中的每一个都在 0 和 1 之间。

```ts
myMaterial.albedoColor = new Color3(0.5, 0, 0.5)
```

您还可以使用 `Color3` 对象的以下功能选择预定的颜色：

```ts
let red = Color3.Red()

let green = Color3.Green()

let blue = Color3.Blue()

let black = Color3.Black()

let white = Color3.White()

let purple = Color3.Purple()

let magenta = Color3.Magenta()

let yellow = Color3.Yellow()

let gray = Color3.Gray()

let teal = Color3.Teal()
```

您可以使用以下功能选择一个随机颜色：

```ts
// Pick a random color
//选择一种随机颜色
let green = Color3.Random()
```

如果更喜欢使用十六进制值指定颜色，就像在 JavaScript Web 开发中经常做的那样，可以使用 `.FromHexString()` 函数来完成

```ts
let gray = Color3.FromHexString("#CCCCCCC")
```

`Color3` 对象还包括许多其他函数，用于颜色的添加，相减，比较，删除或转换颜色的格式。

您还可以在 `Material` 组件中编辑以下字段属性，以微调其颜色的执行方式：

- _emissiveColor_: 从材质发出的颜色.
- _ambientColor_: 环境色又称为漫射色.
- _reflectionColor_: 从材质反射的颜色.
- _reflectivityColor_: 反射颜色，又称为“镜面颜色”.

#### 颜色渐变

可以使用 `.Lerp()` 函数，在两种颜色之间使用线性插值逐渐改变颜色。

```ts
// This variable will store the ratio between both colors
let colorRatio = 0

// Define colors
const red = Color3.Red()
const yellow = Color3.Yellow()

// Create material
const myMaterial = new Material()

// This system changes the value of colorRatio every frame, and sets a new color on the material
export class ColorSystem implements ISystem {
  update(dt: number) {
    myMaterial.albedoColor = Color3.Lerp(red, yellow, colorRatio)
    if (colorRatio < 1) {
      colorRatio += 0.01
    }
  }
}

// Add the system to the engine
engine.addSystem(ColorSystem)
```

上面的示例将材质的颜色从红色更改为黄色，并在每帧上逐步变化。

## 使用纹理

通过创建 `Texture` 组件将图像文件设为纹理。 然后，您可以在 `Material` 和 `BasicMaterial` 组件的字段中使用此纹理组件。

在 `Material` 组件中，可以设置 `albedoTexture` 设置为纹理图像。 Albedo 纹理会对环境光线作出反应，并且可以包含阴影。

```ts
//Create entity and assign shape
const myEntity = new Entity()
myEntity.addComponent(new BoxShape())

//Create texture
const myTexture = new Texture("materials/wood.png")

//Create a material
const myMaterial = new Material()
myMaterial.albedoTexture = myTexture

//Assign the material to the entity
myEntity.addComponent(myMaterial)
```

在创建纹理时，还可以设置其他参数：

- `hasAlpha`：允许纹理具有透明区域
- `samplingMode`：确定渲染时纹理中像素的拉伸或压缩方式
- `wrap`：确定纹理如何平铺到对象上（CLAMP，WRAP 或 MIRROR）

```ts
let smokeTexture = new Texture('textures/smoke-puff3.png',{hasAlpha: true, wrap:CLAMP})
```

#### basic textures 上的纹理

在 `BasicMaterial` 组件中，可以将 `texture` 字段设置为图像纹理。这将渲染一个不受光照影响的纹理。

```ts
//Create entity and assign shape
const myEntity = new Entity()
myEntity.addComponent(new BoxShape())

//Create texture
const myTexture = new Texture("materials/wood.png")

//Create material and configure its fields
const myMaterial = new BasicMaterial()
myMaterial.texture = myTexture

//Assign the material to the entity
myEntity.addComponent(myMaterial)
```

#### 多层纹理

还允许您使用多个图像文件作为图层来构成更逼真的纹理，例如包括 `bumpTexture` 和 `refractionTexture`。

```ts
//Create entity and assign shape
const myEntity = new Entity()
myEntity.addComponent(new BoxShape())

//Create texture
const myTexture = new Texture("materials/wood.png")

//Create second texture
const myBumpTexture = new Texture("materials/woodBump.png")


//Create material and configure its fields
const myMaterial = new Material()
myMaterial.albedoTexture = myTexture
myMaterial.bumpTexture = myBumpTexture

//Assign the material to the entity
myEntity.addComponent(myMaterial)
```

在上面的示例中，材质的图像位于 `materials` 文件夹中，该文件夹位于场景项目文件夹根目录下。

> 提示：我们建议将纹理图像文件保存在场景内的 `/materials` 文件夹中。
> 提示：材质可以有多层纹理，可以在代码编辑器中单击 `.` 查看自动完成菜单显示的纹理列表。

#### 纹理映射

如果您希望将纹理映射到实体上的特定比例或对齐，则需要在[形状组件]({{ site.baseurl }}{% post_url /development-guide/2018-02-6-shape-components %})上配置 _uv_。

`Texture` 组件可以通过设置 `wrap` 字段来配置映射模式。模式可以是 `CLAMP`, `WRAP` 或 `MIRROR`。

```ts
myTexture.wrap = 3
```

上面的例子将映射模式设置为 `MIRROR`。

- `CLAMP`：纹理仅以指定大小显示一次。 网格表面的其余部分将保持透明。
- `WRAP`：在指定的大小中，纹理将重复铺满网格。
- `MIRROR`：映射中，纹理在网格会重复多次，但是这些重复的方向会被镜像

要手动处理纹理映射，请在纹理的 2D 图像上设置 _u_ 和 _v_ 坐标，以对应于形状的顶点。实体具有的顶点越多，需要在纹理上定义的 _uv_ 坐标越多，例如，plane 需要定义8个 _uv_ 点，其两个面中的每一个都需要 4 个。

```ts
//Create material and configure fields
const myMaterial = new BasicMaterial()
myMaterial.texture = "materials/atlas.png"
myMaterial.samplingMode = 0

//Create shape component
const plane = new PlaneShape()
plane.uvs = [
  0,
  0.75,
  0.25,
  0.75,
  0.25,
  1,
  0,
  1,
  0,
  0.75,
  0.25,
  0.75,
  0.25,
  1,
  0,
  1
]

//Create entity and assign shape and material
const myEntity = new Entity()
myEntity.addComponent(plane)
myEntity.addComponent(myMaterial)
```

<!--
Use the [Decentraland sprite helpers](https://github.com/decentraland/dcl-sprites) library to map textures easily. Read documentation on how to use this library in the provided link.

To create an animated sprite, use texture mapping to change the selected regions of a same texture that holds all the frames.要创建动画精灵，请使用纹理映射来更改包含所有帧的相同纹理的选定区域。
-->

#### 纹理拉伸

当纹理被拉伸或缩小到与原始纹理图像不同的大小时，有时会产生伪影。存在各种[纹理过滤](https://en.wikipedia.org/wiki/Texture_filtering)算法以便以不同方式对此进行补偿。 `BasicMaterial` 组件默认使用 _bilinear_ 算法，但您可以通过设置`samplingMode` 将其配置为使用 _nearest neighbor_ 或 _trilinear_ 算法。

```tsx
const myMaterial = new BasicMaterial()
myMaterial.samplingMode = 1
```

上面的示例使用邻近算法。此设置非常适合像素艺术风格的图形，因为纹理在屏幕上显得较大而不模糊时，轮廓仍将保持清晰显示。

## 透明材质

要使材质透明，必须将 Alpha 通道添加到用于纹理的图像中。 _material_ 组件默认忽略纹理图像的 Alpha 通道，因此您必须：

- 设置 `hasAlpha` 为 true。
- 在 `alphaTexture` 中设置一个图像。此图像可以与纹理相同，也可以是不同的图像。

```ts
const myMaterial = new Material()
myMaterial.hasAlpha = true
// or
//Create texture
const myTexture = new Texture("materials/alpha.png")

const myMaterial2 = new Material()
myMaterial2.alphaTexture = myTexture
```

## 重用材质

如果场景中的多个实体使用相同的材​​质，则无需为每个实体创建材质组件的实例。所有实体都可以共享一个相同的实例，这可以使您的场景更轻盈，以防止超出每个场景的最大材质数量。

```ts
//Create entities and assign shapes
const box = new BoxShape()
const myEntity = new Entity()
myEntity.addComponent(box)
const mySecondEntity = new Entity()
mySecondEntity.addComponent(box)
const myThirdEntity = new Entity()
myThirdEntity.addComponent(box)

//Create material and configure fields
const myMaterial = new Material()
myMaterial.albedoColor = Color3.Blue()

//Assign same material to all entities
myEntity.addComponent(myMaterial)
mySecondEntity.addComponent(myMaterial)
myThirdEntity.addComponent(myMaterial)
```