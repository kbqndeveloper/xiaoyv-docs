---
params: "pid: integer, address: integer|string|userdata, data: string, flags?: integer"
returns: "result, nil, status 或 nil, error, status, detail"
---

**方法名称：** 写入原始内存。

**语法：** `session:write(pid, address, data[, flags])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 进程 PID；物理内存访问用 `vm.PHYSICAL_PID`。 |
| `address` | `integer\|string\|userdata` | 是 | 目标地址，支持 [address](../address.md) 的数值格式。 |
| `data` | `string` | 是 | 二进制字符串，`1` 至 `16 MiB`，可含零字节。 |
| `flags` | `integer` | 否 | 默认 `0`；`vm.WRITE_VERIFY` 表示写后回读核对。 |

| 返回值 | 说明 |
|---|---|
| `result, nil, vm.OK` | `result.completed` 为写入字节数，`result.flags` 为结果标志。 |
| `nil, error, status, detail` | 失败或部分写入；`detail` 存在时同样有 `completed`、`flags`，不表示已回滚。 |

**使用示例：**

```lua
-- session 打开时须指定 write=true；address 为已确认可写的目标地址。
local data = assert(vm.value_bytes("u32", 123))
local result, err, status, detail = session:write(pid, address, data, vm.WRITE_VERIFY)
if result then
    print(result.completed)
else
    print(err, status, detail and detail.flags)
end
```

**详细说明：**

按页写入，失败即停，不保证原子性也不自动回滚。即使底层报告失败，也可能已经影响目标内存；`IO_UNCERTAIN_WRITE` 表示存在不确定写入，`IO_VERIFY_FAILED` 表示回读不一致。见[常量页](../常量与返回状态.md)。
