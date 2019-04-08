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

// LINK TO THIS PAGE FROM ALL THAT USE executeTask()

## Overview
## 概述

Most of the code in your scene runs synchronously using a single thread. That means that commands are executed sequentially line by line. Each command must first wait for the previous command to finish executing before it can start.

场景中的大多数代码在单个线程中同步运行。意味着代码是逐行顺序执行的。每条指令必须先等待上一条指令完全执行完才会启动。

Even the `update()` functions in your scene's systems are executed one by one, following the priority numbers set when adding the systems to the engine.

甚至场景系统中的 `update()` 函数也是按照系统添加到引擎时设置的优先级一个接一个地执行的。

Running code synchronously ensures consistency, as you can always be sure you'll know the order in which the commands in your code run.

同步运行代码可以确保一致性，因为可以确定代码指令的运行顺序。

In some special cases though, you want some commands to run asynchronously. This means that you can start off a task and the execution of the next line of code can start without waiting for that task to return a result.

但在某些特殊情况下，有些指令可能需要异步方式运行。这样就可以执行一项任务，然后不用等待该任务返回结果的情况下执行下一行代码。

This is useful for tasks that rely on external services that could take time to respond.

这对于可能需要时间响应的外部服务非常有用。

For example:

例如：

- When playing a sound file
- When retrieving data from a REST API
- When performing a transaction on the blockchain

- 播放声音文件
- 从 REST API 获取数据
- 在区块链上执行交易


<!--
- When parsing a JSON file (??)
-->

Since your scene needs to be updated many times per second, you can't afford to have the scene's main thread stuck waiting for an answer from an external service. The rest of your code needs to keep running, building the next frame, while the task is idle waiting for a response.

因为场景需要每秒更新多次，因此不能让场景的主线程等待来自外部服务的回答。当任务处于等待响应的空闲状态时，其余代码需要继续运行，并构建下一个桢。

[ ASYNC DIAGRAMS]

## The executeTask function
## executeTask 函数

The `executeTask` function executes a lambda function asynchronously, in a separate thread from the scene's main thread.

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

The example above executes a function that includes a `fetch()` operation to retrieve data from an external API. The rest of the code in the scene will keep being executed while this asynchronous process takes place.

上面的示例执行一个函数，该函数包含一个 `fetch()` 操作，用于从外部 API 检索数据。在执行此异步过程时，场景中的其余代码将继续执行。

<!--
Note that there are two `await` statements here, one to get data from
-->

> Note: Keep in mind that several frames of your scene might be rendered before the task finishes executing. Make sure your scene's code is flexible enough to handle the in-between scenarios while the asynchronous task is being completed.

> 注意：请记住，在任务完成执行之前，可能已经渲染了几帧场景。确保场景的代码足够灵活，以便在异步任务完成时处理中间场景。

## OnClick functions
## OnClick 函数

You can add an `OnCLick` component to any entity to trigger an asynchronous lambda function every time that entity is clicked.

您可以向任何实体添加一个 `OnCLick` 组件，以便在每次单击该实体时触发异步 lambda 函数。

```ts
myEntity.add(
  new OnClick(e => {
    log("clicked on the entity", e)
  })
)
```

## Subscribe a listener
## 订阅监听器

Another way to run asynchronous code is to instance an event listener. Event listeners trigger the running of an asynchronous lambda function every time that a given event occurs.

运行异步代码的另一种方法是实例化事件监听器。每次发生给定事件时，事件侦听器都会触发异步 lambda 函数的运行。


```ts
//Instance the Input object
const input = Input.instance

// Subscribe to an event
input.subscribe("BUTTON_A_DOWN", e => {
  log("pointerUp works", e)
})
```

The example above runs a function every time that the button _A_ is pressed down.

上面的示例在每次按下按钮 _A_ 时，都会运行函数。

<!-- If multiple events in rapid succession, do we get multiple independent threads? -->
