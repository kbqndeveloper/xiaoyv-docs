---
params: ""
returns: "message"
---

**方法名称：** 读取当前原生线程最近的底层错误。

**语法：** `vm.last_error()`

**参数说明：**

无。

| 返回值 | 说明 |
|---|---|
| `message` | 错误文本；没有记录时为空字符串。 |

**使用示例：**

```lua
local vm = require("VMmemory")
local session, err, status = vm.open({host_pid = 0})
if not session then
    print(err, status) -- 通常直接使用本次返回的错误即可
    print(vm.last_error())
end
```

**详细说明：**

只读取当前原生线程最近一次 C ABI 调用的错误。后续调用可覆盖或清空它；Lua 参数包装层的错误不保证写入此记录。多线程脚本优先使用每次调用返回的 `error`，不要把此函数当作跨线程错误队列。
