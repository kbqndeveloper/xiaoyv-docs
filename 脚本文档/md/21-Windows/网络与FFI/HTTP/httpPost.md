---
params: "url: string, postdata: string, timeout: number? 或 header: table/string, header: table/string?"
returns: "string, integer, table, string 或 nil, string"
---

**方法名称：** 发送同步 HTTP POST 请求。

**语法：** `httpPost(url, postdata[, timeout[, header]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `url` | `string` | 是 | `http://` 或 `https://` 地址。 |
| `postdata` | `string` | 是 | 要发送的正文，表单或 JSON 编码由调用方负责。 |
| `timeout` | `number?` | 否 | 超时秒数，默认 `30`；此位置也可直接传请求头。 |
| `header` | `table 或 string?` | 否 | 请求头 table 或原始请求头字符串。 |

| 返回值 | 说明 |
|---|---|
| `string, integer, table, string` | 依次返回正文、HTTP 状态码、响应头 table 和状态消息。 |
| `nil, string` | 请求无法完成时返回错误文本。 |

**详细说明：**

未提供 `Content-Type` 时默认使用 `application/x-www-form-urlencoded; charset=utf-8`。发送 JSON 等其他正文时，应在请求头中显式设置对应类型。

```lua
local body, code = httpPost(
    "https://example.com/api",
    '{"name":"xiaoyu"}',
    10,
    { ["Content-Type"] = "application/json" }
)
assert(body, code)
print(code, body)
```
