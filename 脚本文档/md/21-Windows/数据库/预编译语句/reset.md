---
params: ""
returns: "true 或 nil, error: string"
---

**方法名称：** 重置预编译语句。

**语法：** `statement:reset()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 不需要参数。 |

| 返回值 | 说明 |
|---|---|
| `true` | 语句已重置，可以重新绑定参数执行。 |
| `nil, error` | 语句已关闭，或仍被游标占用。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER PRIMARY KEY, name TEXT)"))

local stmt = assert(conn:prepare(
    "INSERT INTO user(name) VALUES(?)"))
assert(stmt:exec("第一次"))
assert(stmt:reset())
assert(stmt:exec("第二次"))
stmt:close()
conn:close()
```

**详细说明：**

`exec` 和 `query` 会自动重置语句；通常只有需要明确结束一次执行、但还要保留语句 userdata 时才直接调用 `reset`。有活动游标时必须先关闭游标。
