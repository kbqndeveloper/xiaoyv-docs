---
params: "newKey: string"
returns: "true 或 nil, error"
---

**方法名称：** 修改当前 SQLite 数据库密码。

**语法：** `connection:rekey(newKey)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `newKey` | `string` | 是 | 新密码字符串；传入空字符串可按驱动规则移除密码。 |

| 返回值 | 说明 |
|---|---|
| `true` | 修改成功。 |
| `nil, error` | 连接已关闭、驱动不支持或修改失败。 |

**使用示例：**

```lua
local db = require("db")
local secure = assert(db.open("sqlite", {
    path = "data/secure.sqlite",
    key = "旧密码",
    cipher = "sqlcipher",
    compatibility = 4,
}))
assert(secure:rekey("新密码"))
secure:close()
```

**详细说明：**

打开已有加密库时，`key`、`cipher` 和 `compatibility` 必须匹配原数据库。修改密码后，后续重新打开数据库必须使用新密码；脚本自行负责保存和保护密码。
