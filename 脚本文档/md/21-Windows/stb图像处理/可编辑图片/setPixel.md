---
params: "x: integer, y: integer, rgb: integer"
returns: "boolean"
---

**方法名称：** 修改指定坐标的像素颜色。

**语法：** `image:setPixel(x, y, rgb)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `x` / `y` | `integer` | 是 | 从 `0, 0` 开始的图片坐标。 |
| `rgb` | `integer` | 是 | `0xRRGGBB` 颜色，范围 `0~0xffffff`。 |

| 返回值 | 说明 |
|---|---|
| `true` | 像素修改成功。 |
| `false` | 坐标越界或颜色值超出范围。 |

**详细说明：**

修改直接作用于图片对象内的 RGB24 点阵，需调用 [save](save.md) 才会写入文件。

```lua
local image = assert(stb_image.load("input.png"))
assert(image:setPixel(10, 20, 0xffff00))
assert(image:save("output.png", 100))
image:close()
```
