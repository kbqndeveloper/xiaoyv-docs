---
params: "name?: string"
returns: "loaded: boolean"
---

**方法名称：** 查询指定名称的 OCR 模型是否已加载。

**语法：** `ocr.isLoaded([name])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `name` | `string` | 否 | 模型名称，默认 `"default"`。 |

| 返回值 | 说明 |
|---|---|
| `boolean` | 该名称的模型已加载为 `true`，否则为 `false`。 |

```lua
local ocr = require("rapidocr")
print(ocr.isLoaded())
print(ocr.isLoaded("en"))
```

**详细说明：**

只查询当前 Lua VM 中指定名称的模型状态，不触发模型加载。仅执行 `require` 后返回 `false`。
