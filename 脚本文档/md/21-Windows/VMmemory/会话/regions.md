---
params: "pid: integer, kind?: integer"
returns: "regions, nil, status 或 nil, error, status"
---

**方法名称：** 枚举内存区域。

**语法：** `session:regions(pid[, kind])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 进程 PID；物理内存访问用 `vm.PHYSICAL_PID`。 |
| `kind` | `integer` | 否 | 默认 `vm.MAP_PTE`；也支持 `vm.MAP_VAD`、`vm.MAP_PHYSICAL`。 |

| 返回值 | 说明 |
|---|---|
| `regions, nil, vm.OK` | 区域数组，字段见下表。 |
| `nil, error, status` | 映射类型与 PID 不匹配或枚举失败。 |

| 字段 | 说明 |
|---|---|
| `base` | 区域起始地址对象。 |
| `size` | 区域字节数。 |
| `kind` | 映射类型。 |
| `native_flags` | PTE 原始页标志或 VAD Protection 索引；大于 Lua 有符号整数范围时为地址对象。 |
| `private_memory` | `0/1`，VAD 私有内存标记。 |
| `description` | 区域说明，UTF-8。 |

**使用示例：**

```lua
for _, region in ipairs(assert(session:regions(pid))) do
    print(tostring(region.base), region.size, region.description)
end
local physical = assert(session:regions(vm.PHYSICAL_PID, vm.MAP_PHYSICAL))
```

**详细说明：**

`MAP_PHYSICAL` 必须配合 `PHYSICAL_PID`；PTE/VAD 使用实际 Guest PID。区域存在不保证其中全部字节都能读取。VAD 的 `native_flags` 是 MemProcFS Protection 索引，不是 Win32 `PAGE_*` 值。
