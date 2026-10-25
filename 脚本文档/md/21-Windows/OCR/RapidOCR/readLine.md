---
params: "path: string, options?: table"
returns: "text, score 或 nil, error"
---

**方法名称：** 识别图片中的单行文字。

**语法：** `ocr.readLine(path[, options])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `path` | `string` | 是 | 已裁好的横排单行文字图片，使用 UTF-8 文件路径。 |
| `options` | `table` | 否 | 使用 [read](read.md) 中的 `useAngle`、`minScore`、`maxRecognizerWidth`，默认值和范围相同；检测选项不生效。 |

| 返回值 | 说明 |
|---|---|
| `text, score` | UTF-8 文字和识别得分；没有有效文字或得分低于 `minScore` 时返回 `"", 0`。 |
| `nil, error` | 文件读取、模型加载或识别失败及错误说明。 |

```lua
local ocr = require("rapidocr")
-- 图片已经裁到单行文字，且方向正常
local text, score = ocr.readLine("单行.png", {useAngle = false})
assert(text, score)
print(text, score)
```

**详细说明：**

将整张输入图片作为一行，跳过文字检测和透视裁剪，直接进行识别；不适合整屏或多行图片。`useAngle` 默认开启，方向已知正常时可关闭，省去方向分类推理。

沿用当前完整模型配置和首次识别自动加载规则，仍需配套检测模型；此入口省去的是检测推理，不改变模型加载方式。需要文字框位置或多行结果时使用 [read](read.md)。
