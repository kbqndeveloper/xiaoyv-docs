---
params: "timeoutMs?: integer"
returns: "string?, any? | string?"
---

# ui.receive

**方法名称：** 接收 Windows 中控页面投递的事件。

**语法：** `ui.receive(timeoutMs)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `timeoutMs` | `integer` | 否 | 最长等待毫秒数，范围 `0～60000`，默认 `0`，即非阻塞读取。 |

| 返回值 | 说明 |
|---|---|
| `name, data` | 收到事件；`name` 是事件名，`data` 是页面发送的 JSON 数据。 |
| `nil` | 超时前没有事件。默认调用会立即得到此结果。 |
| `nil, error` | Worker 没有连接中控通道，或收到的数据无效。 |

**详细说明：**

页面通过 `ui.send(workerId, name, data)` 向指定 Worker 投递事件。每次调用只取出一个事件，按进入当前脚本邮箱的顺序返回。邮箱最多等待 256 个事件，并会在脚本停止后清空；页面发送速度可能高于脚本处理速度时，应持续接收并自行合并高频业务数据。

等待期间会让出 Lua VM，并继续响应暂停和停止。页面侧 `ui.send` 成功只表示事件已进入 Worker 邮箱，不表示本函数已经取出事件，更不表示脚本业务已经执行完。

数据类型、大小和 `ui.null` 规则见[中控交互的 JSON 数据规则](../中控交互.md#json-数据规则)。

`timeoutMs` 不是整数或超出范围时会直接产生 Lua 参数错误，不按 `nil, error` 返回。Worker 没有连接中控通道时会立即返回 `nil, error`；普通页面刷新不会把邮箱关闭，等待仍按超时规则结束。原生中控没有网页发送事件，未收到事件时同样按 `timeoutMs` 返回 `nil`，不是立即报错。

**使用示例：**

```lua
while true do
    local name, data = ui.receive(1000)
    if name == "settings" then
        print("账号：", data.account)
    elseif name == nil and data ~= nil then
        print("界面接收失败：", data)
        break
    end
end
```
