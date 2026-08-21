---
params: "callback: function, mode: string?"
returns: "回调返回值、true 或 nil, error"
---

**方法名称：** 执行带自动提交和回滚的事务。

**语法：** `connection:transaction(callback[, mode])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `callback` | `function` | 是 | 事务回调，接收当前连接作为唯一参数。 |
| `mode` | `string` | 否 | `deferred`、`immediate` 或 `exclusive`，默认 `deferred`。 |

| 返回值 | 说明 |
|---|---|
| 回调返回值 | 回调正常结束后自动提交，并原样返回回调结果。 |
| `true` | 回调没有返回值且事务提交成功。 |
| `nil, error` | 开始、提交失败，或回调抛错并已自动回滚。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER PRIMARY KEY, name TEXT)"))

local ok, err = conn:transaction(function(tx)
    assert(tx:exec("INSERT INTO user(name) VALUES(?)", "小鱼"))
    assert(tx:exec("INSERT INTO user(name) VALUES(?)", "小虾"))
end, "immediate")
assert(ok, err)
conn:close()
```

**详细说明：**

回调中应使用传入的 `tx` 连接执行 SQL。回调主动 `error`、数据库操作抛错或提交失败时会回滚；不要在同一连接上嵌套另一个事务。
