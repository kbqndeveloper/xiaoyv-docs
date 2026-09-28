---
params: "pid: integer, address: integer|string|userdata, encoding: string|integer, text: string, flags?: integer"
returns: "result, nil, status 或 nil, error, status, detail"
---

**方法名称：** 写入零结尾文本。

**语法：** `session:write_string(pid, address, encoding, text[, flags])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 进程 PID；物理内存访问用 `vm.PHYSICAL_PID`。 |
| `address` | `integer\|string\|userdata` | 是 | 目标地址，支持 [address](../address.md) 的数值格式。 |
| `encoding` | `string\|integer` | 是 | `"utf8"` / `"utf16le"`，或 `vm.TEXT_UTF8` / `vm.TEXT_UTF16LE`。 |
| `text` | `string` | 是 | UTF-8 文本，不能内嵌零字符；可为空字符串。 |
| `flags` | `integer` | 否 | 默认 `0`；`vm.WRITE_VERIFY` 表示写后回读核对。 |

| 返回值 | 说明 |
|---|---|
| `result, nil, vm.OK` | `result.completed` 为写入字节数，`result.flags` 为结果标志。 |
| `nil, error, status, detail` | 失败或部分写入；`detail` 存在时同样有 `completed`、`flags`，不表示已回滚。 |

**使用示例：**

```lua
-- 需要 write=true，且目标缓冲区有足够空间。
local result = assert(session:write_string(pid, address, "utf16le", "小鱼", vm.WRITE_VERIFY))
print(result.completed) -- 6：两个 UTF-16 字符，加结尾零字符
```

**详细说明：**

自动转换到所选目标编码并追加终止符；`completed` 包含终止符字节。插件不知道目标缓冲区容量，调用方须确保空间足够。编码后的写入长度受单次 `16 MiB` 限制，部分失败不回滚。
