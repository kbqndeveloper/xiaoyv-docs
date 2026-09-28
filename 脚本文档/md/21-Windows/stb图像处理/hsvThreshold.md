---
params: "input: string, output: string, h: number, s: number, v: number, hL: number, sL: number, vL: number"
returns: "boolean"
---

**方法名称：** 按 HSV 范围生成黑白图片。

**语法：** `stb_image.hsvThreshold(input, output, h, s, v, hL, sL, vL)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `input` | `string` | 是 | 输入图片路径。 |
| `output` | `string` | 是 | 输出图片路径；扩展名决定格式。 |
| `h` / `s` / `v` | `number` | 是 | HSV 目标值；H 使用 `0~360`，S/V 使用 `0~100`。 |
| `hL` / `sL` / `vL` | `number` | 是 | 对应通道容差。H 按色环距离比较。 |

| 返回值 | 说明 |
|---|---|
| `boolean` | 处理并写入成功返回 `true`，读取或写入失败返回 `false`。 |

**详细说明：**

匹配目标范围的像素输出白色 `0xffffff`，其余输出黑色 `0x000000`。色相差值超过 180 度时按色环另一侧计算距离；输出质量固定为 100。

```lua
assert(stb_image.hsvThreshold(
    "input.png", "mask.png", 120, 80, 70, 15, 20, 20
))
```
