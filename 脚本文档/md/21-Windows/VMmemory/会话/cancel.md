---
params: ""
returns: "true, nil, status 或 nil, error, status"
---

**方法名称：** 请求取消当前操作。

**语法：** `session:cancel()`

**参数说明：**

无。

| 返回值 | 说明 |
|---|---|
| `true, nil, vm.OK` | 操作成功。 |
| `nil, error, status` | 操作未完成。 |

**使用示例：**

```lua
-- 由持有同一 session 的另一个 Lua 任务调用。
assert(session:cancel())
```

**详细说明：**

取消正在运行且支持协作停止的搜索、逐页写入等操作，不会关闭会话，之后仍可读写。不保证取消尚未开始的排队调用，也不能中断第三方 DLL 内部的阻塞调用。

当前任务同步执行搜索时，不能在它返回前继续执行下一行 `cancel`；需要由另一个任务操作同一个会话。
