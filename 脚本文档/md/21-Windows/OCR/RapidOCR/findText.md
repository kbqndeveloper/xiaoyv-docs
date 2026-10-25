---
params: "path: string, text: string, options?: table"
returns: "result 或 nil, error"
---

**方法名称：** 在图片中查找文字。

**语法：** `ocr.findText(path, text[, options])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `path` | `string` | 是 | UTF-8 图片文件路径。 |
| `text` | `string` | 是 | 要查找的 UTF-8 文字。 |
| `options` | `table` | 否 | 支持 [识别选项](read.md)；另有 `exact`，默认 `false` 为包含匹配，`true` 为整框文字相等。 |

| 返回值 | 说明 |
|---|---|
| `{found = true, ...}` | 第一个匹配文字框，另含 `text, x, y, w, h, score, points`，字段见 [read](read.md)。 |
| `{found = false}` | 没有匹配文字框。 |
| `nil, error` | 运行失败及错误说明。 |

```lua
local ocr = require("rapidocr")
local result, err = ocr.findText("截图.png", "确定", {exact = true})
assert(result, err)
if result.found then
    print(result.x, result.y, result.w, result.h)
end
```

**详细说明：**

按识别结果顺序返回第一个匹配框，区分大小写。即使只匹配框内的一部分文字，返回的也是整个文字框的位置，不是子串位置。坐标相对输入图片左上角。
