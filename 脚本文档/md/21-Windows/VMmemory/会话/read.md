---
params: "pid: integer, address: integer|string|userdata, size: integer, flags?: integer"
returns: "data, nil, status 或 nil, error, status, detail"
---

**方法名称：** 读取原始内存。

**语法：** `session:read(pid, address, size[, flags])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 进程 PID；物理内存访问用 `vm.PHYSICAL_PID`。 |
| `address` | `integer\|string\|userdata` | 是 | 目标地址，支持 [address](../address.md) 的数值格式。 |
| `size` | `integer` | 是 | 读取字节数，`1` 至 `16 MiB`。 |
| `flags` | `integer` | 否 | 默认 `0`，绕过数据缓存；读取标志见[常量页](../常量与返回状态.md)。 |

| 返回值 | 说明 |
|---|---|
| `data, nil, vm.OK` | 完整二进制字符串，长度等于 `size`。 |
| `nil, error, status, detail` | 未完整读取；详情存在时包含 `completed`、`flags`。 |

**使用示例：**

```lua
local data, err, status, detail = session:read(pid, address, 16)
if data then
    print(data:byte(1, #data))
else
    print(err, status, detail and detail.completed)
end
```

**详细说明：**

只有完整读取才返回数据；部分读取不提供字符串。`completed` 是实际读取的总字节数，不一定是从起始地址起连续成功的前缀。

默认绕过数据缓存，但仍可能使用地址转换缓存；也不保证读取期间 Guest 内存不变。
