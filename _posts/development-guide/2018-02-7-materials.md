---
date: 2018-02-7
title: 材质
description: 如何给基本形状的实体添加材质和纹理。
categories:
  - development-guide
type: Document
set: development-guide
set_order: 7
---

## Materials
## 材质

Materials can be applied to entities that use primitive shapes (cube, sphere, plane, etc) by setting them as a component.

You can either set a `Material` or a `BasicMaterial` component. Each entity can only have one of these. Both components have several fields that allow you to configure the properties of the material, add a texture and set the texture's mapping.

通过将材质设置为组件，可以将材质应用于使用基本形状（立方体，球体，平面等）的实体。

您可以设置`Material`或`BasicMaterial`组件。每个实体只能拥有其中一个。这两个组件都有几个字段，允许您配置材质的属性，添加纹理和设置纹理的映射。

You can't add material components to _glTF_ models. _glTF_ models include their own materials that are implicitly imported into a scene together with the model.

When importing a 3D model with its own materials, keep in mind that not all shaders are supported by the Decentraland engine. Only standard materials and PBR (physically based rendering) materials are supported. See [external 3D model considerations]({{ site.baseurl }}{% post_url /development-guide/2018-01-09-external-3d-models %}#materials) for more details.

您无法向_glTF_模型添加材质组件。 _glTF_模型包括它们自己的材料，这些材料与模型一起隐式导入到场景中。

使用自己的材质导入3D模型时，请记住，并非所有着色器都受到Decentraland引擎的支持。仅支持标准材料和PBR（基于物理的渲染）材料。有关详细信息，请参阅[外部3D模型注意事项]（{{site.baseurl}} {％post_url / development-guide / 2018-01-09-external-3d-models％}＃materials）。

## Create and apply a material
##创建并应用材料

The following example creates a material, sets some of its fields to give it a red color and metallic properties, and then applies the material to an entity that also has a `boxShape` component.

下面的示例创建一个材质，设置它的一些字段以赋予它红色和金属属性，然后将材质应用于也具有`boxShape`组件的实体。

```ts
//Create entity and assign shape
const myEntity = new Entity()
myEntity.set(new BoxShape())

//Create material and configure its fields
const myMaterial = new Material()
myMaterial.albedoColor = Color3.Blue()
myMaterial.metallic = 0.9
myMaterial.roughness = 0.1

//Assign the material to the entity
myEntity.set(myMaterial)
```

See [component reference](https://github.com/decentraland/ecs-reference)) for a full list of all the fields that can be configured in a `Material` of `BasicMatieral` component.

有关可在“BasicMatieral”组件的“Material”中配置的所有字段的完整列表，请参阅[组件参考]（https://github.com/decentraland/ecs-reference））。

## Basic materials
##基础材料

Instead of the `Material` component, you can define a material through the `BasicMaterial` entity. This creates materials that are shadeless and are not affected by light. This is useful for creating user interfaces that should be consistently bright, it can also be used to give your scene a more minimalist look.

您可以通过`BasicMaterial`实体定义材料，而不是`Material`组件。这样就可以制成无阴影且不受光影响的材料。这对于创建应始终明亮的用户界面非常有用，它还可用于为场景提供更简约的外观。

```tsx
const myMaterial = new BasicMaterial()
```

> Note: Basic materials have some property names that are different from those in normal materials. For example it uses `texture` instead of `albedoTexture`.

>注意：基本材料的某些属性名称与普通材料中的属性名称不同。例如，它使用`texture`而不是`albedoTexture`。

## Material colors
##材质颜色

Give a material a plain color. In a `BasicMaterial` component, you set the `color` field. In a `Material` component, you set the `albedoColor` field. Albedo colors respond to light and can include shades on them.

给材料一种纯色。在`BasicMaterial`组件中，设置`color`字段。在`Material`组件中，设置`albedoColor`字段。反照率颜色对光线有反应，并且可以包含阴影。

All color fields are of type `Color3`, these hold three values, for _Red_, _Green_ and _Blue_. Each of these numbers is between 0 and 1.

所有颜色字段都是“Color3”类型，它们包含三个值，分别为_Red_，_Green_和_Blue_。这些数字中的每一个都在0和1之间。

```ts
myMaterial.albedoColor = new Color3.(0.5, 0, 0.5)
```

You can also pick predetermined colors using the following functions of the `Color3` object:

您还可以使用“Color3”对象的以下功能选择预定的颜色：

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

You can otherwise pick a random color using the following function:
您可以使用以下功能选择随机颜色：

```ts
// Pick a random color
//选择一种随机颜色
let green = Color3.Random()
```

If you prefer to specify a color using hexadecimal values, as is often done in JavaScript web development, you can do so using the `.FromHexString()` function

如果您更喜欢使用十六进制值指定颜色，就像在JavaScript Web开发中经常做的那样，您可以使用`.FromHexString（）`函数来完成

```ts
let gray = Color3.FromHexString("#CCCCCCC")
```

The `Color3` object also includes a lot of other functions to add, substract, compare, lerp, or convert the format of colors.

`Color 3d`对象还包括许多其他函数，用于添加，减去，比较，删除或转换颜色的格式。

You can also edit the following fields in a `Material` component to fine-tune how its color is percieved:

您还可以在“材质”组件中编辑以下字段，以微调其颜色的执行方式：

- _emissiveColor_: The color emitted from the material.
- _ambientColor_: AKA _Diffuse Color_ in other nomenclature.
- _reflectionColor_: The color reflected from the material.
- _reflectivityColor_: AKA _Specular Color_ in other nomenclature.

- _emissiveColor_: 从材质发出的颜色.
- _ambientColor_: AKA _Diffuse Color_ in other nomenclature.
- _reflectionColor_: 从材质反射的颜色.
- _reflectivityColor_: AKA _Specular Color_ in other nomenclature.


#### Change a color gradually
####逐渐改变颜色

Change a color gradually with linear interpolation between two colors, using the `.lerp()` function.

使用`.lerp（）`函数，在两种颜色之间使用线性插值逐渐改变颜色。

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
    myMaterial.albedoColor = Color3.lerp(red, yellow, colorRatio)
    if (colorRatio < 1) {
      colorRatio += 0.01
    }
  }
}

