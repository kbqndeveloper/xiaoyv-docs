---
params: "points: table"
returns: "integer"
---

**方法名称：** 批量修改图片像素。

**语法：** `image:setPixels(points)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `points` | `table` | 是 | 数组项格式为 `{x, y, rgb}`，`rgb` 为 `0xRRGGBB`。 |

| 返回值 | 说明 |
|---|---|
| `integer` | 实际成功修改的像素数量；越界项不计数。 |

**详细说明：**

数组项不是 table 或缺少有效整数时会抛出 Lua 错误；坐标越界和颜色超范围的项只是不计入返回值。
修改直接作用于内存点阵，需另行调用 `save`。

```lua
local image = assert(stb_image.load("input.png"))
local changed = image:setPixels({
    {10, 20, 0xffff00},
    {11, 20, 0xffff00},
})
print("changed", changed)
image:close()
```
