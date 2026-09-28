---
params: "className: string, loader: userdata?"
returns: "userdata"
---

**方法名称：** 解析 Java 类或包。

**语法：** `import(className[, loader])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `className` | `string` | 是 | 完整类名，或以 `.*` 结尾的完整包名。 |
| `loader` | Java userdata | 否 | `ClassLoader` 或具有 `loadClass(String)` 方法的对象。 |

| 返回值 | 说明 |
|---|---|
| Java Class userdata | 完整类名解析结果，可读取静态成员或直接调用构造函数。 |
| JavaPackage userdata | 包通配解析结果；按字段名延迟解析并缓存类。 |

```lua
local Math = import("java.lang.Math")
local util = import("java.util.*")

print(Math.sin(1.2))
local values = util.ArrayList()
```

**详细说明：**

`import` 不写入 `_G`，也不修改 `_G` 的元表。不同包的同名类由各自代理隔离：

```lua
local util = import("java.util.*")
local sql = import("java.sql.*")

print(util.Date, sql.Date)
```

包代理是只读对象，类首次访问成功后缓存在代理内部。完整类名和包代理都可以使用显式加载源：

```lua
local LuaEngine = import("com.nx.assist.lua.LuaEngine")
local loader = assert(LuaEngine.loadApk("plugin.apk"))
local Plugin = import("com.example.Plugin", loader)
```

支持静态/实例字段、方法、构造函数、公开内部类、重载、数组、集合和接口代理。Java 异常转换为 Lua 错误；Java `null` 对应 `nil`，`void` 方法没有返回值。OpenCV 新脚本直接使用全局 [`opencv`](../10-图像/OpenCV/概述.md)，无需 import。
