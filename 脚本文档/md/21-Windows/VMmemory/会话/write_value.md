---
params: "pid: integer, address: integer|string|userdata, type: string|integer, value: number|string|userdata, flags?: integer"
returns: "result, nil, status 或 nil, error, status, detail"
---

**方法名称：** 写入一个数值。

**语法：** `session:write_value(pid, address, type, value[, flags])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 进程 PID；物理内存访问用 `vm.PHYSICAL_PID`。 |
| `address` | `integer\|string\|userdata` | 是 | 目标地址，支持 [address](../address.md) 的数值格式。 |
| `type` | `string\|integer` | 是 | `u8/u16/u32/u64/i8/i16/i32/i64/f32/f64` 或 [对应常量](../常量与返回状态.md)。 |
| `value` | `number\|string\|userdata` | 是 | 无符号值可用整数、字符串或地址对象；有符号值用整数，浮点值用 number。 |
| `flags` | `integer` | 否 | 默认 `0`；`vm.WRITE_VERIFY` 表示写后回读核对。 |

| 返回值 | 说明 |
|---|---|
| `result, nil, vm.OK` | `result.completed` 为写入字节数，`result.flags` 为结果标志。 |
| `nil, error, status, detail` | 失败或部分写入；`detail` 存在时同样有 `completed`、`flags`，不表示已回滚。 |

**使用示例：**

```lua
-- 需要 write=true 的会话。
local result = assert(session:write_value(pid, address, "i32", -12, vm.WRITE_VERIFY))
print(result.completed) -- 4
```

**详细说明：**

写入字节数由类型决定，按小端编码并校验数值范围。其写入权限、部分失败和不回滚规则与 [write](write.md) 相同。
