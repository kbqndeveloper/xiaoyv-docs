---
params: ""
returns: "Lua 迭代器"
---

**方法名称：** 创建游标行迭代器。

**语法：** `cursor:rows()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 不需要参数。 |

| 返回值 | 说明 |
|---|---|
| 迭代器 | 用于 `for row in cursor:rows() do`，每次返回一行。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER, name TEXT)"))
assert(conn:exec("INSERT INTO user VALUES(?, ?)", 1, "小鱼"))

local cursor = assert(conn:query("SELECT id, name FROM user"))
for row in cursor:rows() do
    print(row.id, row.name)
end
cursor:close()
conn:close()
```

**详细说明：**

迭代器读完结果后自动结束并释放查询状态。迭代过程中发生数据库读取错误会抛出 Lua 错误；需要以 `nil, error` 形式处理时改用 [cursor:fetch](fetch.md)。
