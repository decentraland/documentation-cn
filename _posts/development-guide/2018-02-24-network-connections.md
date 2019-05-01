---
date: 2018-02-24
title: 网络连接
description: 场景如何调用外部服务器或 API
categories:
  - development-guide
type: Document
set: development-guide
set_order: 29
---

场景可以使用 API 公开提供的外部服务，您可以使用它来获取更新的价格数据、天气数据或任何 API 公开的其他类型的信息。

还可以架设自己的外部服务器来为场景服务，并在用户之间同步数据。这可以通过一个提供 REST API 或 WebSockets 服务的服务器实现。

## 调用 REST API

场景的代码可以发送调用 REST API 来获取数据。

由于服务器可能需要一些时间来发送响应，因此必须用[异步函数]({{ site.baseurl }}{% post_url /development-guide/2018-02-25-async-functions %})来调用，使用 `executeTask()`。

```ts
executeTask(async () => {
  try {
    let response = await fetch(callUrl)
    let json = await response.json()
    log(json)
  } catch {
    log("failed to reach URL")
  }
})
```

fetch 命令还可以包含第二个可选参数，该参数将 headers、HTTP method 和 HTTP body 捆绑到一个对象中。

```ts
executeTask(async () => {
  try {
    let response = await fetch(callUrl, {
      headers: { "Content-Type": "application/json" },
      method: "POST",
      body: JSON.stringify(myBody)
    })
    let json = await response.json()
    log(json)
  } catch {
    log("failed to reach URL")
  }
})
```

> 注意: body 必须以 stringified JSON 对象发送。

fetch 命令返回一个带有以下数据的 `response` 对象:

- `headers`:  `ReadOnlyHeaders` 对象. 使用 `get()` 方法获取特定 header 属性, 或使用 `has()` 方法检查属性是否存在。
- `ok`: Boolean
- `redirected`: Boolean
- `status`: 状态码
- `statusText`: 状态信息
- `type`: 以下值之一: _basic_, _cors_, _default_, _error_, _opaque_, _opaqueredirect_
- `url`: URL that was sent

- `json()`: 获取 json 格式的 body.
- `text()`: 获取 text 格式的 body.

> 注意:`json()` 和 `text()`是互斥的。如果您以两种格式之一获取响应主体，那么您就不能再从 `response` 对象获取另一种格式。

## 使用 WebSockets

您还可以向 WebSocket 服务器发送数据或获取数据。

```ts
var socket = new WebSocket("url")

socket.onmessage = function(event) {
  log("WebSocket message received:", event)
}
```

WebSockets 的语法与 JavaScript 本地实现的语法没有什么不同。有关如何通过 websocket 捕获和发送消息的详细信息，请参阅[Mozilla Web API](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)的文档。

> 注意: Decentraland SDK 不支持导入外部库，所以您不能使用 WebSocket 库。