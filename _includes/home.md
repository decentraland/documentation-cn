# 让我们一起来构建虚拟世界

### 使用 Decentraland SDK 构建交互式 3D 内容。
## 快捷菜单

<div class="shortcuts">
  <a href="{{ site.baseurl }}{% post_url /development-guide/2018-01-21-scene-content %}">
    <div>
      <div class="image"><img src="/images/home/1.png"/></div>
      <div class="title">场景内容</div>
      <div class="description">有关构成场景的实体和组件的概述</div>
    </div>
  </a>
  <a href="{{ site.baseurl }}{% post_url /development-guide/2018-06-21-entity-interfaces %}">
    <div>
      <div class="image"><img src="/images/home/2.png"/></div>
      <div class="title">实体参考指南</div>
      <div class="description">有关构建 Decentraland 场景中最基本单元的完整参考</div>
    </div>
  </a>
  <a href="{{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %}">
    <div>
      <div class="image"><img src="/images/home/3.png"/></div>
      <div class="title">场景实例</div>
      <div class="description">帮助您入门的几个示例场景，激发您的创作激情</div>
    </div>
  </a>
</div>

## CLI 安装

在 Decentraland 上构建开发场景，您首先需要安装命令行接口（CLI）。

CLI 可以让你在本地编译和预览场景。场景在本地测试后，您可以使用 CLI 上传内容。

> **注意**: 在安装 CLI 之前，请安装以下的依赖项目：
>
> - [Node.js](https://github.com/decentraland/cli#nodejs-installation) (version 8)
> - [IPFS](https://dist.ipfs.io/#go-ipfs)
> - [Python 2.7.x](https://www.python.org/downloads/)

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

使用代码编辑器打开场景文件夹中的 `scene.tsx` 文件。

{% raw %}
```tsx
import * as DCL from 'decentraland-api'

export default class SampleScene extends DCL.ScriptableScene {
  async render() {
    return (
      <scene>
        <box position={{ x: 5, y: 0.5, z: 5 }} rotation={{ x: 0, y: 45, z: 0 }} color="#4CC3D9" />
        <sphere position={{ x: 6, y: 1.25, z: 4 }} color="#EF2D5E" />
        <cylinder position={{ x: 7, y: 0.75, z: 3 }} radius={0.5} scale={{ x: 0, y: 1.5, z: 0 }} color="#FFC65D" />
        <plane position={{ x: 5, y: 0, z: 6 }} rotation={{ x: -90, y: 0, z: 0 }} scale={4} color="#7BC8A4" />
      </scene>
    )
  }
}
```
{% endraw %}

可以在这里更改任何你想要更改的内容，例如更改其中一个实体的 _x_ 位置。如果预览正在浏览器中运行，则在预览中可以看到相应的更改。

Download this 3D model of an avocado from [Google Poly](https://poly.google.com) in _glTF_ format. [link](https://poly.google.com/view/cgLBGFfm5FU)

从 [Google Poly](https://poly.google.com) 以_glTF_格式下载鳄梨 3D 模型。[链接](https://poly.google.com/view/cgLBGFfm5FU)

![](/images/media/landing_avocado_gltf.png)

Create a new folder under your scene’s directory named `/models`. Extract the downloaded files and place them all in that folder.

In your scene’s code, add the following line in between the other XML entities:

{% raw %}

```tsx
<gltf-model
  src="models/Avocado.gltf"
  position={{ x: 3, y: 0.75, z: 2 }}
  scale={10}
/>
```

{% endraw %}

Check your scene preview once again to see that the 3D model is now there too.

![](/images/media/landing_avocado_in_scene.png)

Read [coding-scenes]({{ site.baseurl }}{% post_url /getting-started/2018-01-02-coding-scenes %}) for a high-level understanding of how Decentraland scenes function. Check [scene-content]({{ site.baseurl }}{% post_url /development-guide/2018-01-21-scene-content %}) for specifics about how to add content to a scene.

## Scene examples

<div class="examples">
  <a target="_blank" href="https://github.com/decentraland/sample-scene-script">
    <div>
      <img src="/images/home/door.png"/>
      <span>Door scene</span>
    </div>
  </a>
  <a target="_blank" href="https://github.com/decentraland/sample-scene-array-of-entities">
    <div>
      <img src="/images/home/hummingbirds.png"/>
      <span>Hummingbirds</span>
    </div>
  </a>
  <a target="_blank" href="https://github.com/decentraland/sample-scene-Block-Dog">
    <div>
      <img src="/images/home/blockdog.png"/>
      <span>BlockDog</span>
    </div>
  </a>
</div>

See [sample scenes]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %}) for more scene examples.

Also see [tutorials]({{ site.baseurl }}{% post_url /tutorials/2018-01-03-tutorials %}) for detailed instructions for building scenes like these.

## Other useful information

- [Design constraints for games]({{ site.baseurl }}{% post_url /design-experience/2018-01-08-design-games %})
- [TypeScript tips]({{ site.baseurl }}{% post_url /development-guide/2018-01-08-typescript-tips %})
- [3D model consierations]({{ site.baseurl }}{% post_url /development-guide/2018-01-09-external-3d-models %})
- [Scene limitations]({{ site.baseurl }}{% post_url /development-guide/2018-01-06-scene-limitations %})
