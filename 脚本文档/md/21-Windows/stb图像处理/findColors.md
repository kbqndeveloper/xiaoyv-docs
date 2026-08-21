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
| `colors` | `string` | 是 | 多点规则，使用 `x\|y\|RRGGBB`，多个点用逗号连接。 |
| `x1, y1, x2, y2` | `integer` | 是 | 搜索区域；四个值全为 `0` 时使用整张图片。 |
| `dir` | `integer` | 是 | `1~8`，八种扫描方向。 |
| `sim` | `integer` | 是 | RGB 通道容差，例如 `0x101010`。 |

| 返回值 | 说明 |
|---|---|
| `x, y` | 命中时返回图片坐标；未找到、解码失败或参数无效返回 `-1, -1`。 |

**详细说明：**

`stb_image.findColor` 是同一实现的兼容别名。规则中可以为单个点追加独立 RGB 容差；扫描方向
和 `colors` 规则与 Windows 屏幕找色入口一致，但此函数只读取图片编码数据，不读取大漠截图地址。

```lua
local file = assert(io.open("input.png", "rb"))
local imageData = file:read("*a")
file:close()

local x, y = stb_image.findColors(
    imageData, "0|0|FF0000,10|5|00FF00", 0, 0, 0, 0, 2, 0x101010
)
print(x, y)
```
