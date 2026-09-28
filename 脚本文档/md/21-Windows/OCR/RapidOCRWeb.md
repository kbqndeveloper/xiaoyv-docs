---
params: "imagePath: string, serviceUrl: string?"
returns: "string"
---

# RapidOCRWeb OCR

RapidOCRWeb 是独立运行的本机 OCR 服务，不属于 Windows Worker 内置模块。Worker 直接通过现有 `httpPost` 调用它，因此项目源码和发布目录不需要加入 OCR 模型或额外 DLL。

## 下载和启动

- [RapidOCRWeb 最新发布页](https://github.com/RapidAI/RapidOCRWeb/releases/latest)
- [Windows v1.0.0 免安装版直接下载](https://github.com/RapidAI/RapidOCRWeb/releases/download/v1.0.0/RapidOCRWeb-3.1.1-windows-2022-X64-no-installer.zip)
- [官方桌面版使用说明](https://rapidai.github.io/RapidOCRDocs/main/install_usage/rapidocr_web/desktop_usage/)

下载并解压后运行 `RapidOCRWeb.exe`。服务默认监听 `9003` 端口，浏览器访问 `http://127.0.0.1:9003/` 能打开页面即表示启动成功。

## Lua 调用

下面的封装读取图片、转为 Base64，再向默认的 `/ocr` 接口提交 JSON。返回值是 RapidOCRWeb 返回的原始 JSON 文本，脚本可按自己的 JSON 库继续解析。

```lua
local mime = require("mime")

local function rapidOCR(imagePath, serviceUrl)
    local file = assert(io.open(imagePath, "rb"))
    local imageData = file:read("*a")
    file:close()

    local postData = '{"file":"data:image/png;base64,' .. mime.b64(imageData) .. '"}'
    local body, status = httpPost(
        serviceUrl or "http://127.0.0.1:9003/ocr",
        postData,
        60,
        { ["Content-Type"] = "application/json; charset=utf-8" }
    )

    assert(body, status)
    assert(status == 200, "RapidOCRWeb 请求失败：" .. tostring(status) .. "\n" .. body)
    return body
end

local resultJson = rapidOCR("截图.png")
print(resultJson)
```

当前发布版的请求体上限为 3 MiB；图片较大时应先裁剪或压缩。RapidOCRWeb 的接口格式可能随版本变化，升级外部程序后如果调用失败，应先核对其对应版本的接口说明。
