---
params: "Mat userdata"
returns: "布尔值、Mat userdata、数值或属性值"
---

**方法名称：** OpenCV 图片矩阵对象。

**语法：** `cv.Mat()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | — | — | 本页说明由其他 OpenCV 函数返回的 Mat 对象；空 Mat 可使用 `cv.Mat()` 创建。 |

| 返回值 | 说明 |
|---|---|
| `Mat userdata` | OpenCV 图片矩阵对象。 |

**详细说明：**

`cv.imread`、`cv.cvtColor` 等函数返回 `Mat userdata`。对象由 Lua 持有，离开引用后由运行时回收；
需要独立副本时使用 `:clone()`。

## 常用方法

| 方法 | 说明 |
|---|---|
| `image:empty()` | 判断矩阵是否为空。 |
| `image:clone()` | 创建独立的 Mat 副本。 |
| `image:size()` | 返回 `{width, height}`。 |
| `image:channels()` | 返回通道数。 |
| `image:type()` | 返回 OpenCV 矩阵类型编号。 |
| `image:at(row, col)` | 读取元素；多通道或不同深度可使用生成绑定中的专用 `*_at` 方法。 |
| `image:set_at(value, row, col)` | 写入单通道元素。 |

常用属性包括 `rows`、`cols`、`dims`、`channels` 和 `data`。`data` 是原生地址，不能在 Mat
释放后继续使用，也不建议直接交给不理解 OpenCV 生命周期的外部代码。

```lua
local cv = require("opencv")
local image = cv.imread("input.png", cv.IMREAD_COLOR)
assert(image and not image:empty())

local copy = image:clone()
print(copy.rows, copy.cols, copy:channels())
```
