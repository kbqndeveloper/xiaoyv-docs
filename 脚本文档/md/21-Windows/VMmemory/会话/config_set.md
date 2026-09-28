---
params: "option: integer|string|userdata, value: integer|string|userdata"
returns: "true, nil, status 或 nil, error, status"
---

**方法名称：** 修改开放的底层配置。

**语法：** `session:config_set(option, value)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `option` | `integer\|string\|userdata` | 是 | 下表的配置编号。 |
| `value` | `integer\|string\|userdata` | 是 | 无符号配置值；缓存周期必须为 `1..0xffffffff`。 |

| 返回值 | 说明 |
|---|---|
| `true, nil, vm.OK` | 设置成功。 |
| `nil, error, status` | 配置未开放、参数无效或底层拒绝。 |

| 编号（十六进制） | 含义 |
|---|---|
| `0x2000000400000000` | 基础 tick 周期，毫秒。 |
| `0x2000000500000000` | 读缓存有效期，tick 数。 |
| `0x2000000600000000` | 地址转换缓存有效期，tick 数。 |
| `0x2000000700000000` | 进程部分刷新周期，tick 数。 |
| `0x2000000800000000` | 进程完整刷新周期，tick 数。 |
| `0x2001ffff00000000` | 刷新所有缓存。 |
| `0x2001100000000000` | 刷新数据缓存。 |
| `0x2001080000000000` | 刷新地址转换缓存。 |
| `0x2001040000000000` | 快速刷新。 |
| `0x2001000100000000` | 中频刷新，含完整进程刷新。 |
| `0x2001001000000000` | 慢速刷新。 |

上述刷新操作传 `1` 即可。进程 DTB / 单进程刷新另支持底层对应编号与 PID 的组合；不是常规读写的前置步骤。

**使用示例：**

```lua
-- 只在确有需要时调整缓存周期；读取原值后再决定新值。
local option = "0x2000000400000000"
local previous = assert(session:config_get(option))
assert(session:config_set(option, 100))
assert(session:config_set(option, previous))
```

**详细说明：**

只开放缓存周期、刷新和进程 DTB，不透传任意生命周期或原生指针配置，也不能把只读会话改成可写。普通刷新优先使用 [refresh](refresh.md)。涉及 DTB 的配置仅适合已掌握目标页表的调用方。
