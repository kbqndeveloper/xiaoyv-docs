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

子线程与主任务共享全局变量，每个窗口最多同时运行 10 个子线程。通过返回的线程对象停止该任务，详见 [stopThread](stopThread.md)。

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
