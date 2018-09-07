---
date: 2018-01-01
title: 实体参考指南
redirect_from:
  - /docs/entities
description: 实体和构造实体的可用接口
categories:
  - sdk-reference
type: Document
set: sdk-reference
set_order: 4
---

## 简介

实体是 Decentraland 场景中构建所有内容的基本单元，可以将它们视为 Web 开发中 DOM 树中元素的等效元素。所有实体共享相同的基本构造函数，它们具有 tag、属性和子实体。

`<entity>` 是 Decentraland 的基本元素，所有元素都是继承基础的 `entity` 对象。 `<entity>` 可以包含多个组件，每个组件引入以不同方式修改实体的属性。 例如，您可以在实体上包含`color` 组件来设置其颜色，或者包含 `withCollisions` 组件以使其不可穿越。

> 提示：通过源代码编辑器（如Visual Studio Code 或 Atom）编辑代码时，您可以看到某种实体支持的组件列表。 通过将光标放在实体中并按 _Ctrl + Space bar_ 就能显示。

实体可以将其他实体作为子实体，这些实体从父实体继承组件。 如果父实体被定位、缩放或旋转，其子项也会受到影响。这样，我们就可以以树形结构来管理实体。

```ts
interface IEntity {
  /** name of the entity */
  tag: string
  /** dictionary of attributes a.k.a.: properties */
  attributes: Dictionary<any>
  /** children entities */
  children: IEntity[]
}
```

## Box

创建立方体。

示例:

{% raw %}

```tsx
<box position={{ x: 5, y: 0, z: 2 }} color="#ff00aa" scale={2} />
```

{% endraw %}

接口参考：

```tsx
interface BoxEntity extends BaseEntity {
  /** Color of the vertices */
  color?: string
  /** Material selector */
  material?: string
  /** Set to true to turn on the collider for the entity. */
  withCollisions?: boolean
}
```

## Sphere

创建球体。

示例:

{% raw %}

```tsx
<sphere position={{ x: 5, y: 0, z: 2 }} color="#ff00aa" scale={2} />
```

{% endraw %}

接口参考：

```tsx
interface SphereEntity extends BaseEntity {
  /** Color of the vertices */
  color?: string
  /** Material selector */
  material?: string
  /** Set to true to turn on the collider for the entity. */
  withCollisions?: boolean
}
```

## Plane

创建平面。

示例:

{% raw %}

```tsx
<plane
  position={{ x: 5, y: 0, z: 2 }}
  color="#ff00aa"
  scale={{ x: 10, y: 5, z: 1 }}
/>
```

{% endraw %}

接口参考：

{% raw %}

```tsx
interface PlaneEntity extends BaseEntity {
  /** Color of the vertices */
  color?: string

  /** Material selector */
  material?: string

  /** Set to true to turn on the collider for the entity. */
  withCollisions?: boolean
}
```

{% endraw %}

## Cylinder and Cone

创建圆锥（cone）。 圆柱体（Cylinder）定义为具有相同底部和顶部半径的圆锥体。

圆锥示例：

```tsx
<cone
  radiusTop={0}
  radiusBottom={1}
  position={vector}
  color="#ff00aa"
  scale={2}
/>
```

圆柱体示例：

```tsx
<cylinder
  openEnded
  arc={180}
  radius={0.5}
  position={vector}
  color="#ff00aa"
  scale={2}
/>
```

接口参考：


```tsx
interface CylinderEntity extends BaseEntity {
  /** Color of the vertices */
  color?: string

  /** Material selector */
  material?: string

  /** Set to true to turn on the collider for the entity. */
  withCollisions?: boolean

  /** Radius (meters) */
  radius?: number

  /** How much of the arc should be rendered, 360 by default (degrees) */
  arc?: number

  /** Radius of the top face (meters) */
  radiusTop?: number

  /** Radius of the bottom face (meters) */
  radiusBottom?: number

  /** Radial segments of the geometry. 4 will render a tetrahedron. */
  segmentsRadial?: number

  /** Vertical segments of the geometry */
  segmentsHeight?: number

  /** Render caps */
  openEnded?: boolean
}
```

## Text entity

Text entity 可以在场景中显示一串文本。可以配置一些基本的文本属性，如文本颜色，字体，字体粗细，行间距等。

