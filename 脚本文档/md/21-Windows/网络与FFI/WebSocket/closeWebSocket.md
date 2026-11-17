---
params: "handle: integer"
returns: "ok: boolean"
---

**方法名称：** 请求关闭 WebSocket 连接。

**语法：** `closeWebSocket(handle)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `handle` | `integer` | 是 | `startWebSocket` 返回的句柄。 |

| 返回值 | 说明 |
|---|---|
| `true` | 已提交关闭请求。 |
| `false` | 句柄无效、连接已经关闭或正在关闭。 |

**详细说明：**

关闭是异步的：函数发出关闭码 `1000` 后立即返回，之后不能再发送；服务器确认后触发 `onClosed`。服务器 3 秒内没有确认时，报告 `onClosed(handle, 1006, "关闭超时")`。

连接尚未建立时调用会直接取消连接，触发 `onClosed(handle, 1000, "连接已取消")`。可以在回调或其他线程中调用。

```lua
if closeWebSocket(handle) then
    print("已请求关闭")
end
```
