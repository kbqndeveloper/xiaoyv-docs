---
params: ""
returns: "string[]"
---

**方法名称：** 获取游标的列名。

**语法：** `cursor:columns()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 不需要参数。 |

| 返回值 | 说明 |
|---|---|
| `string[]` | 按结果集顺序排列的 1-based 列名数组。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER, name TEXT)"))

local cursor = assert(conn:query("SELECT id, name FROM user"))
local columns = cursor:columns()
print(columns[1], columns[2])
cursor:close()
conn:close()
```

**详细说明：**

列名在游标创建时读取，调用 `columns` 不会消耗当前行。即使结果集为空，也会返回查询列定义。
