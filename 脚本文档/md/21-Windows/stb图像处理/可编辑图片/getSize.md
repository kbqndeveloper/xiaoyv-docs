---
params: "无"
returns: "width, height"
---

**方法名称：** 获取图片宽高。

**语法：** `image:getSize()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | — | — | 在 `stb_image.load` 返回的图片对象上调用。 |

| 返回值 | 说明 |
|---|---|
| `width, height` | 当前图片宽度和高度。 |

**详细说明：**

图片已经 `close` 后调用会抛出 Lua 错误；也可以直接读取只读字段 `image.width` 和 `image.height`。

```lua
local image = assert(stb_image.load("input.png"))
local width, height = image:getSize()
print(width, height)
image:close()
```
