---
params: "rgb: integer"
returns: "h, s, v"
---

**方法名称：** 将 RGB 颜色转换为 HSV。

**语法：** `stb_image.rgbToHsv(rgb)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `rgb` | `integer` | 是 | `0xRRGGBB` 颜色值。 |

| 返回值 | 说明 |
|---|---|
| `h` | 色相，范围 `0~360`。 |
| `s` | 饱和度，范围 `0~100`。 |
| `v` | 明度，范围 `0~100`。 |

**详细说明：**

输入值按低 24 位解析为 R、G、B，返回的是 Lua number。灰度颜色的色相和饱和度为 0。

```lua
local h, s, v = stb_image.rgbToHsv(0xff8800)
print(h, s, v)
```