```tsx
type TextEntity = BaseEntity & {
  /**
   * The width of the texts outline
   */
  outlineWidth?: number
  /**
   * The outline color in hexadecimal format (`#ff0000`)
   */
  outlineColor?: string
  /**
   * The text color in hexadecimal format (`#ff0000`)
   */
  color?: string
  /**
   * The name of the font to be used
   */
  fontFamily?: string
  /**
   * The text size
   */
  fontSize?: number
  /**
   * The weight of the text
   */
  fontWeight?: string
  /**
   * The text size
   */
  opacity?: number
  /**
   * The content of the text
   */
  value: string
  /**
   * The size of the space between lines
   */
  lineSpacing?: string
  /**
   * If set to true the text will wrap to the next line when the maximun width is reached
   */
  textWrapping?: boolean
  /**
   * Horizontal alignment (`top`, `right`, `bottom` or `left`)
   */
  hAlign?: string
  /**
   * Vertical alignment (`top`, `right`, `bottom` or `left`)
   */
  vAlign?: string
  /**
   * The text width
   */
  width?: number
  /**
   * The text height
   */
  height?: number
  lineCount?: number
  resizeToFit?: boolean
  shadowBlur?: number
  shadowOffsetX?: number
  shadowOffsetY?: number
  shadowColor?: string
  zIndex?: number
  paddingTop?: number
  paddingRight?: number
  paddingBottom?: number
  paddingLeft?: number
}
```

## glTF 模型

[glTF](https://www.khronos.org/gltf) (GL传输格式)是Khronos 的一个开源项目，它提供了一个通用的、高效又可与现代 Web 技术高度互操作的可扩展的 3D 资产格式。

_gltf-model_实体使用 glTF 文件加载 3D 模型。它支持 _.gltf_ 或 _.glb_ 扩展名。

> _.gltf_ 使用了人类易读的格式，而 _.glb_ 是它的压缩版本。

简单的例子：

{% raw %}

```tsx
<gltf-model
  position={{ x: 5, y: 3, z: 5 }}
  scale={0.5}
  src="models/shark_anim.gltf"
/>
```

{% endraw %}

动画示例：

{% raw %}

```tsx
<gltf-model
  position={{ x: 5, y: 3, z: 5 }}
  scale={0.5}
  src="models/shark_anim.gltf"
  skeletalAnimation={[
    { clip: "shark_skeleton_bite", playing: false },
    {
      clip: "shark_skeleton_swim",
      weight: 0.2,
      playing: true
    }
  ]}
/>
```

{% endraw %}

接口参考：

```tsx
interface GltfEntity extends BaseEntity {
  /**
   * The source URL of the .gltf or .glb model, required
   */
  src: string

  /**
   * List of weighted skeletal animations
   */
  skeletalAnimation?: Array<SkeletalAnimation>
}

interface SkeletalAnimation {
  /**
   * Name or index of the animation in the model
   */
  clip: string | number
  /**
   * Does the animation loop?, default: true
   */
  loop?: boolean
  /**
   * Weight of the animation, values from 0 to 1, used to blend several animations. default: 1
   */
  weight?: number

  /**
   * Is the animation playing? default: true
   */
  playing?: boolean
}
```

> 注意：请记住，所有模型及其纹理必须在[场景限制]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-06-scene-limitations %})范围内。

## 基本实体

`BaseEntity` 接口是最灵活的，因为它没有预定义的组件，可以让你为任何可能的组件设置值。

示例：

{% raw %}

```tsx
<entity position={{ x: 2, y: 1, z: 0 }} scale={{ x: 2, y: 2, z: 0.05 }} />
```

{% endraw %}

您可以通过 XML 标记 `<entity>` 将基本实体添加到场景中。 可以将没有组件的实体添加到场景中以充当容器。 `<entity>` 元素默认没有组件，因此它是不可见的并且对场景没有直接影响，但它可以被定位、缩放和旋转，并且它可以包含其他子实体。子实体相对于父实体进行缩放、旋转和定位。

在动态场景中，使用没有组件的实体将一组实体封装成一个对象也是有用的，它可以成为一些函数的输入参数。

接口参考：

```tsx
interface BaseEntity {
  /**
   * Moves the entity center to a given point in the scene or relative to a parent entity
   */
  position?: Vector3

  /**
   * Rotates the entity
   * The `x,y,z` components are degrees (0°-360°), and every component represents the rotation in that axis
   */
  rotation?: Vector3

  /**
   * Scales the entity in three dimensions
   */
  scale?: Vector3 | number

