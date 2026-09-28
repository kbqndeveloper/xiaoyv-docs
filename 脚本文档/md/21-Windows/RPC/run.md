---
params: ""
returns: "boolean | nil, string?"
---

# server:run

**方法名称：** 等待请求并执行已注册函数。

**语法：** `server:run()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 使用已创建并注册好函数的服务对象。 |

| 返回值 | 说明 |
|---|---|
| `true` | 服务被显式关闭，分发循环正常结束。 |
| `nil, error` | 服务已关闭、已有分发循环或运行失败。 |

**详细说明：**

此方法阻塞当前脚本线程；等待时让出 Lua VM，回调按请求入队顺序逐个执行。同一个服务只能有一个分发循环。循环退出时会关闭服务，再次提供服务需新建 `rpc.listen` 对象。

可由同一 Worker 的另一个 Lua 线程调用 `server:close()` 结束等待。中控停止脚本仍按正常停止流程退出，不保证返回 `true` 或 `nil, error`。阻塞中的 native 函数不会被本接口强制中断。

**使用示例：**

```lua
local rpc = require("rpc")
local server = assert(rpc.listen("xiaoyv.demo"))
server:export("ping", function() return "pong" end)
print_user("服务已就绪")
assert(server:run()) -- 通常持续运行，直到停止脚本或关闭服务
```
