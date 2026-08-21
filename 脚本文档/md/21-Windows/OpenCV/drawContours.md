---
params: "image: Mat userdata, contours: userdata, contourIdx: integer, color: number|table, thickness?: integer"
returns: "Mat userdata"
---

**方法名称：** 在图片上绘制轮廓。

**语法：** `cv.drawContours(image, contours, contourIdx, color[, thickness[, lineType[, hierarchy[, maxLevel[, offset]]]]])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `image` | `Mat userdata` | 是 | 要绘制的图片。 |
| `contours` | `userdata` | 是 | `cv.findContours` 返回的轮廓容器。 |
| `contourIdx` | `integer` | 是 | 指定轮廓索引；传 `-1` 绘制全部轮廓。 |
| `color` | `number/table` | 是 | BGR 颜色，例如 `{0, 255, 0}`。 |
| `thickness` | `integer` | 否 | 线宽；传负数可填充轮廓。 |

| 返回值 | 说明 |
|---|---|
| `Mat userdata` | 绘制后的图片；通常与输入 `image` 共享或写入同一输出对象。 |

```lua
local cv = require("opencv")
local image = cv.imread("输入图片.png", cv.IMREAD_COLOR)
local gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY)
local _, binary = cv.threshold(gray, 128, 255, cv.THRESH_BINARY)
local contours = cv.findContours(binary, cv.RETR_EXTERNAL, cv.CHAIN_APPROX_SIMPLE)
cv.drawContours(image, contours, -1, {0, 255, 0}, 2)
assert(cv.imwrite("轮廓.png", image))
```

**详细说明：**

`contourIdx` 传 `-1` 时绘制所有轮廓；颜色数组按 OpenCV 的 BGR 顺序填写。
