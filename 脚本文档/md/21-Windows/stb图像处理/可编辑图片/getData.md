---
params: "无"
returns: "address, byteCount"
---

**方法名称：** 获取图片点阵的临时借用地址。

**语法：** `image:getData()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | — | — | 在打开的图片对象上调用。 |

| 返回值 | 说明 |
|---|---|
| `address` | RGB24 点阵地址，不复制数据。 |
| `byteCount` | 点阵字节数，等于 `width * height * 3`。 |

**详细说明：**

地址属于图片对象，脚本不需要也不能自行释放。使用地址期间必须保持 `image` 的强引用，不能调用
`image:close()`；对象关闭或被 GC 后地址立即失效。需要脱离对象长期保存时使用 [getPixels](getPixels.md)。

```lua
local image = assert(stb_image.load("input.png"))
local address, byteCount = image:getData()
print(address, byteCount)
-- 在这里完成一次需要该地址的 FFI 调用
image:close()
```
