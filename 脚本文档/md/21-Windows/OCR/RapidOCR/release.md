---
params: ""
returns: "released: boolean"
---

**方法名称：** 释放当前 OCR 模型。

**语法：** `ocr.release()`

**参数说明：** 无。

| 返回值 | 说明 |
|---|---|
| `true` | 已释放原先加载的模型。 |
| `false` | 调用前没有已加载的模型。 |

```lua
local ocr = require("rapidocr")
ocr.release()
```

**详细说明：**

释放模型但保留当前配置，下一次识别自动重新加载该配置。不会卸载插件 DLL。
