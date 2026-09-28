---
params: ""
returns: ""
---

# client:close

**方法名称：** 关闭当前 RPC 客户端连接。

**语法：** `client:close()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 关闭当前客户端对象。 |

| 返回值 | 说明 |
|---|---|
| 无 | 不返回成功值，不能用 `assert(client:close())` 判断。 |

**详细说明：**

允许重复调用。关闭会取消该连接上正在等待的调用，不会关闭远端服务，也不会影响其他客户端。远端可能已经执行请求，关闭连接不代表业务回滚。再次调用需要新建 `rpc.connect` 对象。

**使用示例：**

```lua
local rpc = require("rpc")
local client = assert(rpc.connect("xiaoyv.demo"))
local reply, err = client:call("ping")
client:close()
assert(reply, err)
print(reply)
```
