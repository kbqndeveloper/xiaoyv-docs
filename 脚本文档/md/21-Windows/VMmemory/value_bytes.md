---
params: "type: string|integer, value: number|string|userdata"
returns: "data, nil, status 或 nil, error, status"
---

**方法名称：** 把数值转换为小端字节。

**语法：** `vm.value_bytes(type, value)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `type` | `string\|integer` | 是 | `u8/u16/u32/u64/i8/i16/i32/i64/f32/f64` 或对应常量。 |
| `value` | `number\|string\|userdata` | 是 | 待编码数值；无符号值也支持整数文本/地址对象。 |

| 返回值 | 说明 |
|---|---|
| `data, nil, vm.OK` | 按类型宽度编码的二进制字符串。 |
| `nil, error, status` | 类型或范围错误。 |

**使用示例：**

```lua
local vm = require("VMmemory")
local pattern = assert(vm.value_bytes("u32", 0x12345678))
print(pattern:byte(1, 4)) -- 120 86 52 18
```

**详细说明：**

纯字节转换，不访问虚拟机；可用于 [search](会话/search.md) 的 `pattern` 或批量写入的 `data`。有符号类型使用 Lua 整数，浮点类型使用 number。常量见[常量与返回状态](常量与返回状态.md)。
