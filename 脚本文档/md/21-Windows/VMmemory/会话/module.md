---
params: "pid: integer, name?: string"
returns: "module, nil, status 或 nil, error, status"
---

**方法名称：** 查询指定进程模块。

**语法：** `session:module(pid[, name])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 内的进程 PID，不是宿主 PID。 |
| `name` | `string` | 否 | 模块名；省略、`nil` 或空字符串取得底层模块列表的第一项。 |

| 返回值 | 说明 |
|---|---|
| `module, nil, vm.OK` | 单个模块表，字段与 [modules](modules.md) 中的每项相同。 |
| `nil, error, status` | 模块不存在或查询失败。 |

**使用示例：**

```lua
local image = assert(session:module(pid, "notepad.exe"))
print(tostring(image.base), image.size)
```

**详细说明：**

省略名称通常取得主映像，但本方法实际遵循底层第一项结果；需要确定模块时显式指定名称。基址加偏移可直接写 `image.base + offset`。
