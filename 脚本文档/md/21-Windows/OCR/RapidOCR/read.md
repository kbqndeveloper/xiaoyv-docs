---
params: "path: string, options?: XiaoYvOcrOptions（table）"
returns: "result: XiaoYvOcrResult（table） 或 nil, error: string"
---

**方法名称：** 识别图片中的文字。

**语法：** `ocr.read(path[, options])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `path` | `string` | 是 | UTF-8 图片文件路径，例如 PNG、JPEG。 |
| `options` | `XiaoYvOcrOptions`（table） | 否 | 本次识别选项，见下表。 |

**类型：** `XiaoYvOcrOptions`（table），继承 `XiaoYvOcrLineOptions`（table）。

共同的识别字段见 [单行识别选项](readLine.md)。

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `maxSideLen` | `integer` | 否 | 默认 `960`；检测图像最长边，范围 `32～2048`。 |
| `threshold` | `number` | 否 | 默认 `0.30`；检测二值化阈值，范围 `0.01～0.99`。 |
| `boxThreshold` | `number` | 否 | 默认 `0.50`；检测框得分阈值，范围 `0.01～0.99`。 |
| `unClipRatio` | `number` | 否 | 默认 `1.5`；检测框扩张系数，范围 `0～4`。 |

数值选项超出范围时限制到相应边界。以上选项也适用于找字和原始像素接口。

| 返回值 | 说明 |
|---|---|
| `{text = "...", items = {...}}` | 全部文字和文字框数组；无文字时为 `{text = "", items = {}}`。 |
| `nil, error` | 文件读取、模型加载或识别失败及错误说明。 |

`result.text` 按 `items` 顺序将各文字框的文字用换行符 `\n` 拼接，不恢复原图排版。

**类型：** `XiaoYvOcrResult`（table）

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `text` | `string` | 是 | 全部文字，无文字时为空字符串。 |
| `items` | `XiaoYvOcrBox（table）[]` | 是 | 文字框数组，无文字时为空数组。 |

**类型：** `XiaoYvOcrBox`（table）

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `text` | `string` | 是 | UTF-8 文字。 |
| `x` | `integer` | 是 | 外接矩形左上角 X 坐标。 |
| `y` | `integer` | 是 | 外接矩形左上角 Y 坐标。 |
| `w` | `integer` | 是 | 外接矩形宽度。 |
| `h` | `integer` | 是 | 外接矩形高度。 |
| `score` | `number` | 是 | 识别得分。 |
| `points` | `XiaoYvOcrPoint（table）[]` | 是 | 四个顶点，依次为左上、右上、右下、左下。 |

**类型：** `XiaoYvOcrPoint`（table）

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `x` | `number` | 是 | 顶点 X 坐标。 |
| `y` | `number` | 是 | 顶点 Y 坐标。 |

```lua
local ocr = require("rapidocr")
local result, err = ocr.read("截图.png", {minScore = 0.6})
assert(result, err)
print(result.text) -- 全部文字
for _, item in ipairs(result.items) do
    print(item.text, item.x, item.y, item.w, item.h)
end
```

**详细说明：**

尚未加载模型时，自动加载当前配置，初始配置为 `server`。坐标以输入图片左上角为原点；本方法不负责截图。需要更换模型时调用 [init](init.md)。

输入已裁好的单行文字时，可使用 [readLine](readLine.md) 跳过文字检测。
