---
params: "name: string, callback: function"
returns: "userdata | nil, string?"
---

# server:export

**方法名称：** 注册客户端可以调用的 Lua 函数。

**语法：** `server:export(name, callback)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `name` | `string` | 是 | 方法名称，1～128 字节。 |
| `callback` | `function` | 是 | 接收客户端参数并返回结果的 Lua 函数，支持多个参数和返回值。 |

| 返回值 | 说明 |
|---|---|
| `server` | 当前服务对象，可继续链式注册。 |
| `nil, error` | 服务已经关闭。 |

**详细说明：**

同名注册会替换旧函数。回调在执行 `server:run()` 的 Lua 线程内串行运行，可在函数内调用本 Worker 已加载的插件。参数类型或名称长度错误会产生 Lua 错误。

回调抛出的普通 Lua 错误会返回给客户端，不会因此结束整个服务。参数和结果必须符合 [RPC 数据规则](概述.md#数据规则)，不能直接返回插件 userdata。

**使用示例：**

```lua
local rpc = require("rpc")
local server = assert(rpc.listen("xiaoyv.demo"))
server:export("add", function(a, b) return a + b end)
      :export("echo", function(...) return ... end)
assert(server:run())
```
