---
params: "name: string, timeoutMs?: integer"
returns: "userdata | nil, string?"
---

# rpc.connect

**方法名称：** 连接本机 RPC 服务。

**语法：** `rpc.connect(name, timeoutMs)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `name` | `string` | 是 | 与服务端 `rpc.listen` 相同的名称，1～96 字节，区分大小写，不能包含零字节。 |
| `timeoutMs` | `integer` | 否 | 连接等待时间，默认 `5000` 毫秒，范围 `0～60000`；`0` 只立即尝试连接。 |

| 返回值 | 说明 |
|---|---|
| `client` | 客户端 userdata。 |
| `nil, error` | 服务不存在、连接超时或连接失败。 |

**详细说明：**

超时参数只限制建立连接，不限制后续 `client:call` 的执行时间。等待会让出 Lua VM；脚本停止仍按运行时控制退出。超时参数类型或范围错误会产生 Lua 错误。断开后需显式新建连接。

**使用示例：**

```lua
local rpc = require("rpc")
local client = assert(rpc.connect("xiaoyv.demo", 5000))
print(assert(client:call("ping")))
client:close()
```
