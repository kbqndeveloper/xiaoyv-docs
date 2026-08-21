---
params: "input: string, output: string, quality: integer"
returns: "boolean"
---

**方法名称：** 转换图片格式。

**语法：** `stb_image.convert(input, output, quality)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `input` | `string` | 是 | 输入图片路径。 |
| `output` | `string` | 是 | 输出图片路径；支持 BMP、PNG 和 JPG/JPEG。 |
| `quality` | `integer` | 是 | JPG/JPEG 质量，自动限制到 `1~100`。 |

| 返回值 | 说明 |
|---|---|
| `boolean` | 转换并写入成功返回 `true`，否则返回 `false`。 |

**详细说明：**

转换过程先将输入解码为 RGB24，再按输出扩展名写入，不保留 alpha 通道。

```lua
assert(stb_image.convert("input.bmp", "output.jpg", 90))
```
