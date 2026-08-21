---
params: "x: integer, y: integer, split: boolean?"
returns: "rgb 或 r, g, b"
---

**方法名称：** 读取指定坐标的像素颜色。

**语法：** `image:getPixel(x, y, split)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `x` / `y` | `integer` | 是 | 从 `0, 0` 开始的图片坐标。 |
| `split` | `boolean` | 否 | 为真时拆分返回 R、G、B；默认返回一个 RGB 整数。 |

| 返回值 | 说明 |
|---|---|
| `rgb` | 默认返回 `0xRRGGBB`。 |
| `r, g, b` | `split=true` 时分别返回 `0~255` 的三个通道值。 |
| `-1` 或 `-1, -1, -1` | 坐标越界。 |

**详细说明：**

图片关闭后调用会抛出 Lua 错误。`split` 使用 Lua truthy 规则判断。

```lua
local image = assert(stb_image.load("input.png"))
local rgb = image:getPixel(10, 20)
local r, g, b = image:getPixel(10, 20, true)
print(rgb, r, g, b)
image:close()
```
