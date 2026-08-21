---
params: "sql: string, ...: any"
returns: "row、nil 或 nil, error"
---

**方法名称：** 执行查询并读取第一行。

**语法：** `connection:queryOne(sql, ...)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `sql` | `string` | 是 | 一条查询 SQL 语句。 |
| `...` | `any` | 否 | 按绑定序号传入参数。 |

| 返回值 | 说明 |
|---|---|
| `row` | 第一行结果表，同时支持数字下标和列名键。 |
| `nil` | 查询成功但没有任何行。 |
| `nil, error` | SQL 准备、参数绑定或执行失败。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER, name TEXT)"))
assert(conn:exec("INSERT INTO user VALUES(?, ?)", 1, "小鱼"))

local row, err = conn:queryOne(
    "SELECT id, name FROM user WHERE name = ?", "小鱼")
assert(err == nil, err)
if row == nil then
    print("没有找到用户")
else
    print(row.id, row.name)
end
conn:close()
```

**详细说明：**

该方法只读取第一行并自动释放临时语句，适合查询单条配置或记录。查询没有结果时返回单个 `nil`，不要把它和 `nil, error` 混为一谈。
