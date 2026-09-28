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

HTML 模式下页面需先使用 `ui.expose(name, callback)` 注册函数。回调可以是异步函数，并会额外收到包含来源 `workerId` 的上下文，页面可据此按 Worker 组织或区分数据。

等待期间会让出 Lua VM，并继续响应暂停和停止。超时只结束 Lua 等待，不会取消已经进入页面的回调，也不会撤销回调已经产生的副作用；对写入、提交等操作不要在超时后盲目重试。

数据类型、大小和 `ui.null` 规则见[中控交互的 JSON 数据规则](../中控交互.md#json-数据规则)。本接口只能调用页面明确 `expose` 的名称；回调具体执行什么操作由页面作者决定。

`name` 类型错误、`timeoutMs` 不是整数或超出范围时会直接产生 Lua 参数错误；页面调用失败、超时或数据无法转换等运行期失败才按 `nil, error` 返回。Worker 没有连接中控通道时会立即返回 `nil, error`。

**使用示例：**

```lua
local saved, err = ui.call("saveSettings", {
    account = "demo",
    proxy = ui.null
})
if err then
    print("保存界面设置失败：", err)
else
    print("页面返回：", saved)
end
```
