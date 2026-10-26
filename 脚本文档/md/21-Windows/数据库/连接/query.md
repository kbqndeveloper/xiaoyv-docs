---
params: "sql: string, ...: any"
returns: "cursor: XiaoYvDbCursor（userdata） 或 nil, error: string"
---

**方法名称：** 执行查询并创建游标。

**语法：** `connection:query(sql, ...)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `sql` | `string` | 是 | 一条会返回结果集的 SQL 语句。 |
| `...` | `any` | 否 | 按绑定序号传入参数。 |

| 返回值 | 说明 |
|---|---|
| `cursor` | 查询游标；使用 `fetch` 或 `rows` 读取结果。 |
| `nil, error` | SQL 准备、参数绑定或执行失败。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER, name TEXT)"))
conn:exec("INSERT INTO user VALUES(?, ?)", 1, "小鱼")

local cursor = assert(conn:query("SELECT id, name FROM user WHERE id > ?", 0))
for row in cursor:rows() do
    print(row.id, row.name)
end
cursor:close()
conn:close()
```

**详细说明：**

游标创建后查询语句保持占用状态，读取完成或不再需要时应调用 `cursor:close()`。需要一次读取一行并自行处理错误时使用 `cursor:fetch()`。
