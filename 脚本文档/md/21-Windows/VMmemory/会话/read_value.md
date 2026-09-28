---
params: "pid: integer, address: integer|string|userdata, type: string|integer, flags?: integer"
returns: "value, nil, status 或 nil, error, status"
---

**方法名称：** 读取一个数值。

**语法：** `session:read_value(pid, address, type[, flags])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 进程 PID；物理内存访问用 `vm.PHYSICAL_PID`。 |
| `address` | `integer\|string\|userdata` | 是 | 目标地址，支持 [address](../address.md) 的数值格式。 |
| `type` | `string\|integer` | 是 | `u8/u16/u32/u64/i8/i16/i32/i64/f32/f64` 或 [对应常量](../常量与返回状态.md)。 |
| `flags` | `integer` | 否 | 默认 `0`，绕过数据缓存；读取标志见[常量页](../常量与返回状态.md)。 |

| 返回值 | 说明 |
|---|---|
| `value, nil, vm.OK` | 整数、浮点数；超出 Lua 有符号整数上限的 `u64` 返回地址对象。 |
| `nil, error, status` | 读取不完整或类型错误，不能使用失败详情中的初始数值。 |

**使用示例：**

```lua
local number = assert(session:read_value(pid, address, "u32"))
local pointer = assert(session:read_value(pid, address, "u64"))
print(number, tostring(pointer))
```

**详细说明：**

按小端格式读取类型所需的完整字节数；部分读取不会被当作有效数值。读取指针时按 Guest 进程位宽选择 `u32` 或 `u64`，不要按宿主 Worker 位宽推断。
