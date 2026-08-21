---
params: "src: Mat userdata, ksize: {width, height}, sigmaX: number, dst?: Mat userdata, sigmaY?: number, borderType?: integer"
returns: "Mat userdata"
---

**方法名称：** 高斯滤波，常用于降噪和边缘检测前的平滑。

**语法：** `cv.GaussianBlur(src, ksize, sigmaX[, dst[, sigmaY[, borderType]]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `src` | `Mat userdata` | 是 | 输入图片。 |
| `ksize` | `table` | 是 | 核大小 `{width, height}`，通常使用奇数，例如 `{5, 5}`。 |
| `sigmaX` | `number` | 是 | X 方向标准差；传 `0` 时由核大小推导。 |
| `dst` | `Mat userdata` | 否 | 已有输出对象。 |
| `sigmaY` | `number` | 否 | Y 方向标准差，省略时使用 `sigmaX`。 |
| `borderType` | `integer` | 否 | 边界处理方式。 |

| 返回值 | 说明 |
|---|---|
| `Mat userdata` | 滤波后的图片。 |

```lua
local cv = require("opencv")
local image = cv.imread("输入图片.png", cv.IMREAD_COLOR)
local blurred = cv.GaussianBlur(image, {5, 5}, 0)
```

**详细说明：**

`ksize` 的宽高通常使用正奇数；`sigmaX` 传 `0` 时由核大小自动推导。
