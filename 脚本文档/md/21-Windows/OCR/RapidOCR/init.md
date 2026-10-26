---
params: "config?: string|XiaoYvOcrPresetConfig（table）|XiaoYvOcrModelConfig（table）"
returns: "true 或 nil, error: string"
---

**方法名称：** 初始化或切换 OCR 模型。

**语法：** `ocr.init([config])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `config` | `string\|XiaoYvOcrPresetConfig（table）\|XiaoYvOcrModelConfig（table）` | 否 | 省略为 `server`；字符串可为 `mobile` 或 `server`，表字段见下方。 |

**类型：** `XiaoYvOcrPresetConfig`（table）

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `preset` | `"mobile"\|"server"` | 否 | 预设名，默认 `server`。 |
| `threads` | `integer` | 否 | 推理线程数，默认 `2`。 |

**类型：** `XiaoYvOcrModelConfig`（table）

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `det` | `string` | 是 | 自定义检测 ONNX 文件路径。 |
| `rec` | `string` | 是 | 自定义识别 ONNX 文件路径。 |
| `keys` | `string` | 是 | 与识别模型匹配的 UTF-8 字典文件，每行一个字符。 |
| `cls` | `string` | 否 | 自定义方向分类 ONNX 文件路径。 |
| `threads` | `integer` | 否 | 推理线程数，默认 `2`。 |

| 返回值 | 说明 |
|---|---|
| `true` | 模型已加载。 |
| `nil, error` | 加载失败及错误说明。 |

```lua
local ocr = require("rapidocr")
assert(ocr.init())
assert(ocr.init("mobile"))
assert(ocr.init({preset = "server", threads = 2}))

-- 使用自定义模型时改用此配置
assert(ocr.init({
    det = "det.onnx",
    rec = "rec.onnx",
    keys = "keys.txt",
    threads = 2
}))
```

**详细说明：**

调用后立即加载模型；同一配置重复调用复用已加载的模型。切换配置先释放旧模型，加载失败后保留新配置，后续识别会重试该配置。

自定义模型需符合 PP-OCR 的 ONNX 浮点 NCHW 输入和 CTC 识别输出约定，并配套正确字典；不能直接使用任意 OCR 模型。路径为 UTF-8 普通文件路径。预设自带字典信息，无需单独指定 `keys`。
