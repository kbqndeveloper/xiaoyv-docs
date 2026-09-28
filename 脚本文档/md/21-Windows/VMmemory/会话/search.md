---
params: "pid: integer, options: table"
returns: "result, nil, status 或 nil, error, status, detail"
---

**方法名称：** 搜索指定内存范围。

**语法：** `session:search(pid, options)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 进程 PID；物理内存访问用 `vm.PHYSICAL_PID`。 |
| `options` | `table` | 是 | 搜索配置，见下表。 |

| 返回值 | 说明 |
|---|---|
| `result, nil, vm.OK` | 完整扫描结果；没匹配时 `count = 0`。 |
| `nil, error, status, detail` | 上限、超时、取消或部分扫描；有效详情包含已有匹配和下一搜索位置。 |

| 配置 | 默认值 | 说明 |
|---|---|---|
| `start` | 必填 | 起始地址。 |
| `length` | 必填 | 搜索字节长度，范围为 `[start, start + length)`。 |
| `pattern` | 必填 | `1..4096` 字节的二进制模式。 |
| `mask` | 省略 | 同长度的按位掩码；省略或空字符串为精确匹配。 |
| `alignment` | `1` | 匹配起始绝对地址的对齐字节数。 |
| `limit` | `256` | 最多保存的地址数，`1..1000000`。 |
| `timeout_ms` | `0` | 超时毫秒数，`0` 不限时。 |
| `read_flags` | `0` | 读取标志。 |

| 结果字段 | 说明 |
|---|---|
| `addresses` | 匹配地址对象数组。 |
| `count` | 已返回匹配数量。 |
| `examined_bytes`、`skipped_bytes` | 已检查和因不可读而跳过的字节统计。 |
| `next_address` | 下一搜索起始地址对象。 |

**使用示例：**

```lua
local result, err, status, detail = session:search(pid, {
    start = address,
    length = 4096,
    pattern = assert(vm.value_bytes("u32", 123)),
    alignment = 4,
    limit = 256,
    timeout_ms = 1000,
})
if not result then print(err, status) end
local found = result or detail
if found then
    for _, hit in ipairs(found.addresses) do print(tostring(hit)) end
    print(found.count, tostring(found.next_address))
end
```

**详细说明：**

`pattern` 是二进制字节，不是 `"12 ?? 34"` 形式的文本。需要通配时使用等长 `mask`；每一位为 `1` 表示必须匹配，为 `0` 表示忽略，例如 `string.char(255, 0, 255)` 忽略第二字节。

不可读区域会跳过并报告 `PARTIAL`，不会把空洞两边的字节拼起来匹配。超时从开始实际搜索后计时，不包括前面的会话锁等待，也不能强制中断底层阻塞读取。

达到上限等情况下续搜时，将 `start` 改成 `next_address`，同时把 `length` 改成“原结束地址减去新 start”，不能保持原长度，否则会扩大搜索范围。检查失败是否有有效详情，再决定续搜。
