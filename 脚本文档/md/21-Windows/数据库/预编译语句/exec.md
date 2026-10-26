---
params: "...: any"
returns: "affectedRows: integer, lastInsertId: integer 或 nil, error: string"
---

**方法名称：** 执行预编译语句。

**语法：** `statement:exec(...)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `...` | `any` | 按 SQL 定义 | 按预编译 SQL 的绑定序号传入参数。 |

| 返回值 | 说明 |
|---|---|
| `affectedRows` | 本次执行影响的行数。 |
| `lastInsertId` | 所属连接最近一次插入的 rowid。 |
| `nil, error` | 参数数量、类型或 SQL 执行失败。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER PRIMARY KEY, name TEXT)"))

local stmt = assert(conn:prepare(
    "INSERT INTO user(name) VALUES(?)"))
local affected, id = stmt:exec("小鱼")
print(affected, id)
stmt:close()
conn:close()
```

**详细说明：**

每次执行前会重置上一次绑定值。`statement:exec` 不能执行会返回结果集的查询；查询应使用 [statement:query](query.md)。
