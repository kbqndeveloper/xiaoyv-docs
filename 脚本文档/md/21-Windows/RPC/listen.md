---
params: "name: string"
returns: "userdata | nil, string?"
---

# rpc.listen

**方法名称：** 创建本机 RPC 服务。

**语法：** `rpc.listen(name)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `name` | `string` | 是 | 服务名称，1～96 字节，区分大小写，不能包含零字节。 |

| 返回值 | 说明 |
|---|---|
| `server` | 服务端 userdata。 |
| `nil, error` | 名称无效、已被占用或无法创建服务。 |

**详细说明：**

同名服务不能重复监听。服务创建后还需注册函数并调用 `server:run()` 才会执行请求；不需要通过中控注册。参数类型错误会产生 Lua 错误。通用规则见[概述](概述.md)。

**使用示例：**

```lua
local rpc = require("rpc")
local server = assert(rpc.listen("xiaoyv.demo"))
server:export("ping", function() return "pong" end)
assert(server:run())
```
