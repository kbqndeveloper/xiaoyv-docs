---
params: "path: string"
returns: "image userdata 或 nil, error"
---

**方法名称：** 加载可编辑图片对象。

**语法：** `stb_image.load(path)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `path` | `string` | 是 | 图片路径，使用 UTF-8；格式由 stb_image 支持。 |

| 返回值 | 说明 |
|---|---|
| `image userdata` | 图片已解码为 RGB24 点阵，可继续调用对象方法。 |
| `nil, error` | 文件不存在、格式不支持或解码失败。 |

**详细说明：**

图片对象提供 `width`、`height` 字段以及读取、修改和保存方法。对象由 Lua userdata 持有，未主动
调用 `close` 时也会在 GC 时自动释放；关闭后继续访问会抛出 Lua 错误。

```lua
local image, error = stb_image.load("input.jpg")
if not image then
    error(error)
end
print(image.width, image.height)
```
