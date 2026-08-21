---
params: "src: Mat userdata, dsize: {width, height}, dst?: Mat userdata, fx?: number, fy?: number, interpolation?: integer"
returns: "Mat userdata"
---

**方法名称：** 缩放图片。

**语法：** `cv.resize(src, dsize[, dst[, fx, fy, interpolation]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `src` | `Mat userdata` | 是 | 输入图片。 |
| `dsize` | `table` | 是 | 两个整数的数组 `{width, height}`；指定目标尺寸时使用。 |
| `dst` | `Mat userdata` | 否 | 已有输出对象。 |
| `fx, fy` | `number` | 否 | 缩放比例；通常在 `dsize` 为 `{0, 0}` 时使用。 |
| `interpolation` | `integer` | 否 | 插值方式，例如 `cv.INTER_LINEAR`、`cv.INTER_AREA`。 |

| 返回值 | 说明 |
|---|---|
| `Mat userdata` | 缩放后的图片。 |

```lua
local cv = require("opencv")
local image = cv.imread("输入图片.png", cv.IMREAD_COLOR)
local small = cv.resize(image, {640, 360}, nil, 0, 0, cv.INTER_AREA)
```

**详细说明：**

指定目标尺寸时直接传 `{width, height}`；比例缩放时传 `{0, 0}` 并提供 `fx`、`fy`。
