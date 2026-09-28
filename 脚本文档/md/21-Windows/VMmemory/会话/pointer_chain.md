---
params: "pid: integer, start: integer|string|userdata, pointer_bits: integer, offsets: table, flags?: integer"
returns: "address, nil, status 或 nil, error, status"
---

**方法名称：** 按指针链计算目标地址。

**语法：** `session:pointer_chain(pid, start, pointer_bits, offsets[, flags])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 进程 PID；物理内存访问用 `vm.PHYSICAL_PID`。 |
| `start` | `integer\|string\|userdata` | 是 | 第一次读取指针的位置。 |
| `pointer_bits` | `integer` | 是 | Guest 指针位宽，`32` 或 `64`。 |
| `offsets` | `table` | 是 | 连续的有符号整数偏移数组，最多 64 层；可为空。 |
| `flags` | `integer` | 否 | 默认 `0`，绕过数据缓存；读取标志见[常量页](../常量与返回状态.md)。 |

| 返回值 | 说明 |
|---|---|
| `address, nil, vm.OK` | 最终地址对象。 |
| `nil, error, status` | 空指针、读取失败、溢出或参数错误。 |

**使用示例：**

```lua
local image = assert(session:module(pid))
local target = assert(session:pointer_chain(pid, image.base + 0x100, 64, {0x20, 0x8}))
local value = assert(session:read_value(pid, target, "u32"))
```

**详细说明：**

每层顺序是 `当前地址 = 读取指针(当前地址) + 偏移`，不是先加偏移再读取。上例计算 `read_pointer(read_pointer(image.base + 0x100) + 0x20) + 0x8`，随后才读取最终数值。

空偏移数组返回起始地址。模块内初始偏移由调用方先加到 `start`；不要因为 Worker 是 x64 就给 32 位 Guest 进程传 `64`。
