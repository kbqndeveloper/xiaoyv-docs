---
params: "callback: function, url: string, timeout: number? 或 header: table/string, header: table/string?"
returns: "thread userdata"
---

**方法名称：** 在线程中发送 HTTP GET 请求。

**语法：** `asynHttpGet(callback, url[, timeout[, header]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `callback` | `function` | 是 | 请求结束后调用 `callback(body, codeOrError)`。 |
| `url` | `string` | 是 | `http://` 或 `https://` 地址。 |
| `timeout` | `number?` | 否 | 超时秒数，默认 `30`；此位置也可直接传请求头。 |
| `header` | `table 或 string?` | 否 | 请求头。 |

| 返回值 | 说明 |
|---|---|
| `thread userdata` | 新建的线程对象，可调用 `:stopThread()`。 |

**详细说明：**

回调在新建的 Worker Lua 子线程中执行。成功时收到 `body, statusCode`，失败时收到 `nil, errorMessage`；异步入口不会把响应头和状态消息作为回调参数传出。

```lua
local thread = asynHttpGet(function(body, codeOrError)
    if body then
        print("HTTP", codeOrError, #body)
    else
        print("请求失败", codeOrError)
    end
end, "https://example.com", 10)
```
