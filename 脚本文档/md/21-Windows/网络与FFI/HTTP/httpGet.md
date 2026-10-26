---
params: "url: string, timeout: number? 或 header: table/string, header: table/string?"
returns: "body: string, statusCode: integer, headers: table, statusMessage: string 或 nil, error: string"
---

**方法名称：** 发送同步 HTTP GET 请求。

**语法：** `httpGet(url[, timeout[, header]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `url` | `string` | 是 | `http://` 或 `https://` 地址。 |
| `timeout` | `number?` | 否 | 超时秒数，默认 `30`；此位置也可直接传请求头。 |
| `header` | `table 或 string?` | 否 | 请求头 table 或原始请求头字符串。 |

| 返回值 | 说明 |
|---|---|
| `string, integer, table, string` | 依次返回正文、HTTP 状态码、响应头 table 和状态消息。 |
| `nil, string` | 请求无法完成时返回错误文本。 |

**详细说明：**

HTTP 状态码不是 Lua 调用失败；例如 404 仍会返回正文和 `404`。只有 URL 无效、连接失败、超时、响应读取失败或响应体超过 64 MiB 时才返回 `nil, error`。

```lua
local body, code, headers, message = httpGet("https://example.com", 10, {
    ["Accept"] = "text/html",
})
assert(body, message)
print(code, message, headers["Content-Type"])
```