// Add the system to the engine
engine.addSystem(ColorSystem)
```

The example above changes the color of a material from red to yellow, incrementally shifting it on every frame.

上面的示例将材质的颜色从红色更改为黄色，并在每帧上逐步移动它。

## Using textures

##使用纹理

Use an image file as a texture in a material. In a `BasicMaterial` component, you set the `texture` field. In a `Material` component, you set the `albedoTexture` field. Albedo textures respond to light and can include shades on them.

The `Material` component allows you to use several image files as layers to compose more realistic textures, for example including a `bumpTexture` and a `refractionTexture`.

将图像文件用作材质中的纹理。在`BasicMaterial`组件中，设置`texture`字段。在`Material`组件中，设置`albedoTexture`字段。反照率纹理对光线有反应，并且可以包含阴影。

`Material`组件允许您使用多个图像文件作为图层来构成更逼真的纹理，例如包括`bumpTexture`和`refractionTexture`。

```ts
//Create entity and assign shape
const myEntity = new Entity()
myEntity.set(new BoxShape())

//Create material and configure its fields
const myMaterial = new Material()
myMaterial.albedoTexture = "materials/wood.png"
myMaterial.bumpTexture = "materials/woodBump.png"

//Assign the material to the entity
myEntity.set(myMaterial)
```

In the example above, the image for the material is located in a `materials` folder, which is located at root level of the scene project folder.

在上面的示例中，材质的图像位于“材料”文件夹中，该文件夹位于场景项目文件夹的根级别。

> Tip: We recommend keeping your texture image files separate in a `/materials` folder inside your scene.

>提示：我们建议将纹理图像文件保存在场景内的`/ materials`文件夹中。

#### Texture mapping

####纹理映射

If you want the texture to be mapped to specific scale or alignment on your entities, then you need to configure _uv_ properties on the [shape components]({{ site.baseurl }}{% post_url /development-guide/2018-02-6-shape-components %}).

如果您希望将纹理映射到实体上的特定比例或对齐，则需要在[形状组件]上配置_uv_属性（{{site.baseurl}} {％post_url / development-guide / 2018-02- 6形状组分％}）。

<!--
Use the [Decentraland sprite helpers](https://github.com/decentraland/dcl-sprites) library to map textures easily. Read documentation on how to use this library in the provided link.

-->

To handle texture mapping manually, you set _u_ and _v_ coordinates on the 2D image of the texture to correspond to the vertices of the shape. The more vertices the entity has, the more _uv_ coordinates need to be defined on the texture, a plane for example needs to have 8 _uv_ points defined, 4 for each of its two faces.

要手动处理纹理映射，请在纹理的2D图像上设置_u_和_v_坐标，以对应于形状的顶点。实体具有的顶点越多，需要在纹理上定义的_uv_坐标越多，例如，平面需要定义8个_uv_点，其两个面中的每一个都需要4个。

```tsx
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
myEntity.set(plane)
myEntity.set(myMaterial)
```

To create an animated sprite, use texture mapping to change the selected regions of a same texture that holds all the frames.


要创建动画精灵，请使用纹理映射来更改包含所有帧的相同纹理的选定区域。

## Reuse materials

##重复使用材料

If multiple entities in your scene use a same material, there's no need to create an instance of the material component for each. All entities can share one same instance, this keeps your scene lighter to load and prevents you from exceeding the maximum amount of materials per scene.

如果场景中的多个实体使用相同的材​​质，则无需为每个实体创建材质组件的实例。所有实体都可以共享一个相同的实例，这可以使您的场景更轻，以防止您超出每个场景的最大材料量。

```ts
//Create entities and assign shapes
const box = new BoxShape()
const myEntity = new Entity()
myEntity.set(box)
const mySecondEntity = new Entity()
mySecondEntity.set(box)
const myThirdEntity = new Entity()
myThirdEntity.set(box)

