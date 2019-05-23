---
date: 2018-02-25
title: 异步代码
description: 如何在场景代码中使用异步函数。
categories:
  - development-guide
type: Document
set: development-guide
set_order: 30
---

## 概述

场景中的大多数代码在单个线程中同步运行。意味着代码是逐行顺序执行的。每条指令必须先等待上一条指令完全执行完才会启动。

甚至场景系统中的 `update()` 函数也是按照[优先级](({{ site.baseurl }}{% post_url /development-guide/2018-02-3-systems %}#system-execution-order))一个接一个地执行的。

同步运行代码可以确保一致性，因为可以确定代码指令的运行顺序。

另一方面，场景需要每秒更新许多次来构建下一帧。 如果一部分代码需要很长时间才能响应，那么整个主线程就会卡住，从而导致帧速率滞后。

就就是为什么，在某些特殊情况下，有些指令需要异步方式运行。这样就可以执行一项任务，然后不用等待该任务返回结果的情况下执行下一行代码。

这对于需要时间响应的外部服务非常有用。这样不会因为等待响应而阻止其他任务执行。

例如：

- 播放声音文件
- 从 REST API 获取数据
- 在区块链上执行交易


<!--
- When parsing a JSON file (??)
-->
<!--
因为场景需要每秒更新多次，因此不能让场景的主线程等待来自外部服务的回答。当任务处于等待响应的空闲状态时，其余代码需要继续运行，并构建下一个桢。

[ ASYNC DIAGRAMS]
-->

## executeTask 函数

`executeTask` 函数在与场景主线程不同的线程中异步执行 lambda 函数。

```ts
// Start an asynchronous task
executeTask(async () => {
  try {
    let response = await fetch(callUrl)
    let json = await response.json()
    log(json)
  } catch {
    log("failed to reach the URL")
  }
})

// Rest of the code keeps being executed
```

上面的示例执行一个函数，该函数包含一个 `fetch()` 操作，用于从外部 API 检索数据。在执行此异步过程时，场景中的其余代码将继续执行。

<!--
Note that there are two `await` statements here, one to get data from
-->

> 注意：请记住，在任务完成执行之前，可能已经渲染了几帧场景。确保场景的代码足够灵活，以便在异步任务完成时处理中间场景。

## OnClick 函数

您可以向任何实体添加一个 `OnCLick` 组件，以便在每次单击该实体时触发异步 lambda 函数。

```ts
myEntity.addComponent(
  new OnClick(e => {
    log("clicked on the entity", e)
  })
)
```

## 订阅监听器

运行异步代码的另一种方法是实例化事件监听器。每次发生给定事件时，事件侦听器都会触发异步 lambda 函数的运行。


```ts
//Instance the Input object
const input = Input.instance

// Subscribe to an event
input.subscribe("BUTTON_DOWN", e => {
  log("pointerUp works", e)
})
```

上面的示例在每次按下按钮 _A_ 时，都会运行函数。

<!-- If multiple events in rapid succession, do we get multiple independent threads? -->
