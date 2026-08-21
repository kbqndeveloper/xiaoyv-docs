---
params: "无"
returns: "boolean"
---

**方法名称：** 关闭图片并释放点阵。

**语法：** `image:close()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | — | — | 在图片对象上调用。 |

| 返回值 | 说明 |
|---|---|
| `true` | 本次调用释放了仍打开的图片。 |
| `false` | 图片此前已经关闭。 |

**详细说明：**

关闭是幂等的；关闭后不能再读取、修改或保存，`getData` 取得的借用地址也立即失效。若不主动
关闭，Lua GC 会自动释放 userdata 持有的点阵。

```lua
local image = assert(stb_image.load("input.png"))
assert(image:close())
assert(not image:close())
```
