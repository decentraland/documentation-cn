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

`base` 字段定义要使用哪块地作为基础。所有实体位置将按照该地块的东南角开始计算。

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

`policy` 部分中的 `teleportPosition` 字段定义用户直接访问场景时出现的位置，方法是直接在浏览器中输入坐标。

您的场景可能具有可阻止用户移动的对象（如果恰好在树上或楼梯上），或者您的场景可能具有高架地形。 如果用户一进入就不能移动，对用户来说将是一次糟糕的体验。 这就是为什么您可以选择设置不同于默认值的自定义出现位置。

```json
  "policy": {
    "teleportPosition": "3, 0, 3"
  }
```

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