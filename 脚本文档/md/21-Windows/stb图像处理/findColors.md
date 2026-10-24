---
params: "imageData: string, colors: string, x1: integer, y1: integer, x2: integer, y2: integer, dir: integer, sim: integer"
returns: "x, y"
---

**方法名称：** 在图片编码数据中执行多点找色。

**语法：** `stb_image.findColors(imageData, colors, x1, y1, x2, y2, dir, sim)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `imageData` | `string` | 是 | PNG、BMP、JPG/JPEG 等图片文件的编码二进制数据。 |
| `colors` | `string` | 是 | 颜色描述，格式见[多点颜色规则](../../11-找色/查找与分析/原生多点找色.md#颜色规则)。 |
| `x1, y1, x2, y2` | `integer` | 是 | 搜索区域；四个值全为 `0` 时使用整张图片。 |
| `dir` | `integer` | 是 | `1~8`，八种扫描方向。 |
| `sim` | `integer` | 是 | 默认 RGB 通道偏色，例如 `0x101010`；不作用于 HSV。 |

| 返回值 | 说明 |
|---|---|
| `x, y` | 命中时返回图片坐标；未找到、解码失败或参数无效返回 `-1, -1`。 |

**详细说明：**

`stb_image.findColor` 是同一实现的兼容别名。此函数只读取图片编码数据，不读取大漠截图地址。

返回锚点的图片坐标。其他描述点允许超出搜索范围，但必须位于解码后的图片内。

```lua
local file = assert(io.open("input.png", "rb"))
local imageData = file:read("*a")
file:close()

local x, y = stb_image.findColors(
    imageData, "0|0|FF0000,10|5|00FF00", 0, 0, 0, 0, 2, 0x101010
)
print(x, y)
```
