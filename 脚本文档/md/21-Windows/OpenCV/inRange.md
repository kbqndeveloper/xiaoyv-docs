---
params: "src: Mat userdata, lowerb: number|table, upperb: number|table, dst?: Mat userdata"
returns: "Mat userdata"
---

**方法名称：** 按像素通道范围生成掩码。

**语法：** `cv.inRange(src, lowerb, upperb[, dst])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `src` | `Mat userdata` | 是 | 输入图片。 |
| `lowerb` | `number/table` | 是 | 各通道下限；多通道使用数组，例如 `{35, 50, 50}`。 |
| `upperb` | `number/table` | 是 | 各通道上限。 |
| `dst` | `Mat userdata` | 否 | 已有输出对象。 |

| 返回值 | 说明 |
|---|---|
| `Mat userdata` | 单通道掩码，满足范围的像素为 `255`。 |

```lua
local cv = require("opencv")
local image = cv.imread("输入图片.png", cv.IMREAD_COLOR)
local hsv = cv.cvtColor(image, cv.COLOR_BGR2HSV)
local mask = cv.inRange(hsv, {35, 50, 50}, {85, 255, 255})
```

输出是单通道掩码，满足范围的像素为 `255`，其他像素为 `0`。

**详细说明：**

多通道上下限按 BGR、HSV 等输入矩阵的通道顺序填写；上下限数组长度应与通道数匹配。
