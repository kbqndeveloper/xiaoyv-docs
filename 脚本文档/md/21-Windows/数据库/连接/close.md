---
params: ""
returns: "true"
---

**方法名称：** 关闭 SQLite 连接。

**语法：** `connection:close()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 不需要参数。 |

| 返回值 | 说明 |
|---|---|
| `true` | 连接已关闭；重复调用也返回 `true`。 |

**使用示例：**

```lua
local db = require("db")
local conn = assert(db.open("sqlite", {path = ":memory:"}))
-- 使用 conn 执行 SQL
assert(conn:close())
```

**详细说明：**

关闭连接前应先关闭该连接创建的游标和预编译语句。连接 userdata 在 Lua 回收时也会自动释放底层资源，但长时间运行的脚本应主动关闭不用的连接。
