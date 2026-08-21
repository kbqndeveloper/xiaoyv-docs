---
params: "image: Mat userdata, templ: Mat userdata, method: integer, result?: Mat userdata, mask?: Mat userdata"
returns: "Mat userdata"
---

**方法名称：** 计算模板匹配结果矩阵。

**语法：** `cv.matchTemplate(image, templ, method[, result[, mask]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `image` | `Mat userdata` | 是 | 待搜索的大图。 |
| `templ` | `Mat userdata` | 是 | 模板图，尺寸不能大于大图。 |
| `method` | `integer` | 是 | 例如 `cv.TM_CCOEFF_NORMED`、`cv.TM_SQDIFF_NORMED`。 |
| `result` | `Mat userdata` | 否 | 已有输出对象。 |
| `mask` | `Mat userdata` | 否 | 模板掩码；是否支持取决于匹配方法。 |

| 返回值 | 说明 |
|---|---|
| `Mat userdata` | 模板匹配结果矩阵。 |

```lua
local cv = require("opencv")
local image = cv.imread("屏幕.png", cv.IMREAD_COLOR)
local templ = cv.imread("模板.png", cv.IMREAD_COLOR)
local result = cv.matchTemplate(image, templ, cv.TM_CCOEFF_NORMED)
local _, score, _, location = cv.minMaxLoc(result)
print(score, location[1], location[2])
```

`matchTemplate` 只返回结果矩阵；要取得最佳位置和分数，请继续调用 [minMaxLoc](minMaxLoc.md)。

**详细说明：**

模板图不能大于待搜索图片；`TM_CCOEFF_NORMED` 的分数通常越大越接近目标。
