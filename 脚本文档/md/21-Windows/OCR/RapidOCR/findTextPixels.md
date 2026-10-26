---
params: "data: integer|lightuserdata|string, width: integer, height: integer, text: string, options?: XiaoYvOcrFindPixelOptions（table）"
returns: "result: XiaoYvOcrMatch（table）|XiaoYvOcrMiss（table） 或 nil, error: string"
---

**方法名称：** 在原始像素中查找文字。

**语法：** `ocr.findTextPixels(data, width, height, text[, options])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `data` | `integer / lightuserdata / string` | 是 | 原始像素地址或字节串，格式和内存要求见 [readPixels](readPixels.md)。 |
| `width, height` | `integer` | 是 | 图像宽、高。 |
| `text` | `string` | 是 | 要查找的 UTF-8 文字。 |
| `options` | `XiaoYvOcrFindPixelOptions`（table） | 否 | 支持 [像素识别选项](readPixels.md)和以下匹配选项。 |

**类型：** `XiaoYvOcrFindPixelOptions`（table），继承 `XiaoYvOcrPixelOptions`（table）。

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `exact` | `boolean` | 否 | 默认 `false`；匹配规则见 [findText](findText.md)。 |

| 返回值 | 说明 |
|---|---|
| `{found = true, ...}` | 第一个匹配文字框，字段与 [findText](findText.md) 相同。 |
| `{found = false}` | 没有匹配文字框。 |
| `nil, error` | 运行失败及错误说明。 |

```lua
local ocr = require("rapidocr")
-- pixels 是调用方已有的 RGB 原始像素字节串
local result, err = ocr.findTextPixels(pixels, width, height, "确定", {
    format = "RGB", exact = true
})
assert(result, err)
```

**详细说明：**

匹配规则与 [findText](findText.md) 一致。返回整个文字框，坐标相对输入像素图像左上角。
