---
params: ""
returns: "loaded: boolean"
---

**方法名称：** 查询 OCR 模型是否已加载。

**语法：** `ocr.isLoaded()`

**参数说明：** 无。

| 返回值 | 说明 |
|---|---|
| `boolean` | 当前模型已加载为 `true`，否则为 `false`。 |

```lua
local ocr = require("rapidocr")
print(ocr.isLoaded())
```

**详细说明：**

只查询当前 Lua VM 的模型状态，不触发模型加载。仅执行 `require` 后返回 `false`。
