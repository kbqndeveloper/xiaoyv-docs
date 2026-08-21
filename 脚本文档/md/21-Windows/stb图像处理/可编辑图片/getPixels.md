---
params: "无"
returns: "pixels: string"
---

**方法名称：** 复制当前图片的 RGB24 点阵。

**语法：** `image:getPixels()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | — | — | 在打开的图片对象上调用。 |

| 返回值 | 说明 |
|---|---|
| `pixels` | RGB24 二进制字符串，长度为 `image.width * image.height * 3`。 |

**详细说明：**

每次调用都会复制完整点阵，返回值与图片对象生命周期无关；需要高频读取时不要反复复制，可考虑
使用 [getData](getData.md) 的借用地址并在使用期间保持图片对象有效。

```lua
local image = assert(stb_image.load("input.png"))
local pixels = image:getPixels()
print(#pixels, image.width * image.height * 3)
image:close()
```
