---
params: "driver: string, options: table"
returns: "connection 或 nil, error"
---

**方法名称：** 打开 SQLite 数据库。

**语法：** `db.open(driver, options)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `driver` | `string` | 是 | 当前只能填写 `"sqlite"`。 |
| `options` | `table` | 是 | 数据库路径、创建方式、超时和加密配置。 |

| 选项 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| `path` | `string` | 无 | 数据库文件路径，必填；也可以填写 `:memory:`。 |
| `key` | `string` | 无 | 可选数据库密码；填写后按加密数据库打开。 |
| `cipher` | `string` | `"sqlcipher"` | `key` 存在时使用的 SQLite3MC 加密格式。 |
| `compatibility` | `integer` | SQLCipher 为 `4` | 只适用于 SQLCipher，取值 `1` 到 `4`。 |
| `busy_timeout` | `integer` | `5000` | 忙等待时间，单位毫秒，范围 `0` 到 `600000`。 |
| `read_only` | `boolean` | `false` | 只读打开；不能与 `wal = true` 同时使用。 |
| `create` | `boolean` | `true` | 非只读时允许创建不存在的数据库文件。 |
| `wal` | `boolean` | `false` | 打开后执行 `PRAGMA journal_mode=WAL`。 |
| `cipher_options` | `table` | 无 | SQLite3MC 高级加密选项；键为字符串、值为整数。 |

| 返回值 | 说明 |
|---|---|
| `connection` | 打开的 SQLite 连接 userdata。 |
| `nil, error` | 参数、路径、数据库格式或密码不正确时返回。 |

**使用示例：**

```lua
local db = require("db")
local conn, err = db.open("sqlite", {
    path = "data/sample.sqlite",
    create = true,
    busy_timeout = 3000,
})
assert(conn, err)
assert(conn:ping())
conn:close()
```

**详细说明：**

路径和 SQL 文本使用 UTF-8。`create = false` 时不会创建不存在的文件；`read_only = true` 时必须打开已有数据库。加密数据库需要正确的 `key`、`cipher` 和 `compatibility`，可用加密格式见 [db.ciphers](ciphers.md)。
