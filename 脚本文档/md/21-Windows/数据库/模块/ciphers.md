---
params: ""
returns: "ciphers: string[]"
---

**方法名称：** 获取 SQLite3MC 支持的加密格式。

**语法：** `db.ciphers()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 不需要参数。 |

| 返回值 | 说明 |
|---|---|
| `string[]` | 1-based 字符串数组，内容来自当前编译进 Worker 的 SQLite3MC 驱动。 |

**使用示例：**

```lua
local db = require("db")
for index, name in ipairs(db.ciphers()) do
    print(index, name)
end
```

**详细说明：**

不同构建的驱动支持列表可能不同。打开加密数据库前应使用返回列表中的名称填写 `db.open` 的 `cipher`，不要直接假定其他名称一定可用。
