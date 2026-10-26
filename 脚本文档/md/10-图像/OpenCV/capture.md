---
params: "x1, y1, x2, y2: integer"
returns: "mat: userdata"
---

**方法名称：** 获取 OpenCV `Mat` 截图。

**语法：** `opencv.capture(x1, y1, x2, y2)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `x1, y1` | `integer` | 是 | 区域一个端点。 |
| `x2, y2` | `integer` | 是 | 区域另一个端点；边界包含在结果内。 |

四个坐标均为 `0` 时表示完整当前画面。

| 返回值 | 说明 |
|---|---|
| `userdata` | 独立的 RGBA `org.opencv.core.Mat`。 |

**详细说明：**

该函数与取色、找图共用当前截图缓存，因此锁帧和固定图片屏幕同样生效。区域越界、截图无效或 OpenCV 运行时加载失败时抛出 Lua 错误。返回的 `Mat` 由脚本持有，使用完后必须调用 `release()`。

```lua
local image = opencv.capture(0, 0, 31, 31)
print(image:cols(), image:rows()) -- 32, 32
image:release()
```
