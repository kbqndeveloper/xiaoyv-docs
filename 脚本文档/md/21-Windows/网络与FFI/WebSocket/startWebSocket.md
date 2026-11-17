---
params: "url: string, onOpened?: fun(handle: integer), onClosed?: fun(handle: integer, code: integer, reason: string), onError?: fun(handle: integer, message: string, code: integer), onRecv?: fun(handle: integer, data: string)"
returns: "handle: integer"
---

**方法名称：** 创建 WebSocket 连接并注册事件回调。

**语法：** `startWebSocket(url[, onOpened[, onClosed[, onError[, onRecv]]]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `url` | `string` | 是 | `ws://` 或 `wss://` 地址。 |
| `onOpened` | `fun(handle: integer)\|nil` | 否 | 握手成功后调用。 |
| `onClosed` | `fun(handle: integer, code: integer, reason: string)\|nil` | 否 | 连接结束后调用一次。 |
| `onError` | `fun(handle: integer, message: string, code: integer)\|nil` | 否 | 连接失败或异常断开时调用，随后还会调用 `onClosed`。 |
| `onRecv` | `fun(handle: integer, data: string)\|nil` | 否 | 每收到一条完整的文本或二进制消息调用一次。 |

| 返回值 | 说明 |
|---|---|
| `integer` | 连接句柄，用于发送和关闭。 |

**详细说明：**

函数立即返回句柄，不等待连接完成；连接成功或失败都通过回调通知。只关心后面的回调时，前面的回调传 `nil` 占位。

`url` 不是 `ws://` 或 `wss://` 地址、回调不是函数、Lua 子线程已达到 10 个上限时，直接抛出 Lua 错误，不触发回调。事件规则、超时和名额说明见 [WebSocket 概述](概述.md)。

```lua
local function connect()
    return startWebSocket("ws://127.0.0.1:8080/ws",
        function(h) print("已连接", h) end,
        function(h, code, reason)
            print("已关闭", code, reason)
            sleep(3000)
            connect() -- 断开后 3 秒重连
        end,
        function(h, message, code) print("出错", message, code) end,
        function(h, data) print("收到", data) end)
end

connect()
while true do
    sleep(1000)
end
```
