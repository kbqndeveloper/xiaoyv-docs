---
params: "pid: integer, address: integer|string|userdata, encoding: string|integer, max_bytes: integer, flags?: integer"
returns: "text, nil, status 或 nil, error, status, detail"
---

**方法名称：** 读取零结尾文本。

**语法：** `session:read_string(pid, address, encoding, max_bytes[, flags])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 进程 PID；物理内存访问用 `vm.PHYSICAL_PID`。 |
| `address` | `integer\|string\|userdata` | 是 | 目标地址，支持 [address](../address.md) 的数值格式。 |
| `encoding` | `string\|integer` | 是 | `"utf8"` / `"utf16le"`，或 `vm.TEXT_UTF8` / `vm.TEXT_UTF16LE`。 |
| `max_bytes` | `integer` | 是 | 最多读取的目标字节数，`1` 至 `16 MiB`；UTF-16LE 必须为偶数，包含终止符空间。 |
| `flags` | `integer` | 否 | 默认 `0`，绕过数据缓存；读取标志见[常量页](../常量与返回状态.md)。 |

| 返回值 | 说明 |
|---|---|
| `text, nil, vm.OK` | 转换后的 UTF-8 字符串，不含终止零字符。 |
| `nil, error, status, detail` | 读取、编码或终止符检查失败；详情的 `required` 为已计算的 UTF-8 输出容量，含终止符，未计算时可为 `0`。 |

**使用示例：**

```lua
local text = assert(session:read_string(pid, address, "utf16le", 512))
print(text)
```

**详细说明：**

只接受 UTF-8 或 UTF-16LE，不会自动猜测 ANSI。必须在范围内读到终止符；未终止、编码非法或数据不完整均不返回完整文本。需要原始字节时用 [read](read.md)。
