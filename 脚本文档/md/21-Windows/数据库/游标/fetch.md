---
params: ""
returns: "row、nil 或 nil, error"
---

**方法名称：** 读取游标的下一行。

**语法：** `cursor:fetch()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 不需要参数。 |

| 返回值 | 说明 |
|---|---|
| `row` | 下一行结果表，同时支持数字下标和列名键。 |
| `nil` | 已经读取完所有行。 |
| `nil, error` | 游标对应连接关闭或读取失败。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
assert(conn:exec("CREATE TABLE user(id INTEGER, name TEXT)"))
assert(conn:exec("INSERT INTO user VALUES(?, ?)", 1, "小鱼"))

local cursor = assert(conn:query("SELECT id, name FROM user"))
while true do
    local row, err = cursor:fetch()
    assert(err == nil, err)
    if row == nil then break end
    print(row.id, row.name)
end
cursor:close()
conn:close()
```

**详细说明：**

`fetch` 适合需要逐次判断错误或在读取过程中提前停止的代码。提前停止时仍应调用 `cursor:close()`，否则所属语句会保持占用状态。
