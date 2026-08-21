---
params: "image: Mat userdata, mode: integer, method: integer, contours?: userdata, hierarchy?: userdata, offset?: {x, y}"
returns: "contours userdata, hierarchy userdata"
---

**方法名称：** 在二值图中查找轮廓。

**语法：** `cv.findContours(image, mode, method[, contours[, hierarchy[, offset]]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `image` | `Mat userdata` | 是 | 单通道 8 位二值图；函数可能修改输入数据，保留原图时先 `:clone()`。 |
| `mode` | `integer` | 是 | 例如 `cv.RETR_EXTERNAL`、`cv.RETR_TREE`。 |
| `method` | `integer` | 是 | 例如 `cv.CHAIN_APPROX_SIMPLE`、`cv.CHAIN_APPROX_NONE`。 |
| `contours` | `userdata` | 否 | 已有输出容器。 |
| `hierarchy` | `userdata` | 否 | 已有层级输出容器。 |
| `offset` | `table` | 否 | 坐标偏移 `{x, y}`。 |

| 返回值 | 说明 |
|---|---|
| `contours userdata` | 轮廓容器。 |
| `hierarchy userdata` | 轮廓层级容器。 |

```lua
local cv = require("opencv")
local gray = cv.cvtColor(cv.imread("输入图片.png"), cv.COLOR_BGR2GRAY)
local _, binary = cv.threshold(gray, 128, 255, cv.THRESH_BINARY)
local contours, hierarchy = cv.findContours(binary, cv.RETR_EXTERNAL, cv.CHAIN_APPROX_SIMPLE)
print(contours:size())
```

**详细说明：**

输入应为单通道 8 位二值图；需要保留原二值图时，调用前先使用 `image:clone()`。
