---
params: "path: string, width: integer, height: integer, pixels: string, quality: integer"
returns: "boolean"
---

**方法名称：** 写入 RGB24 图片。

**语法：** `stb_image.write(path, width, height, pixels, quality)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `path` | `string` | 是 | 输出路径；扩展名决定 BMP、PNG 或 JPG/JPEG。 |
| `width` | `integer` | 是 | 图片宽度，必须大于 0。 |
| `height` | `integer` | 是 | 图片高度，必须大于 0。 |
| `pixels` | `string` | 是 | RGB24 二进制点阵，长度至少为 `width * height * 3`。 |
| `quality` | `integer` | 是 | JPG/JPEG 质量，自动限制到 `1~100`；其他格式忽略。 |

| 返回值 | 说明 |
|---|---|
| `boolean` | 文件成功写入返回 `true`，尺寸、数据或扩展名无效返回 `false`。 |

**详细说明：**

点阵按行连续存储，不能传入带 alpha 的 RGBA 数据。PNG、BMP 和 JPG/JPEG 由输出路径扩展名选择。

```lua
local pixels = string.rep(string.char(255, 0, 0), 100 * 100)
assert(stb_image.write("red.jpg", 100, 100, pixels, 90))
```
