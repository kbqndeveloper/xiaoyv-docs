---
params: "path: string"
returns: "width, height, pixels 或 0, 0, \"\""
---

**方法名称：** 读取图片为 RGB24 二进制字符串。

**语法：** `stb_image.read(path)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `path` | `string` | 是 | UTF-8 图片路径。 |

| 返回值 | 说明 |
|---|---|
| `width, height, pixels` | 宽、高和 RGB24 点阵字符串，长度为 `width * height * 3`。 |
| `0, 0, ""` | 读取或解码失败。 |

**详细说明：**

该函数返回的是一次复制后的二进制快照，不会创建可编辑图片对象。需要连续修改像素时使用 [stb_image.load](load.md)；点阵顺序按行排列，坐标 `(x, y)` 的偏移为 `(y * width + x) * 3`。

```lua
local width, height, pixels = stb_image.read("input.png")
assert(width > 0 and height > 0)
print(width, height, #pixels)
```
