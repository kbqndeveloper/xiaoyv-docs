---
params: ""
returns: ""
---

# server:close

**方法名称：** 关闭 RPC 服务及其客户端连接。

**语法：** `server:close()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 关闭当前服务对象。 |

| 返回值 | 说明 |
|---|---|
| 无 | 不返回成功值，不能用 `assert(server:close())` 判断。 |

**详细说明：**

允许重复调用。关闭监听、连接和未处理请求，使 `server:run()` 结束；不承诺处理完队列再退出。不要在回调中关闭服务后，还依赖该回调的最终结果能返回给客户端。

这不会强制终止已经在执行的 native 函数，也不会结束 Worker 进程。关闭后的对象不能重新启动。

**使用示例：**

```lua
local rpc = require("rpc")
local server = assert(rpc.listen("xiaoyv.demo"))
server:close() -- 尚未 run 的服务也可以直接关闭
```
