---
params: "name: string, data?: JSON value, timeoutMs?: integer"
returns: "any | nil, string?"
---

# ui.call

**方法名称：** 调用 Windows 中控页面注册的函数。

**语法：** `ui.call(name, data, timeoutMs)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `name` | `string` | 是 | 页面通过 `ui.expose` 注册的函数名，不能为空，最长 128 字节。 |
| `data` | JSON 数据 | 否 | 传给页面回调的参数；省略或传 `nil` 时为 JSON `null`。 |
| `timeoutMs` | `integer` | 否 | 等待毫秒数，范围 `0～60000`，默认 `5000`；`0` 会按最短 1 毫秒处理。 |

| 返回值 | 说明 |
|---|---|
| `value, nil` | 页面回调成功完成；`value` 是回调返回的 JSON 数据，最外层 JSON `null` 为 `nil`。 |
| `nil, error` | 页面未就绪、函数未注册、数据无效、调用超时或页面回调失败。 |
| `nil, nil` | 原生中控模式下正常忽略页面调用；HTML 回调返回 `null` 时也会得到此结果。 |

**详细说明：**

页面先用 [ui.expose](JavaScript接口.md#与-lua-交互) 注册同名函数；回调收到 `(data, {workerId})`，可返回值或 Promise。传参和返回值见[交互数据](交互数据.md)。

超时不会取消页面中已经执行的操作；写入、提交等操作不要直接重试。未连接中控时返回 `nil, error`；参数类型或超时范围错误会抛出 Lua 错误。

**使用示例：**

```lua
-- 默认 HTML 已注册 getSettings 和 setStatus
local settings, err = ui.call("getSettings")
assert(not err, err)
print("账号：", settings and settings.account or "")

local _, statusErr = ui.call("setStatus", "采集完成")
assert(not statusErr, statusErr)
```
