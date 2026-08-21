---
params: "path: string, quality: integer?"
returns: "boolean"
---

**方法名称：** 保存当前图片点阵。

**语法：** `image:save(path, quality)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `path` | `string` | 是 | 输出路径；扩展名决定 BMP、PNG 或 JPG/JPEG。 |
| `quality` | `integer` | 否 | JPG/JPEG 质量，默认 `100`，自动限制到 `1~100`。 |

| 返回值 | 说明 |
|---|---|
| `boolean` | 写入成功返回 `true`，格式、路径或点阵无效返回 `false`。 |

**详细说明：**

保存读取对象当前内存中的 RGB24 点阵，不会自动关闭对象。PNG、BMP 忽略质量参数。

```lua
local image = assert(stb_image.load("input.png"))
image:setPixel(0, 0, 0xffff00)
assert(image:save("output.jpg", 90))
image:close()
```
