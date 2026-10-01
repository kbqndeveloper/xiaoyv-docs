---
params: "name: string, ...: RPC value"
returns: "... | nil, string?"
---

# client:call

**方法名称：** 调用服务端已注册的函数并等待结果。

**语法：** `client:call(name, ...)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `name` | `string` | 是 | 服务端导出的方法名称，1～128 字节。 |
| `...` | RPC 数据 | 否 | 按顺序传给回调的参数，见[数据规则](概述.md#数据规则)。 |

| 返回值 | 说明 |
|---|---|
| `...` | 服务函数的全部返回值，包括中间和末尾的 `nil`。 |
| `nil, error` | 未注册方法、回调报错、数据不支持或通信失败。 |

**详细说明：**

没有单次调用超时参数；`rpc.connect` 的超时不作用于这里。同一连接上的并发调用会串行处理，耗时函数会阻塞该服务后续请求；不要在回调中同步调用自身或形成循环等待。

停止在途调用会关闭该连接，需要重新 `rpc.connect`；不会自动重放或撤销远端业务。参数类型或方法名长度错误会产生 Lua 错误。服务函数主动返回 `nil, error` 与通信层报错形状相同，建议业务成功时返回明确的非 nil 结果。

**使用示例：**

```lua
local rpc = require("rpc")
local client = assert(rpc.connect("xiaoyv.demo"))
local sum, err = client:call("add", 12, 30)
if err then print("调用失败：", err) else print("结果：", sum) end
client:close()
```
