---
params: "config?: string|XiaoYvOcrPresetConfig（table）|XiaoYvOcrModelConfig（table）"
returns: "true 或 nil, error: string"
---

**方法名称：** 加载预设或自定义 OCR 模型。

**语法：** `ocr.init([config])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `config` | `string\|XiaoYvOcrPresetConfig（table）\|XiaoYvOcrModelConfig（table）` | 否 | 省略为 `server`；字符串可为 `mobile`、`server` 或 `english`，加载到名称 `default`；表字段见下方。 |

**类型：** `XiaoYvOcrPresetConfig`（table）

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `name` | `string` | 否 | 模型在当前 Lua VM 中的名称，默认 `"default"`；识别时通过 `options.model` 选择。 |
| `preset` | `"mobile"\|"server"\|"english"` | 否 | 模型预设，默认 `server`；与模型名称无关。 |
| `threads` | `integer` | 否 | 推理线程数，默认 `2`。 |

**类型：** `XiaoYvOcrModelConfig`（table）

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `name` | `string` | 否 | 模型在当前 Lua VM 中的名称，默认 `"default"`。 |
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
assert(ocr.init({name = "en", preset = "english", threads = 2}))
local text, score = ocr.readLine("英文.png", {model = "en"})
assert(text, score)
print(text, score)

-- 更换默认模型需要先释放；命名模型 en 仍保留
ocr.release()
assert(ocr.init("mobile"))

-- 自定义模型也可使用独立名称
assert(ocr.init({
    name = "custom",
    det = "det.onnx",
    rec = "rec.onnx",
    keys = "keys.txt",
    threads = 2
}))
```

**详细说明：**

调用后立即加载模型，不只是设置后续识别参数。`name` 是调用方给模型取的名称，`preset` 决定加载哪套模型文件；例如 `{name = "en", preset = "english"}` 加载名称为 `en` 的英文模型，不改变默认模型。

同名、相同配置重复调用会复用；同名、不同配置返回 `nil, error`，保留原模型，需要先调用 [release(name)](release.md)。不同名称可以同时保留；模型路径和线程数相同的配置共享同一份模型，最后一个名称释放后才释放模型资源。共享仅限当前 Lua VM。

模型文件缺失或加载失败返回 `nil, error`，不会改用其他模型。为 `default` 加载新配置失败时保留该配置，后续默认识别会重试；其他名称加载失败后须重新 `init`。只有 `default` 支持首次识别自动加载，初始配置为 `server`。

自定义模型需符合 PP-OCR 的 ONNX 浮点 NCHW 输入和 CTC 识别输出约定，并配套正确字典；不能直接使用任意 OCR 模型。路径为 UTF-8 普通文件路径。预设自带字典信息，无需单独指定 `keys`。
