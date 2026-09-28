---
params: "pid: integer, requests: table, flags?: integer"
returns: "results, nil, status 或 nil, error, status, detail"
---

**方法名称：** 批量读取多个地址。

**语法：** `session:read_batch(pid, requests[, flags])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 进程 PID；物理内存访问用 `vm.PHYSICAL_PID`。 |
| `requests` | `table` | 是 | 连续数组，每项为 `{address = 地址, size = 字节数}`。 |
| `flags` | `integer` | 否 | 默认 `0`，绕过数据缓存；读取标志见[常量页](../常量与返回状态.md)。 |

| 返回值 | 说明 |
|---|---|
| `results, nil, vm.OK` | 所有请求完整成功，结果顺序与请求相同。 |
| `nil, error, status, detail` | 部分或全部失败；有结果详情时 `detail` 为逐项数组。 |

| 每项字段 | 说明 |
|---|---|
| `address` | 请求地址对象。 |
| `status` | 该项状态码。 |
| `completed` | 实际读取字节总数，不保证为连续前缀。 |
| `data` | 仅完整成功项有此二进制字符串。 |

**使用示例：**

```lua
local results, err, status, detail = session:read_batch(pid, {
    {address = address, size = 4},
    {address = address + 16, size = 8},
})
if not results then print(err, status) end
for _, item in ipairs(results or detail or {}) do
    if item.status == vm.OK then
        print(tostring(item.address), item.data)
    else
        print(tostring(item.address), item.status, item.completed)
    end
end
```

**详细说明：**

请求必须是连续数组，共 `1..4096` 项；每项 `1` 至 `16 MiB`，总量不超过 `256 MiB`。

使用底层批量读取，不保证不同地址的数据来自同一时刻。单项只有完整成功才有 `data`，不要根据 `completed` 自行拼接失败项。
