---
params: "filename: string, img: Mat userdata, params?: table"
returns: "boolean"
---

**方法名称：** 将 Mat 写入图片文件。

**语法：** `cv.imwrite(filename, img[, params])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `filename` | `string` | 是 | 输出路径，使用 UTF-8；扩展名决定编码格式。 |
| `img` | `Mat userdata` | 是 | 要保存的图片矩阵。 |
| `params` | `table` | 否 | OpenCV 编码参数数组；不需要时省略。 |

| 返回值 | 说明 |
|---|---|
| `boolean` | 写入成功返回 `true`，失败返回 `false` 或抛出 OpenCV 错误。 |

```lua
local cv = require("opencv")
local image = cv.imread("输入图片.png", cv.IMREAD_COLOR)
assert(image and not image:empty())
assert(cv.imwrite("输出图片.jpg", image))
```

**详细说明：**

输出格式通常由文件扩展名决定；写入前应确认 Mat 非空且通道类型被目标编码器支持。
