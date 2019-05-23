# 让我们一起来构建虚拟世界

### 使用 Decentraland SDK 构建交互式 3D 内容。
## 快捷菜单

<div class="shortcuts">
  <a href="{{ site.baseurl }}{% post_url /getting-started/2018-01-02-coding-scenes %}">
    <div>
      <div class="image"><img src="/images/home/1.png"/></div>
      <div class="title">场景开发</div>
      <div class="description">工具概述及 SDK 的基本概念</div>
    </div>
  </a>
  <a href="">
    <div>
      <div class="image"><img src="/images/home/2.png"/></div>
      <div class="title">组件和对象参考指南</div>
      <div class="description">有关缺省组件和对象及相关函数的完整参考</div>
    </div>
  </a>
  <a href="{{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %}">
    <div>
      <div class="image"><img src="/images/home/3.png"/></div>
      <div class="title">场景实例</div>
      <div class="description">帮助您入门的几个代码示例，激发您的创作激情</div>
    </div>
  </a>
</div>

## CLI 安装

在 Decentraland 上构建开发场景，您首先需要安装命令行接口（CLI）。

CLI 可以让你在本地编译和预览场景。场景在本地测试后，您可以使用 CLI 上传内容。

> **注意**: 在安装 CLI 之前，请安装以下的依赖项目：
>
> - [Node.js](https://github.com/decentraland/cli#nodejs-installation) (version 8)

请在命令行工具中运行以下命令安装 CLI：

```bash
npm install -g decentraland
```

有关安装 CLI 的更多详细信息，请参阅[安装指南]({{ site.baseurl }}{% post_url /getting-started/2018-01-01-installation-guide %})。

## 创建你的第一个场景

在空目录中运行以下命令创建新场景：

```bash
dcl init
```

在此目录中运行以下命令，以在浏览器中预览 3D 场景：

```bash
dcl start
```

有关场景预览的更多信息，请查看[场景预览]({{ site.baseurl }}{% post_url /getting-started/2018-01-04-preview-scene %})。

## 编辑场景

使用代码编辑器打开场景文件夹中的 `src/game.ts` 文件。

> 提示：我们建议使用如[Visual Studio Code](https://code.visualstudio.com/)或[Atom](https://atom.io/)这样的源代码编辑器。 可以帮助您标记语法错误，自动完成，甚至可以提示基于上下文的智能建议。 还可以单击对象以查看其类的完整定义。

```ts
/// --- Set up a system ---

class RotatorSystem {
  // this group will contain every entity that has a Transform component
  group = engine.getComponentGroup(Transform)

  update(dt: number) {
    // iterate over the entities of the group
    for (let entity of this.group.entities) {
      // get the Transform component of the entity
      const transform = entity.getComponent(Transform)

      // mutate the rotation
      transform.rotate(Vector3.Up(), dt * 10) 
    }
  }
}

// Add a new instance of the system to the engine
engine.addSystem(new RotatorSystem())

/// --- Spawner function ---

function spawnCube(x: number, y: number, z: number) {
  // create the entity
  const cube = new Entity()

  // set a transform to the entity
  cube.addComponent(new Transform({ position: new Vector3(x, y, z) }))

  // set a shape to the entity
  cube.addComponent(new BoxShape())

  // add the entity to the engine
  engine.addEntity(cube)

  return cube
}

/// --- Spawn a cube ---

const cube = spawnCube(5, 1, 5)

cube.addComponent(
  new OnClick(() => {
    cube.getComponent(Transform).scale.z *= 1.1
    cube.getComponent(Transform).scale.x *= 0.9

    spawnCube(Math.random() * 8 + 1, Math.random() * 8, Math.random() * 8 + 1)
  })
)
```

可以在这里更改任何你想要更改的内容，例如更改第一个 `cube` 实体的 _x_ 位置。如果预览正在浏览器中运行，则在预览中将能看到相应的更改。

从 [Google Poly](https://poly.google.com) 以 _glTF_ 格式下载这个鳄梨的 3D 模型。[链接](https://poly.google.com/view/cgLBGFfm5FU)

![](/images/media/landing_avocado_gltf.png)

在场景目录下建立一个新的目录 `/models`。提取下载的文件并将它们全部放在该文件夹中。

在场景的代码最后添加以下行：

```tsx
let avocado = new Entity()
avocado.addComponent(new GLTFShape("models/avocado.gltf"))
avocado.addComponent(new Transform({ 
    position: new Vector3(3, 1, 3), 
    scale: new Vector3(10, 10, 10)
    }))
engine.addEntity(avocado)
```

再次检查场景预览，看看 3D 模型是否已经出现。

![](/images/media/landing_avocado_in_scene.png)

添加的行创建了一个新的[实体]({{ site.baseurl }}{% post_url /development-guide/2018-02-1-entities-components %})，然后给了它一个基于您下载的 3D 模型的[形状]({{ site.baseurl }}{% post_url /development-guide/2018-02-6-shape-components %})，并[设置了位置和缩放]({{ site.baseurl }}{% post_url /development-guide/2018-01-12-entity-positioning %})。

请注意，您添加的鳄梨会旋转，就像场景中的所有其他实体一样。 那是因为在这个场景的默认代码中定义的 `RotatorSystem` [系统]({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %})遍历了场景中的所有实体然后将它旋转。

要深入了解 Decentraland 场景的运作方式，请阅读[场景开发]({{ site.baseurl }}{% post_url /getting-started/2018-01-02-coding-scenes %}) 。

有关向场景添加内容的更多说明，请参阅**开发指南**部分。

## 发布场景

创建场景完成后要将其上传到 LAND ，请参阅[发布]({{ site.baseurl }}{% post_url /deploy/2018-01-07-publishing %})。

## 场景示例

<div class="examples">
  <a target="_blank" href="https://github.com/decentraland-scenes/Hypno-wheels">
    <div>
      <img src="/images/home/example-hypno-wheel.png"/>
      <span>Hypno wheels</span>
    </div>
  </a>
  <a target="_blank" href="https://github.com/decentraland-scenes/Hummingbirds">
    <div>
      <img src="/images/home/hummingbirds.png"/>
      <span>Hummingbirds</span>
    </div>
  </a>
  <a target="_blank" href="https://github.com/decentraland-scenes/Gnark-patrol">
    <div>
      <img src="/images/home/example-gnark.png"/>
      <span>Gnark patrolling</span>
    </div>
  </a>
</div>

有关更多场景示例，请参阅[场景示例]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %})。

有关构建此类场景的详细说明，请参阅[教程]({{ site.baseurl }}{% post_url /tutorials/2018-01-03-tutorials %})。

## 其他有用的信息

- [游戏设计限制]({{ site.baseurl }}{% post_url /design-experience/2018-01-08-design-games %})
- [3D 模型考虑因素]({{ site.baseurl }}{% post_url /development-guide/2018-01-09-external-3d-models %})
- [场景限制]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})
