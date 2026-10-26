---
params: "callback: function, url: string, postdata: string, timeout: number? 或 header: table/string, header: table/string?"
returns: "thread: XiaoYvWindowsThread（userdata）"
---

**方法名称：** 在线程中发送 HTTP POST 请求。

**语法：** `asynHttpPost(callback, url, postdata[, timeout[, header]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `callback` | `function` | 是 | 请求结束后调用 `callback(body, codeOrError)`。 |
| `url` | `string` | 是 | `http://` 或 `https://` 地址。 |
| `postdata` | `string` | 是 | POST 正文。 |
| `timeout` | `number?` | 否 | 超时秒数，默认 `30`；此位置也可直接传请求头。 |
| `header` | `table 或 string?` | 否 | 请求头。 |

| 返回值 | 说明 |
|---|---|
| `thread userdata` | 新建的线程对象，可调用 `:stopThread()`。 |

**详细说明：**

请求语义与 [httpPost](httpPost.md) 一致，只是把同步等待移到新线程。成功回调收到 `body, statusCode`，失败回调收到 `nil, errorMessage`。

```lua
asynHttpPost(function(body, codeOrError)
    print(body and ("HTTP " .. codeOrError) or codeOrError)
end, "https://example.com/api", "key=value", 15)
```
