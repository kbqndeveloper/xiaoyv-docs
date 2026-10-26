---
params: "...: any"
returns: "cursor: XiaoYvDbCursor（userdata） 或 nil, error: string"
---

**方法名称：** 执行预编译查询。

**语法：** `statement:query(...)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `...` | `any` | 按 SQL 定义 | 按预编译 SQL 的绑定序号传入参数。 |

| 返回值 | 说明 |
|---|---|
| `cursor` | 查询游标；使用 `fetch` 或 `rows` 读取。 |
| `nil, error` | 参数数量、类型或查询执行失败。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER, name TEXT)"))
assert(conn:exec("INSERT INTO user VALUES(?, ?)", 1, "小鱼"))

local stmt = assert(conn:prepare(
    "SELECT id, name FROM user WHERE id > ?"))
local cursor = assert(stmt:query(0))
for row in cursor:rows() do
    print(row.id, row.name)
end
cursor:close()
stmt:close()
conn:close()
```

**详细说明：**

查询会暂时占用语句，游标读取结束或关闭后语句才可以再次 `query`、`exec`、`reset` 或 `close`。
