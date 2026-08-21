---
params: "path1: string, path2: string, output: string, x: integer, y: integer, method: integer"
returns: "boolean"
---

**方法名称：** 合并两张图片。

**语法：** `stb_image.merge(path1, path2, output, x, y, method)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `path1` | `string` | 是 | 第一张图片路径。 |
| `path2` | `string` | 是 | 第二张图片路径。 |
| `output` | `string` | 是 | 输出图片路径。 |
| `x` / `y` | `integer` | 是 | 第二张图片相对第一张图片的偏移；可为负数。 |
| `method` | `integer` | 是 | `1`：在第一张图右侧合并；`2`：在第一张图下方合并。 |

| 返回值 | 说明 |
|---|---|
| `boolean` | 合并并写入成功返回 `true`，读取、参数或输出格式失败返回 `false`。 |

**详细说明：**

`method` 为 `1` 时实际横向偏移为 `x + path1.width`；为 `2` 时实际纵向偏移为
`y + path1.height`。输出范围会覆盖两张图片及负偏移产生的区域，输出格式由扩展名决定。

```lua
assert(stb_image.merge("left.png", "right.png", "all.png", 0, 0, 1))
```