  /**
   * Defines if the entity and its children should be rendered
   */
  visible?: string

  /**
   * The ID is used to attach events and identify the entity in the scene tree
   */
  id?: string

  /**
    * The function that handles the click interaction event
    */
   onClick?: (e: IEvents["click"]) => void

  /**
   * Used to differentiate similar entities in lists
   */
  key?: string | number

  /**
   * Used to animate the transitions in the same fashion as CSS
   */
  transition?: {
    position?: TransitionValue
    rotation?: TransitionValue
    scale?: TransitionValue
    color?: TransitionValue
  }

  /**
   * Billboard defines a behavior that makes the entity face the camera in any moment.
   * There are three combinable types of camera-facing options defined in the object BillboardModes.
   *   BILLBOARDMODE_NONE: 0
   *   BILLBOARDMODE_X: 1
   *   BILLBOARDMODE_Y: 2
   *   BILLBOARDMODE_Z: 4
   *   BILLBOARDMODE_ALL: 7
   *
   * To combine billboard types, write those in the form:
   *   BillboardModes.BILLBOARDMODE_X | BillboardModes.BILLBOARDMODE_Y
   */
  billboard?: IBillboardModes

  /**
   * Adds spatial sound to the entities
   */
  sound?: SoundComponent
}

/**
 * This data type defines a three component vector. It is used for scaling, positioning and rotations
 */
interface Vector3 {
  x: number
  y: number
  z: number
}

/**
 * This data type defines the configurations for the animations of some components like "position".
 */
interface TransitionValue {
  duration: number
  timing?: TimingFunction
  delay?: number
}

type TimingFunction =
  | "linear"
  | "ease-in"
  | "ease-out"
  | "ease-in-out"
  | "quadratic-in"
  | "quadratic-out"
  | "quadratic-inout"
  | "cubic-in"
  | "cubic-out"
  | "cubic-inout"
  | "quartic-in"
  | "quartic-out"
  | "quartic-inout"
  | "quintic-in"
  | "quintic-out"
  | "quintic-inout"
  | "sin-in"
  | "sin-out"
  | "sin-inout"
  | "exponential-in"
  | "exponential-out"
  | "exponential-inout"
  | "bounce-in"
  | "bounce-out"
  | "bounce-inout"
  | "elastic-in"
  | "elastic-out"
  | "elastic-inout"
  | "circular-in"
  | "circular-out"
  | "circular-inout"
  | "back-in"
  | "back-out"
  | "back-inout"

export type SoundComponent = {
  /** Distance fading model, default: 'linear' */
  distanceModel?: "linear" | "inverse" | "exponential"
  /** Does the sound loop? default: false */
  loop?: boolean
  /** The src of the sound to be played */
  src: string
  /** Volume of the sound, values 0 to 1, default: 1 */
  volume?: number
  /** Used in inverse and exponential distance models, default: 1 */
  rolloffFactor?: number
  /** Is the sound playing?, default: true */
  playing?: boolean
}
```

## Obj

> 注意：目前作为遗留功能支持 obj 模型，但我们有可能在未来不再支持。请尽量使用 GLTF。

示例:

```tsx
<obj-model src="models/shark.obj" />
```

接口参考：

```tsx
interface ObjEntity extends BaseEntity {
  /**
   * The source URL of the .obj model, required
   */
  src: string
}
```

## 材质

材质被定义为场景中的单独实体，这可以预防重复定义材质，简化场景中的代码。

然后可以将材质应用于 MaterialEntity（它本身就是BaseEntity 的子类）的子类的任何实体。所有基本实体和平面实体都可以使用材质，可以通过向其添加 `material` 组件来设置。

Example:

```tsx
  <material
    id="reusable_material"
    albedo-color="materials/wood.png"
    roughness={0.5}
    />
  <sphere
    material="#reusable_material"
    />
