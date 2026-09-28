---
params: "pid: integer, requests: table, flags?: integer"
returns: "results, nil, status 或 nil, error, status, detail"
---

**方法名称：** 按顺序批量写入。

**语法：** `session:write_batch(pid, requests[, flags])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 进程 PID；物理内存访问用 `vm.PHYSICAL_PID`。 |
| `requests` | `table` | 是 | 连续数组，每项为 `{address = 地址, data = 二进制字符串}`。 |
| `flags` | `integer` | 否 | 默认 `0`；`vm.WRITE_VERIFY` 表示写后回读核对。 |

| 返回值 | 说明 |
|---|---|
| `results, nil, vm.OK` | 所有写入成功的逐项结果数组。 |
| `nil, error, status, detail` | 失败时，详情如存在为逐项结果数组，未执行项标为 `vm.NOT_EXECUTED`。 |

**使用示例：**

```lua
-- 需要 write=true；两个地址范围均须确认可写。
local results, err, status, detail = session:write_batch(pid, {
    {address = address, data = assert(vm.value_bytes("u32", 123))},
    {address = address + 16, data = assert(vm.value_bytes("f32", 1.5))},
}, vm.WRITE_VERIFY)
if not results then
    print(err, status)
    for _, item in ipairs(detail or {}) do
        print(tostring(item.address), item.status, item.completed, item.flags)
    end
end
```

**详细说明：**

请求必须是连续数组，共 `1..4096` 项；每项 `1` 至 `16 MiB`，总量不超过 `256 MiB`。

先校验全部请求参数，再按数组顺序执行，遇到失败停止后续写入。不是事务；前面成功项及当前失败项已产生的修改不回滚。每项包含 `address`、`status`、`completed`、`flags`，含义同 [write](write.md)。
