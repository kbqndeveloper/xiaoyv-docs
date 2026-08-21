---
params: ""
returns: "true"
---

**方法名称：** 关闭查询游标。

**语法：** `cursor:close()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 不需要参数。 |

| 返回值 | 说明 |
|---|---|
| `true` | 游标已关闭；重复调用也返回 `true`。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER)"))
assert(conn:exec("INSERT INTO user VALUES(?)", 1))

local cursor = assert(conn:query("SELECT id FROM user"))
local row = cursor:fetch()
print(row and row.id)
assert(cursor:close())
conn:close()
```

**详细说明：**

关闭游标会结束剩余结果读取，并释放所属查询语句的占用状态。提前结束 `for` 循环或不再需要结果时应主动关闭。
