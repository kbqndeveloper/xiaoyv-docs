---
params: "见下方方法表"
returns: "见下方方法表"
---

**方法名称：** JSON 编码与解码（Android / Windows）。

**语法：** 见下方方法表。

**参数说明：** 使用 `local json = require("json")` 加载内置模块，无需安装插件。

| 方法 | 参数 | 返回值 |
|---|---|---|
| `json.encode(value: any)` | 将 Lua 值编码为 JSON 文本 | text: string |
| `json.decode(text: string)` | 将 JSON 文本解码为 Lua 值 | value: any |

| 返回值 | 说明 |
|---|---|
| text | 编码得到的 JSON 文本 |
| value | 解码得到的 Lua 值；JSON null 对应 `json.null` |

编码遇到不支持的类型、循环引用，或解码遇到非法 JSON 时抛出 Lua 错误，可用 `pcall` 捕获。

| 字段 | 类型 | 说明 |
|---|---|---|
| json.null | lightuserdata | JSON 的 null，用于保留字段或数组元素中的空值 |
| json.empty_array | lightuserdata | 编码为 `[]` 的空数组标记 |

**详细说明：**

- `{}` 编码为空对象，`json.empty_array` 编码为空数组。解码 `[]` 后重新编码仍为 `[]`。
- JSON 的 `null` 用 `json.null` 表示，Lua 表中的 `nil` 会使字段消失。
- 文本使用 UTF-8；数字按双精度浮点数处理，长整数编号请用字符串保存。

```lua
local json = require("json")

local text = json.encode({name = "小鱼", items = json.empty_array, remark = json.null})
local value = json.decode(text)
print(value.name) -- 小鱼
print(value.remark == json.null) -- true
print(json.encode(value.items)) -- []
```
