---
params: "Mat userdata"
returns: "布尔值、Mat userdata、数值或属性值"
---

**方法名称：** OpenCV 图片矩阵对象。

**语法：**

```lua
cv.Mat()
cv.Mat(rows, cols, type[, data[, step]])
```

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `rows` | `integer` | 使用带尺寸构造时必填 | 矩阵高度。 |
| `cols` | `integer` | 使用带尺寸构造时必填 | 矩阵宽度。 |
| `type` | `integer` | 使用带尺寸构造时必填 | OpenCV 矩阵类型，例如 `cv.CV_8UC4`。 |
| `data` | 原生地址 | 否 | 外部像素数据地址；可传 `dm.GetScreenData` 返回的整数地址。传入后只引用外部内存，不复制。 |
| `step` | `integer` | 否 | 每行字节数；省略时按连续矩阵计算。 |

| 返回值 | 说明 |
|---|---|
| `Mat userdata` | OpenCV 图片矩阵对象。 |

**详细说明：**

`cv.imread`、`cv.cvtColor` 等函数返回 `Mat userdata`。对象由 Lua 持有，离开引用后由运行时回收；需要独立副本时使用 `:clone()`。

## 从大漠屏幕内存创建 Mat

本例在同一个 x86 Worker 中同时加载大漠和 x86 OpenCV；地址不能直接传到 x64 Worker 使用。

`dm.GetScreenData(x1, y1, x2, y2)` 返回指定区域的像素地址。大漠点阵格式为 `0x00RRGGBB`，每个像素占 4 字节；在 Windows 小端内存中按 `BGRA` 顺序交给 OpenCV 使用。

```lua
local cv = require("opencv")
local dm = require("xiaoyv_hcbyj")

local x1, y1 = 100, 100
local x2, y2 = 800, 600
local width = x2 - x1 + 1
local height = y2 - y1 + 1

local ptr = dm.GetScreenData(x1, y1, x2, y2)
assert(ptr ~= 0, "获取屏幕数据失败")

-- 直接引用大漠内存，不复制；每行占 width * 4 字节。
local image = cv.Mat(height, width, cv.CV_8UC4, ptr, width * 4)

-- 例如转换为灰度图。
local gray = cv.cvtColor(image, cv.COLOR_BGRA2GRAY)
```

上例中的 `image` 只持有大漠点阵的引用，不拥有这块内存；只应在该屏幕数据有效期间使用。需要保存、异步处理或长期持有时，先调用 `local copy = image:clone()` 创建独立副本。

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

常用属性包括 `rows`、`cols`、`dims`、`channels` 和 `data`。`data` 是原生地址，不能在 Mat 释放后继续使用，也不建议直接交给不理解 OpenCV 生命周期的外部代码。

```lua
local cv = require("opencv")
local image = cv.imread("input.png", cv.IMREAD_COLOR)
assert(image and not image:empty())

local copy = image:clone()
print(copy.rows, copy.cols, copy:channels())
```
