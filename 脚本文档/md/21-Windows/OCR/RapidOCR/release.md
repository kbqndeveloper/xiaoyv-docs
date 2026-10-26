---
params: "name?: string"
returns: "released: boolean"
---

**方法名称：** 释放指定名称的 OCR 模型。

**语法：** `ocr.release([name])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `name` | `string` | 否 | 模型名称，默认 `"default"`。 |

| 返回值 | 说明 |
|---|---|
| `true` | 已移除该名称对模型的引用。 |
| `false` | 该名称没有已加载的模型。 |

```lua
local ocr = require("rapidocr")
ocr.release()
ocr.release("en")
```

**详细说明：**

仅释放指定名称；多个名称共享同一配置时，最后一个名称释放后才释放模型资源。不会卸载插件 DLL。

`default` 保留配置，下一次默认识别自动重新加载。其他名称释放后需要重新 [init](init.md)，否则通过 `options.model` 使用该名称时返回 `nil, error`。
