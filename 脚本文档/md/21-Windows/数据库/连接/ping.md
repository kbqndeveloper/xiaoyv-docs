---
params: ""
returns: "true 或 nil, error"
---

**方法名称：** 检测 SQLite 连接是否仍然可用。

**语法：** `connection:ping()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 不需要参数。 |

| 返回值 | 说明 |
|---|---|
| `true` | 连接打开且数据库结构可以读取。 |
| `nil, error` | 连接已关闭、密码错误或数据库读取失败。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = "data/sample.sqlite"}))
assert(conn:ping())
conn:close()
```

**详细说明：**

`ping` 会执行一次轻量的数据库结构读取，适合在长时间空闲后确认连接状态；它不会替代实际 SQL 操作的错误处理。
