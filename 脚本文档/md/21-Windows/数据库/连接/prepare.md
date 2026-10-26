---
params: "sql: string"
returns: "statement: XiaoYvDbStatement（userdata） 或 nil, error: string"
---

**方法名称：** 创建可重复使用的 SQLite 预编译语句。

**语法：** `connection:prepare(sql)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `sql` | `string` | 是 | 一条待预编译的 SQL 语句。 |

| 返回值 | 说明 |
|---|---|
| `statement` | 预编译语句 userdata，可反复调用 `exec` 或 `query`。 |
| `nil, error` | SQL 为空、包含多条语句或预编译失败。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER PRIMARY KEY, name TEXT)"))

local insert = assert(conn:prepare(
    "INSERT INTO user(name) VALUES(?)"))
for _, name in ipairs({"小鱼", "小虾"}) do
    assert(insert:exec(name))
end
insert:close()
conn:close()
```

**详细说明：**

预编译语句适合重复执行相同 SQL。每次 `exec` 或 `query` 都会先清理上一次绑定值；游标仍在读取时不能再次执行、重置或关闭语句。
