---
date: 2018-02-26
title: Scene metadata
description: 如何添加场景元数据。
categories:
  - development-guide
type: Document
set: development-guide
set_order: 30
---


所有场景都有一个 `scene.json` 文件，您可以在其中设置场景的元数据。 此文件中的某些字段是 Decentraland 客户端所需要的。

您也可以自由添加任何您想要的字段。 将来，可以通过其它客户端或嵌入在装备中的其他脚本读取使用自定义字段。

## 场景地块

当[部署](({{ site.baseurl }}{% post_url /deploy/2018-01-07-publishing %}))场景时，`scene.json` 文件必须包含有关使用 Decentraland 地图中哪些地块的信息。 CLI 从此字段中读取此信息并将场景直接部署到这些土地上。

```json
 "scene": {
    "parcels": [
      "54,-14"
    ],
    "base": "54,-14"
  }
```

除非您正在构建占用多个地块的场景，否则在离线开发场景时不需要此信息。

`base` 字段定义要使用哪块地作为基准。如果场景只有一块地，那么基准就是这块地。如果你的场景有多块地，则应该是左下角(西南方向)的地块。所有实体的位置将参照本地块的西南角来计算。

所有实体位置将按照该地块的东南角开始计算。

要在场景预览中显示多块土地，请列出您打算使用的土地。 它们不需要是您将要部署的确切地块，但它们应该相邻并以相同的方式相互排列。

```json
 "scene": {
    "parcels": [
      "54,-14",  "55,-14"
    ],
    "base": "54,-14"
  }
```


## 联系信息

如果您希望其他开发人员能够与您联系，您可以将联系信息添加到 `scene.json` 文件中。

```json
  "contact": {
    "name": "author-name",
    "email": "name@mail.com"
  },
```

## 用户出现位置

`spawnPoints` 字段定义用户直接访问场景时出现的位置，或直接在浏览器中输入坐标。

您的场景可能具有可阻止用户移动的对象（如果恰好在树上或楼梯上），或者您的场景可能具有高架地形。 如果用户一进入就不能移动，对用户来说将是一次糟糕的体验。 这就是为什么可以设置多个出现位置。

```json
"spawnPoints": [
    {
      "name": "spawn1",
      "default": true,
      "position": {
        "x": 5,
        "y": 1,
        "z": 4
      }
    }
  ],
```

位置由场景内的坐标组成。 这些数字为地块中的位置，类似于您在 Transform 组件场景代码中用于定位的[实体位置](({{ site.baseurl }}{% post_url /development-guide/2018-01-12-entity-positioning %}))。

### 多个出现点

单个场景可以有多个出现点。 如果多个玩家同时访问场景，这有助于玩家重叠在一起。 要设置多个出现点，只需将它们列为数组即可。


```json
"spawnPoints": [
    {
      "name": "spawn1",
      "default": true,
      "position": {
        "x": 5,
        "y": 1,
        "z": 4
      }
	},
	{
      "name": "spawn2",
      "default": true,
      "position": {
        "x": 3,
        "y": 1,
        "z": 1
      }
    }
  ],
```


Spawn points marked as `default` are given preference. When there are multiple spawn points marked as `default`, one of them will be picked randomly from the list.

标记为 `default` 的出现点被优先考虑。 当多个出现点标记为 `default` 时，将从列表中随机挑选一个。

> 注意：在将来的版本中，当玩家试图出现在场景中并其他玩家占用了默认的出现点时，玩家将被发送到列出的其他位置。 这将允许玩家根据出现点名称传送到出现点，如 `scene.json` 中所述。


### 出现区域

可以在场景中设置整个区域以充当出现点。 通过在位置的任何维度上指定两个数字的数组，玩家将出现在该数字范围内的随机位置。 这有助于防止玩家进入时重叠。


```json
"spawnPoints": [
    {
      "name": "spawn1",
      "default": true,
      "position": {
        "x": [1,5],
        "y": 1,
        "z": [2,4]
      }
    }
  ],
```

在上面的例子中，玩家可能出现在广场上 _1,1,2_ 和 _5,1,4_ 间的任何角落。

### 朝向

还可以指定玩家出现时的朝向，让他们出现时面向特定的方向。 这使您可以更好地控制他们的第一印象，并希望他们能朝向特定方向非常有用。

只需在出现点数据中添加 `cameraTarget` 字段即可。 `cameraTarget` 的值应引用空间中的位置，相对于场景的 _x_，_y_ 和 _z_ 坐标，就像 `position` 字段一样。


```json
"spawnPoints": [
    {
      "name": "spawn1",
      "default": true,
      "position": {
        "x": 5,
        "y": 1,
        "z": 4
      },
      "cameraTarget": {
        "x": 10,
        "y": 1,
        "z": 4
      }
    }
],
```

上例玩家在 _5,1,4_ 位置并朝向 _10, 1, 4_ 出现。 如果生成位置是范围，则玩家的朝向将始终与指示的目标匹配。 如果有多个出现点，则每个出现点都可以有自己独立的目标。

## 自定义数据

您可以在 "policy" 部分中增加任何您认为有关的内容。 用于浏览世界的社区开发的（非官方）客户端可能会使用这些字段，用于过滤内容或限制用户执行某些操作（如飞行或战斗）。

此元数据也可以由装备脚本使用，例如，轻型军刀可能用于允许发生战斗的场景中。

随着清晰的用例出现，我们计划对可以添加到此文件的元数据定义更多约定。

```json
  "policy": {
    "contentRating": "E",
    "fly": true,
    "voiceEnabled": true,
    "blacklist": [],
    "teleportPosition": ""
  }
```
