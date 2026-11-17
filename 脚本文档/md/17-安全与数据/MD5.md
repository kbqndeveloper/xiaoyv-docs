---
params: "见下方方法表"
returns: "见下方方法表"
---

**方法名称：** MD5 摘要（Android / Windows）。

**语法：** 见下方方法表。

**参数说明：** 使用 `local md5 = require("md5")` 加载内置模块，无需安装插件；`data` 为待计算的字符串，支持包含零字节的二进制数据。

| 方法 | 参数 | 返回值 |
|---|---|---|
| `md5.sumhexa(data: string)` | 计算便于显示和比较的十六进制摘要 | hex: string |
| `md5.sum(data: string)` | 计算原始二进制摘要 | digest: string |

| 返回值 | 说明 |
|---|---|
| hex | 32 位小写十六进制字符串 |
| digest | 16 字节的二进制字符串，不是可直接显示的文本 |

**详细说明：**

按输入字符串的实际字节计算，不转换文本编码。计算文件摘要时先读取文件内容，再传给上述函数；传入文件路径只会计算路径字符串的摘要。

```lua
local md5 = require("md5")

print(md5.sumhexa("hello")) -- 5d41402abc4b2a76b9719d911017c592
local digest = md5.sum("hello")
print(#digest) -- 16
```
