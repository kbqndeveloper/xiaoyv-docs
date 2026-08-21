---
params: "filename: string, flags?: integer"
returns: "Mat userdata"
---

**方法名称：** 从文件读取图片。

**语法：** `cv.imread(filename[, flags])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `filename` | `string` | 是 | 图片路径，使用 UTF-8；支持中文路径。 |
| `flags` | `integer` | 否 | 读取模式，常用 `cv.IMREAD_COLOR`、`cv.IMREAD_GRAYSCALE`、`cv.IMREAD_UNCHANGED`。 |

| 返回值 | 说明 |
|---|---|
| `Mat userdata` | 读取结果；文件不存在或解码失败时返回空 Mat。 |

```lua
local cv = require("opencv")
local image = cv.imread("输入图片.png", cv.IMREAD_COLOR)
assert(image and not image:empty(), "图片读取失败")
```

彩色图片默认按 BGR 通道顺序读取。需要 RGB 或灰度时，再调用 [cvtColor](cvtColor.md)。

**详细说明：**

读取失败时不会自动抛出文件错误，必须通过 `image:empty()` 检查结果。