```

此示例定义一个新的材质，然后将它应用于球体。

接口参考：

```tsx
export type MaterialDescriptorEntity = {
  /**
   * Id of the material, it will be used to pick this material from other entities
   */
  id: string

  /**
   * Opacity.
   */
  alpha?: number

  /**
   * The color of a material in ambient lighting.
   */
  ambientColor?: ColorComponent

  /**
   * AKA Diffuse Color in other nomenclature.
   */
  albedoColor?: ColorComponent

  /**
   * AKA Specular Color in other nomenclature.
   */
  reflectivityColor?: ColorComponent

  /**
   * The color reflected from the material.
   */
  reflectionColor?: ColorComponent

  /**
   * The color emitted from the material.
   */
  emissiveColor?: ColorComponent

  /**
   * Specifies the metallic scalar of the metallic/roughness workflow.
   * Can also be used to scale the metalness values of the metallic texture.
   */
  metallic?: number

  /**
   * Specifies the roughness scalar of the metallic/roughness workflow.
   * Can also be used to scale the roughness values of the metallic texture.
   */
  roughness?: number

  /**
   * Texture applied as material.
   */
  albedoTexture?: string

  /**
   * Texture applied as opacity. Default: the same texture used in albedoTexture.
   */
  alphaTexture?: string

  /**
   * Emmisive texture.
   */
  emisiveTexture?: string

  /**
   * Stores surface normal data used to displace a mesh in a texture.
   */
  bumpTexture?: string

  /**
   * Stores the refracted light information in a texture.
   */
  refractionTexture?: string

  /**
   * Intensity of the direct lights e.g. the four lights available in scene.
   * This impacts both the direct diffuse and specular highlights.
   */
  directIntensity?: number

  /**
   * Intensity of the emissive part of the material.
   * This helps controlling the emissive effect without modifying the emissive color.
   */
  emissiveIntensity?: number

  /**
   * Intensity of the environment e.g. how much the environment will light the object
   * either through harmonics for rough material or through the refelction for shiny ones.
   */
  environmentIntensity?: number

  /**
   * This is a special control allowing the reduction of the specular highlights coming from the
   * four lights of the scene. Those highlights may not be needed in full environment lighting.
   */
  specularIntensity?: number

  /**
   * AKA Glossiness in other nomenclature.
   */
  microSurface?: number

  /**
   * If sets to true, disables all the lights affecting the material.
   */
  disableLighting?: boolean

  /**
   * Sets the transparency mode of the material.
   *
   * | Value | Type                                |
   * | ----- | ----------------------------------- |
   * | 0     | OPAQUE  (default)                   |
   * | 1     | ALPHATEST                           |
   * | 2     | ALPHABLEND                          |
   * | 3     | ALPHATESTANDBLEND                   |
   */
  transparencyMode?: ITransparencyModes

  /**
   * Does the albedo texture has alpha?
   */
  hasAlpha?: boolean
}

export type MaterialEntity = BaseEntity & {
  /**
   * Color of the vertices
   */
  color?: string | number

  /**
   * Material selector
   */
  material?: string

  /**
   * Set to true to turn on the collider for the entity.
   */
  withCollisions?: boolean
}

export type BasicMaterialEntity = {
  /**
   * Id of the material, it will be used to pick this material from other entities
   */
  id: string

  /**
   * The source of the texture image
   */
  texture: string

  /**
   * Enabled crisper images based on the provided sampling mode
   * | Value | Type      |
   * |-------|-----------|
   * |     1 | NEAREST   |
   * |     2 | BILINEAR  |
   * |     3 | TRILINEAR |
   */
  samplingMode?: number

  /**
   * A number between 0 and 1.
   * Any pixel with an alpha lower than this value will be shown as transparent.
   */
  alphaTest?: number
}
```

## 创建自定义接口

您可以创建自己的接口来创建具有自定义默认行为和特征的实体。 要定义接口，请创建一个新的 _.tsx_ 文件，其中包含构造和处理实体所需的所有组件和方法。

例如，下面的示例定义了一个实体类型 `button`：

{% raw %}

```tsx
import { createElement, Vector3Component } from "decentraland-api"

export interface IProps {
  position: Vector3Component
}

export const Button = (props: IProps) => {
  return (
    <entity position={props.position}>
      <gltf-model
        id="start_button"
        src="assets/My_Button.gltf"
        rotation={{ x: 90, y: 0, z: 0 }}
        scale={{ x: 0.5, y: 0.5, z: 0.5 }}
      />
    </entity>
  )
}
```

{% endraw %}

在使用此实体类型之前，请将其保存为 _.tsx_ 文件并将其导入 _scene.tsx_：

```tsx
import { Button } from "./src/Button"
```

导入文件后，只需编写以下内容即可向场景添加按钮：

{% raw %}

```tsx
<Button position={{ x: 0, y: 1.5, z: 0 }} />
```

{% endraw %}
