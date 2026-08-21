---
params: "ctype: string 或 ctype；value/init: 可选 Lua 值或 cdata"
returns: "ctype、cdata、number、boolean、string 或 nil"
---

**方法名称：** 创建、转换和检查 C 数据与类型。

**语法：**

```lua
ffi.new(ctype[, init...])
ffi.cast(ctype, value)
ffi.typeof(ctype)
ffi.sizeof(ctypeOrCdata[, length])
```

**参数说明：**

| 参数 | 类型 | 说明 |
|---|---|---|
| `ctype` / `ctypeOrCdata` | `string`、ctype 或 cdata | 已声明的 C 类型、声明字符串或实际 cdata。 |
| `init` | 可选 | 初始值；结构体字段和数组可以逐项初始化。 |
| `value` | Lua 值或 cdata | 要转换为目标 C 类型的值。 |
| `length` | `integer`（可选） | 变长数组或按指定长度计算时使用的元素数量。 |

| 返回值 | 说明 |
|---|---|
| `cdata` | `ffi.new` 分配的数据或 `ffi.cast` 转换结果。 |
| `ctype` | `ffi.typeof` 返回的可复用类型对象。 |
| `integer` | `ffi.sizeof` 等布局查询函数返回的大小或偏移。 |
| `boolean` | `ffi.istype` 等类型判断函数的结果。 |
| `string` / `number` / `nil` | 字符串、数字和空指针相关转换结果。 |

**详细说明：**

常用配套函数包括 `ffi.alignof`、`ffi.offsetof`、`ffi.istype`、`ffi.addressof`、`ffi.nullptr`、
`ffi.string`、`ffi.copy`、`ffi.fill` 和 `ffi.tonumber`。结构体、指针和数组布局由当前 Windows ABI
决定，应以 `ffi.sizeof`、`ffi.alignof` 和目标 DLL 的声明为准。不得把任意整数或项目 userdata
直接当作有效 C 指针。

```lua
local ffi = require("ffi")
ffi.cdef[[
    typedef struct { int x; int y; } xiaoyv_point;
]]

local point = ffi.new("xiaoyv_point")
point.x, point.y = 12, 34
local buffer = ffi.new("char[32]")
ffi.fill(buffer, ffi.sizeof(buffer), 0)
ffi.copy(buffer, "xiaoyv")
print(point.x, point.y, ffi.string(buffer))
```
