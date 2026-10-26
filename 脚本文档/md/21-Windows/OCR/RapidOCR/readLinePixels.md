---
params: "data: integer|lightuserdata|string, width: integer, height: integer, options?: XiaoYvOcrLinePixelOptions（table）"
returns: "text: string, score: number 或 nil, error: string"
---

**方法名称：** 从原始像素识别单行文字。

**语法：** `ocr.readLinePixels(data, width, height[, options])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `data` | `integer / lightuserdata / string` | 是 | 已裁好的横排单行文字像素地址或字节串，格式和内存要求见 [readPixels](readPixels.md)。 |
| `width, height` | `integer` | 是 | 输入图像的实际宽、高，单位为像素。 |
| `options` | `XiaoYvOcrLinePixelOptions`（table） | 否 | 支持 [单行识别选项](readLine.md)，以及下面的像素选项。 |

**类型：** `XiaoYvOcrLinePixelOptions`（table），继承 `XiaoYvOcrLineOptions`（table）。

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `format` | `"BGRA"\|"RGBA"\|"BGR"\|"RGB"` | 否 | 默认 `BGRA`。 |
| `stride` | `integer` | 否 | 默认 `0`；含义见 [readPixels](readPixels.md)。 |

| 返回值 | 说明 |
|---|---|
| `text, score` | UTF-8 文字和识别得分；没有有效文字或得分低于 `minScore` 时返回 `"", 0`。 |
| `nil, error` | 运行失败及错误说明。 |

```lua
local dm = require("xiaoyv_hcbyj") -- x86 大漠插件
local ocr = require("rapidocr")
-- 按实际文字位置调整，区域内应只有一行文字
local left, top, right, bottom = 100, 100, 299, 139
local data = dm.GetScreenData(left, top, right, bottom)
-- 大漠点阵宽高按坐标差计算，不加 1。
local text, score = ocr.readLinePixels(
    data, right - left, bottom - top
)
assert(text, score)
print(text, score)
```

**详细说明：**

单行识别流程、模型加载和方向处理与 [readLine](readLine.md) 一致，跳过文字检测和透视裁剪。默认 BGRA 可直接读取大漠 `GetScreenData` 点阵，无需保存图片文件。返回文字与得分，不返回坐标。
