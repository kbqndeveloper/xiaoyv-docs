---
params: "src: Mat userdata, code: integer, dst?: Mat userdata, dstCn?: integer"
returns: "Mat userdata"
---

**方法名称：** 转换图片的颜色空间或通道布局。

**语法：** `cv.cvtColor(src, code[, dst[, dstCn]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `src` | `Mat userdata` | 是 | 输入图片。 |
| `code` | `integer` | 是 | 转换常量，例如 `cv.COLOR_BGR2GRAY`、`cv.COLOR_BGR2RGB`、`cv.COLOR_BGR2HSV`。 |
| `dst` | `Mat userdata` | 否 | 已有输出对象；省略时自动创建并返回。 |
| `dstCn` | `integer` | 否 | 输出通道数，通常省略。 |

| 返回值 | 说明 |
|---|---|
| `Mat userdata` | 颜色转换后的图片。 |

```lua
local cv = require("opencv")
local image = cv.imread("输入图片.png", cv.IMREAD_COLOR)
local gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY)
local hsv = cv.cvtColor(image, cv.COLOR_BGR2HSV)
```

`imread` 的彩色结果是 BGR，不要直接按 RGB 顺序解释像素。

**详细说明：**

转换常量必须与输入通道布局匹配；例如 BGR 图片转灰度使用 `COLOR_BGR2GRAY`。
