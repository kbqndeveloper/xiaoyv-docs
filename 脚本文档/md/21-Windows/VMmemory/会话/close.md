---
params: ""
returns: "true, nil, status 或 nil, error, status"
---

**方法名称：** 关闭内存会话。

**语法：** `session:close()`

**参数说明：**

无。

| 返回值 | 说明 |
|---|---|
| `true, nil, vm.OK` | 操作成功。 |
| `nil, error, status` | 操作未完成。 |

**使用示例：**

```lua
assert(session:close())
assert(session:close()) -- 同一个 Lua 会话可以重复关闭
```

**详细说明：**

关闭后会话不可再访问内存；重复关闭同一 Lua 对象视为成功。关闭会请求取消已有操作，并等待占用该会话的底层调用退出，因此第三方阻塞时可能需要等待。

Lua `<close>` 或 GC 也会释放会话。若要求确定的释放时间，主动关闭或使用 `<close>`，不要依赖 GC 时机。
