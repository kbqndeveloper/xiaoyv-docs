---
params: "sql: string, ...: any"
returns: "affectedRows: integer, lastInsertId: integer 或 nil, error: string"
---

**方法名称：** 执行一条不返回结果集的 SQL。

**语法：** `connection:exec(sql, ...)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `sql` | `string` | 是 | 一条 SQL 语句；支持 `?` 或命名占位符。 |
| `...` | `any` | 否 | 按绑定序号传入参数，类型见[数据类型与资源](../数据类型.md)。 |

| 返回值 | 说明 |
|---|---|
| `affectedRows` | 本次执行影响的行数。 |
| `lastInsertId` | 本次连接最近一次插入的 rowid。 |
| `nil, error` | SQL、参数数量或参数类型不正确，或数据库执行失败。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER PRIMARY KEY, name TEXT)"))
local affected, id = conn:exec("INSERT INTO user(name) VALUES(?)", "小鱼")
print(affected, id)
conn:close()
```

**详细说明：**

每次只能执行一条 SQL。`exec` 不能用于会返回结果集的查询语句，这类 SQL 应使用 [connection:query](query.md) 或 [connection:queryOne](queryOne.md)。
