---
params: "handle: integer, text: string, timeout?: integer"
returns: "ok: boolean"
---

**方法名称：** 向 WebSocket 连接发送一条文本消息。

**语法：** `sendWebSocket(handle, text[, timeout])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `handle` | `integer` | 是 | `startWebSocket` 返回的句柄。 |
| `text` | `string` | 是 | 要发送的文本；其他 Lua 值先转换为字符串。 |
| `timeout` | `integer` | 否 | 最长等待毫秒数，默认 `3000`，从调用开始计时，包含排队时间。必须是正整数。 |

| 返回值 | 说明 |
|---|---|
| `true` | 消息已写出。不代表服务器已处理。 |
| `false` | 句柄无效、连接未就绪或已关闭、发送失败或超时。 |

**详细说明：**

函数等待本次发送完成后才返回，期间不影响其他 Lua 线程。只能在 `onOpened` 之后、连接关闭之前发送；连接中或关闭后调用直接返回 `false`。

同一连接一次只发送一条消息。多个线程同时发送时依次排队，每条消息完整发出、不会交错；同一线程的消息保持顺序，不同线程之间的先后不保证。需要严格顺序时只在一个线程中发送。

超时分两种情况：

| 超时时 | 结果 |
|---|---|
| 还在排队 | 返回 `false`，消息没有发出，连接不受影响，可以重发。 |
| 已经开始发送 | 返回 `false`，连接断开并触发 `onError` 和 `onClosed`；消息可能已部分发出，不要当作未发送。 |

连接断开时，正在排队的发送立即返回 `false`。参数 `timeout` 无效时抛出 Lua 错误。

```lua
local ok = sendWebSocket(handle, '{"type":"ping"}', 1000)
if not ok then
    print("发送失败")
end
```
