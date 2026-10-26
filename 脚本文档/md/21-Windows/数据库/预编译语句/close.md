---
params: ""
returns: "true 或 nil, error: string"
---

**方法名称：** 关闭预编译语句。

**语法：** `statement:close()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 不需要参数。 |

| 返回值 | 说明 |
|---|---|
| `true` | 语句已关闭；重复调用也返回 `true`。 |
| `nil, error` | 语句仍被活动游标占用。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER PRIMARY KEY, name TEXT)"))

local stmt = assert(conn:prepare(
    "INSERT INTO user(name) VALUES(?)"))
assert(stmt:exec("小鱼"))
assert(stmt:close())
conn:close()
```

**详细说明：**

语句关闭后不能再执行或重置。若语句创建过游标，必须先关闭游标；不再引用的语句最终也会由 Lua userdata 自动回收。