//Create material and configure fields
const myMaterial = new Material()
myMaterial.albedoColor = Color3.Blue()

//Assign same material to all entities
myEntity.set(myMaterial)
mySecondEntity.set(myMaterial)
myThirdEntity.set(myMaterial)
```

## Transparent materials
##透明材料

To make a material transparent, you must add an alpha channel to the image you use for the texture. The _material_ component ignores the alpha channel of the texture image by default, so you must either:

要使材质透明，必须将Alpha通道添加到用于纹理的图像中。 _material_组件默认忽略纹理图像的Alpha通道，因此您必须：

- Set `hasAlpha` to true.
- Set an image in `alphaTexture`. This image can be the same as the texture, or a different image.

```tsx
const myMaterial = new Material()
myMaterial.hasAlpha = true
// or
const myMaterial2 = new Material()
myMaterial2.alphaTexture = "materials/alphaTexture.png"
```

## Texture stretching in basic materials
##基础材料的纹理拉伸

When textures are stretched or shrinked to a different size from the original texture image, this can sometimes create artifacts. There are various [texture filtering](https://en.wikipedia.org/wiki/Texture_filtering) algorithms that exist to compensate for this in different ways. The `BasicMaterial` component uses the _bilinear_ algorithm by default, but you can configure it to use the _nearest neighbor_ or _trilinear_ algorithms instead by setting the `samplingMode`.

当纹理被拉伸或缩小到与原始纹理图像不同的大小时，这有时会产生伪影。存在各种[纹理过滤]（https://en.wikipedia.org/wiki/Texture_filtering）算法以便以不同方式对此进行补偿。 `BasicMaterial`组件默认使用_bilinear_算法，但您可以通过设置`samplingMode`将其配置为使用_nearest neighbor_或_trilinear_算法。

```tsx
const myMaterial = new BasicMaterial()
myMaterial.samplingMode = 1
```

The example above uses a nearest neighbor algorithm. This setting is ideal for pixel art style graphics, as the contours will remain sharply marked as the texture is seen larger on screen instead of being blurred.

上面的示例使用最近邻居算法。此设置非常适合像素艺术风格的图形，因为在屏幕上看到较大的纹理而不是模糊时，轮廓将保持清晰标记。
