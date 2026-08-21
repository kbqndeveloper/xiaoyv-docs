---
params: "callback: function, ...: any"
returns: "thread userdata"
---

**方法名称：** 启动并返回 Lua 子线程对象。

**语法：** `thread.newThread(callback, ...)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `callback` | `function` | 是 | 子线程入口函数。 |
| `...` | `any` | 否 | 原样传给 `callback` 的附加参数。 |

| 返回值 | 说明 |
|---|---|
| `thread userdata` | 线程句柄，可调用 `:stopThread()` 请求停止。 |

**详细说明：**

线程对象只用于控制对应子线程，不代表独立 Lua VM；共享数据依然位于当前 Worker 的 Lua 状态中。线程自然结束后，
对象仍可安全回收。

```lua
local threadObject = thread.newThread(function(name)
    for index = 1, 3 do
        print(name, index)
        sleep(100)
    end
end, "worker")

sleep(500)
threadObject:stopThread()
```
