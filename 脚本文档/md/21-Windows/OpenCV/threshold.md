---
params: "src: Mat userdata, thresh: number, maxval: number, type: integer, dst?: Mat userdata"
returns: "retval: number, dst: Mat userdata"
---

**方法名称：** 对单通道图片进行固定阈值或 Otsu 二值化。

**语法：** `cv.threshold(src, thresh, maxval, type[, dst])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `src` | `Mat userdata` | 是 | 输入单通道图片，通常先转为灰度。 |
| `thresh` | `number` | 是 | 阈值；使用 Otsu 时通常传 `0`。 |
| `maxval` | `number` | 是 | 二值化后的最大值，常用 `255`。 |
| `type` | `integer` | 是 | 例如 `cv.THRESH_BINARY`、`cv.THRESH_BINARY_INV`，可与 `cv.THRESH_OTSU` 组合。 |
| `dst` | `Mat userdata` | 否 | 已有输出对象。 |

| 返回值 | 说明 |
|---|---|
| `retval` | 实际使用的阈值。 |
| `dst` | 二值化结果 Mat。 |

```lua
local cv = require("opencv")
local gray = cv.cvtColor(cv.imread("输入图片.png"), cv.COLOR_BGR2GRAY)
local usedThreshold, binary = cv.threshold(gray, 128, 255, cv.THRESH_BINARY)
```

**详细说明：**

输入应为单通道图片；需要自动选择阈值时，将 `cv.THRESH_OTSU` 与基础阈值类型按位组合。
