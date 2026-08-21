---
params: "src: Mat userdata, mask?: Mat userdata"
returns: "minVal, maxVal, minLoc, maxLoc"
---

**方法名称：** 读取单通道矩阵的最小值、最大值及其坐标。

**语法：** `cv.minMaxLoc(src[, mask])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `src` | `Mat userdata` | 是 | 单通道矩阵，常见来源是 `cv.matchTemplate` 的结果。 |
| `mask` | `Mat userdata` | 否 | 可选单通道掩码。 |

| 返回值 | 说明 |
|---|---|
| `minVal` | 最小值。 |
| `maxVal` | 最大值。 |
| `minLoc` | 最小值坐标 `{x, y}`。 |
| `maxLoc` | 最大值坐标 `{x, y}`。 |

```lua
local cv = require("opencv")
local result = cv.Mat.ones(10, 10, cv.CV_32F)
local minVal, maxVal, minLoc, maxLoc = cv.minMaxLoc(result)
print(minVal, maxVal, minLoc[1], maxLoc[2])
```

**详细说明：**

返回的坐标使用 Lua 数组形式 `{x, y}`；模板匹配时通常根据匹配方法选择 `maxLoc` 或 `minLoc`。
