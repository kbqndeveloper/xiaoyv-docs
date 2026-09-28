---
params: "callback: function, ...: any"
returns: "无"
---

**方法名称：** 启动 Lua 子线程。

**语法：** `beginThread(callback, ...)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `callback` | `function` | 是 | 子线程入口函数。 |
| `...` | `any` | 否 | 原样传给 `callback` 的附加参数。 |

| 返回值 | 说明 |
|---|---|
| 无 | 只启动子线程，不返回线程对象。 |

**详细说明：**

`callback` 在同一 Worker 的 Lua VM 中执行。也可通过 `thread.beginThread` 使用同一入口。一个 Worker 最多同时运行 10 个用户子线程；子线程错误只写入日志并结束该子线程。需要主动停止或等待某个线程时，请使用 [newThread](newThread.md)。

```lua
local total = 0
beginThread(function(step, count)
    for _ = 1, count do
        total = total + step
        sleep(20)
    end
end, 2, 10)
```
