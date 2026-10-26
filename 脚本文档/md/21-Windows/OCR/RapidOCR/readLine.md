---
params: "path: string, options?: XiaoYvOcrLineOptions（table）"
returns: "text: string, score: number 或 nil, error: string"
---

**方法名称：** 识别图片中的单行文字。

**语法：** `ocr.readLine(path[, options])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `path` | `string` | 是 | 已裁好的横排单行文字图片，使用 UTF-8 文件路径。 |
| `options` | `XiaoYvOcrLineOptions`（table） | 否 | 本次识别选项；检测选项不生效。 |

**类型：** `XiaoYvOcrLineOptions`（table）

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `model` | `string` | 否 | 模型名称，默认 `"default"`；其他名称需先通过 [init](init.md) 加载，未加载时返回 `nil, error`。 |
| `allowlist` | `string` | 否 | UTF-8 字符白名单，例如 `"0123456789"`；省略或空字符串不限制字符。 |
| `useAngle` | `boolean` | 否 | 默认 `true`；启用已配置的方向分类模型。 |
| `minScore` | `number` | 否 | 默认 `0.5`；识别结果最低得分，范围 `0～1`。 |
| `maxRecognizerWidth` | `integer` | 否 | 默认 `4096`；识别图像最大宽度，范围 `32～4096`。 |

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

-- 数字区域：在解码时限制可选字符
local digits, digitScore = ocr.readLine("数字.png", {
    useAngle = false, allowlist = "0123456789"
})
assert(digits, digitScore)
print(digits, digitScore)
```

**详细说明：**

将整张输入图片作为一行，跳过文字检测和透视裁剪，直接进行识别；不适合整屏或多行图片。`useAngle` 默认开启，方向已知正常时可关闭，省去方向分类推理。

`model` 选择本次使用的模型，不改变后续调用的默认值。沿用 [read](read.md) 的模型加载规则，仍需配套检测模型；此入口省去的是检测推理，不改变模型加载方式。需要文字框位置或多行结果时使用 [read](read.md)。

`allowlist` 在解码选字符时生效，不是识别后删除字符；只在模型字典中已有的允许字符与空白符之间选择，不会增加模型不支持的字符。不填或传空字符串时，识别和得分计算与原来一致。设置白名单也不会重新归一化得分，结果仍受 `minScore` 限制，不保证模糊字符一定识别正确。

只识别字母可填 `"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz"`，数字和字母则在此字符串中加上 `0123456789`。白名单按字符区分大小写；需要空格、小数点或负号时也应列入。上述共享选项同样适用于多行、找字和像素入口。
