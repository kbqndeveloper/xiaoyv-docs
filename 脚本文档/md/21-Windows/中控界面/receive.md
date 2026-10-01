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

页面用 [ui.send(workerId, name, data)](JavaScript接口.md#与-lua-交互) 发送数据；每次调用按接收顺序取出一个事件。数据格式见[交互数据](交互数据.md)。

最多缓存 256 个事件，脚本停止后清空；需要持续收取时使用循环。发送成功不代表脚本已处理完成。

超时参数无效会抛出 Lua 错误。原生界面下没有页面事件，等待结束后返回 `nil`。

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
