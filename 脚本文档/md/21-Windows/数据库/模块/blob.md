---
params: "value: string"
returns: "BLOB 绑定值或 nil, error"
---

**方法名称：** 创建 SQLite BLOB 绑定值。

**语法：** `db.blob(value)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `value` | `string` | 是 | 要按原始二进制写入 SQLite 的字符串。 |

| 返回值 | 说明 |
|---|---|
| BLOB userdata | 只能作为 `exec` 或预编译语句的绑定参数使用。 |
| `nil, error` | `value` 不是字符串时返回。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE bytes(value BLOB)"))
assert(conn:exec("INSERT INTO bytes(value) VALUES(?)", db.blob("\0\1\2")))
conn:close()
```

**详细说明：**

普通 Lua 字符串按 `TEXT` 绑定；只有经过 `db.blob` 标记后才按 BLOB 绑定。查询得到的 BLOB 会作为 Lua 二进制字符串返回，不需要再次调用 `db.blob`。
