---
params: ""
returns: "version"
---

**方法名称：** 读取 VMmemory 接口版本。

**语法：** `vm.abi_version()`

**参数说明：**

无。

| 返回值 | 说明 |
|---|---|
| `version` | 当前插件 C ABI 版本整数；当前为 `1`。 |

**使用示例：**

```lua
local vm = require("VMmemory")
print(vm.abi_version())
```

**详细说明：**

该值表示插件接口的 ABI 版本，用于核对接口兼容性，不代表插件的发布版本。
