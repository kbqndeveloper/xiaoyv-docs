---
params: "text: string"
returns: "string"
---

# U2G 和 G2U

**方法名称：** 在 UTF-8 和 GBK 之间转换文本。

**语法：**

```lua
U2G(utf8Text)
G2U(gbkText)
```

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `utf8Text` | `string` | 是 | UTF-8 字节字符串。 |
| `gbkText` | `string` | 是 | GBK（代码页 936）字节字符串。 |

| 返回值 | 说明 |
|---|---|
| `string` | 转换后的 Lua 字符串。 |

**详细说明：**

`U2G` 把 UTF-8 Lua 字符串转为代码页 936（GBK），`G2U` 执行反向转换。
它们是 Windows 旧脚本和旧 DLL/文本接口的兼容函数，不属于 `m`。无效编码会抛出 Lua 错误。

框架自带的 `io` 文件接口、`os.remove/os.rename`、Lua 模块搜索、`package.loadlib`、`ffi.load`
和 `stb_image` 路径均直接使用 UTF-8，不需要先调用 `U2G()`。大漠及其他只接收 ANSI 的旧 DLL
仍由脚本按接口要求自行转换。`io.popen/os.execute` 传入的是命令行，不属于文件路径转换范围。
