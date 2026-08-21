---
params: "无"
returns: "无"
---

**方法名称：** 请求子线程停止并等待其退出。

**语法：** `thread:stopThread()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | — | — | 在 `thread.newThread` 返回的对象上调用。 |

| 返回值 | 说明 |
|---|---|
| 无 | 子线程停止或已经自然结束后返回。 |

**详细说明：**

调用后运行时设置停止标志并等待目标线程退出。目标线程需要经过 `sleep`、HTTP 或字节码检查点
才能响应；正在执行的 native 调用不会被强行中断。重复调用已结束的线程对象是安全的，在目标线程
自身调用该方法会抛出错误。

```lua
local threadObject = thread.newThread(function()
    while true do
        print("running")
        sleep(50)
    end
end)

sleep(200)
threadObject:stopThread()
print("线程已退出")
```
