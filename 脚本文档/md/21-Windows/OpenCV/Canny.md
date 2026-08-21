---
params: "image: Mat userdata, threshold1: number, threshold2: number, edges?: Mat userdata, apertureSize?: integer, L2gradient?: boolean"
returns: "Mat userdata"
---

**方法名称：** 使用 Canny 算法提取边缘。

**语法：** `cv.Canny(image, threshold1, threshold2[, edges[, apertureSize[, L2gradient]]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `image` | `Mat userdata` | 是 | 输入单通道图片，通常先灰度化并降噪。 |
| `threshold1` | `number` | 是 | 滞后阈值下限。 |
| `threshold2` | `number` | 是 | 滞后阈值上限。 |
| `edges` | `Mat userdata` | 否 | 已有输出对象。 |
| `apertureSize` | `integer` | 否 | Sobel 核大小，常用 `3`。 |
| `L2gradient` | `boolean` | 否 | 是否使用更精确的 L2 梯度。 |

| 返回值 | 说明 |
|---|---|
| `Mat userdata` | 单通道边缘图，边缘像素通常为 `255`。 |

```lua
local cv = require("opencv")
local image = cv.imread("输入图片.png", cv.IMREAD_COLOR)
local gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY)
local edges = cv.Canny(gray, 50, 150)
```

**详细说明：**

输入通常先经过灰度化和高斯滤波；两个阈值的大小关系会直接影响边缘数量。
