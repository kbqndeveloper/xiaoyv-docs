---
params: "data: integer|lightuserdata|string, width: integer, height: integer, options?: XiaoYvOcrPixelOptions（table）"
returns: "result: XiaoYvOcrResult（table） 或 nil, error: string"
---

**方法名称：** 从原始像素识别文字。

**语法：** `ocr.readPixels(data, width, height[, options])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `data` | `integer / lightuserdata / string` | 是 | 像素首地址或原始像素字节串；不是 PNG、JPEG 等编码文件内容。 |
| `width, height` | `integer` | 是 | 图像宽、高，单位为像素。 |
| `options` | `XiaoYvOcrPixelOptions`（table） | 否 | 支持 [识别选项](read.md)，另有以下像素选项。 |

**类型：** `XiaoYvOcrPixelOptions`（table），继承 `XiaoYvOcrOptions`（table）。

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `format` | `"BGRA"\|"RGBA"\|"BGR"\|"RGB"` | 否 | 默认 `BGRA`，表示内存字节顺序。 |
| `stride` | `integer` | 否 | 默认 `0`；每行字节数，`0` 表示按宽度和通道数紧密排列。 |

| 返回值 | 说明 |
|---|---|
| `{text = "...", items = {...}}` | 与 [read](read.md) 相同的全部文字和文字框数组；无文字时为 `{text = "", items = {}}`。 |
| `nil, error` | 运行失败及错误说明。 |

```lua
local ocr = require("rapidocr")
-- pixels 是调用方已有的 BGRA 原始像素字节串
local result, err = ocr.readPixels(pixels, width, height)
assert(result, err)
print(result.text)
```

**详细说明：**

像素从顶行开始，调用方负责保证内存有效且大小足够。默认 BGRA 对应 Windows 小端内存中的 `0x00RRGGBB` 点阵。调用期间不得由其他原生线程改写输入内存。返回坐标相对该像素图像左上角。

输入已裁好的单行文字时，可使用 [readLinePixels](readLinePixels.md) 跳过文字检测。
