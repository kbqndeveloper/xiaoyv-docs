---
params: "value: integer|string|userdata"
returns: "address 或 nil, error, status"
---

**方法名称：** 构造完整 64 位地址。

**语法：**

```lua
vm.address(value)
vm.u64(value)
```

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `value` | `integer\|string\|userdata` | 是 | 非负 Lua 整数、十进制/`0x` 十六进制字符串或已有地址对象。 |

| 返回值 | 说明 |
|---|---|
| `address` | 保存完整 uint64 的 userdata。 |
| `nil, error, status` | 格式、符号或范围不正确。 |

**使用示例：**

```lua
local vm = require("VMmemory")
local base = assert(vm.address("0xffff800000001000"))
local next = base + 32
print(tostring(next)) -- 0xffff800000001020
print(next - base)   -- 0x20，结果仍是地址对象
```

**详细说明：**

范围为 `0` 至 `0xffffffffffffffff`。不接受浮点数或负整数；高位地址请传字符串，不要先 `tonumber`。

支持地址加有符号整数偏移、无符号减法及地址间比较。加法溢出或减法下溢会抛出 Lua 错误，不返回 `nil, error`。`tostring` 返回十六进制文本。不要用 `address == 0` 判断零地址，应与 `vm.address(0)` 比较。

这是目标地址的数值对象，不持有对应目标内存。需要经跨 Worker 通信传递时，可自行用 `tostring` 序列化，再在接收端重新构造。
