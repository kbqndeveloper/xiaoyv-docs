---
params: "pid: integer, address: integer|string|userdata"
returns: "physical, nil, status 或 nil, error, status"
---

**方法名称：** 把虚拟地址转换为物理地址。

**语法：** `session:translate(pid, address)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 内的进程 PID，不是宿主 PID。 |
| `address` | `integer\|string\|userdata` | 是 | 目标地址，支持 [address](../address.md) 的数值格式。 |

| 返回值 | 说明 |
|---|---|
| `physical, nil, vm.OK` | Guest 物理地址对象。 |
| `nil, error, status` | 地址转换失败。 |

**使用示例：**

```lua
local physical = assert(session:translate(pid, address))
local bytes = assert(session:read(vm.PHYSICAL_PID, physical, 16))
```

**详细说明：**

地址转换基于当前页表，不产生固定映射或锁定目标页；目标进程变化后需要重新查询。物理地址同样不是宿主可直接解引用的指针。
